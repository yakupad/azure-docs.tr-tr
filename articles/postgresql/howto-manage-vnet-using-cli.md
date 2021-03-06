---
title: Oluşturma ve PostgreSQL sanal ağ hizmet uç noktaları ve Azure CLI kullanarak kuralları için Azure veritabanı'nı yönetme | Microsoft Docs
description: Bu makalede, oluşturmak ve PostgreSQL sanal ağ hizmet uç noktaları ve Azure CLI komut satırını kullanarak kurallar için Azure veritabanı yönetmek açıklar.
services: postgresql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 7312000d1f22af3eb0091b46caac2c9607231513
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38531710"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-using-azure-cli"></a>Oluşturma ve Azure CLI kullanarak PostgreSQL sanal ağ hizmet uç noktaları için Azure veritabanı'nı yönetme
Sanal ağ (VNet) Hizmetleri uç noktaları ve kuralları PostgreSQL için Azure veritabanı sunucunuza sanal ağ özel adres alanını genişletin. Uygun Azure komut satırı arabirimi (CLI) komutlarını kullanarak, oluşturabilir, güncelleştirme, silme, liste ve sanal ağ hizmet uç noktaları ve sunucunuzu yönetmek için kuralları göster. Sınırlamalar da dahil olmak üzere PostgreSQL sanal ağ hizmet uç noktaları için Azure veritabanı'nın genel bir bakış için bkz. [PostgreSQL sunucusu sanal ağ hizmet uç noktaları için Azure veritabanı](concepts-data-access-and-security-vnet.md). Sanal ağ hizmet uç noktalarının genel önizlemede tüm desteklenen bölgeler için Azure veritabanı PostgreSQL için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Cloud Shell kullanın.
- Bir [PostgreSQL sunucusu ve veritabanı için Azure veritabanı](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için sanal ağ hizmet uç noktalarını yapılandırma
[Az ağ vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) komutları, sanal ağları yapılandırmak için kullanılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü görmek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

CLI’yi yerel olarak çalıştırıyorsanız, [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) komutunu kullanarak hesabınızda oturum açmanız gerekir. Komut çıktısındaki ilgili abonelik adına karşılık gelen **id** özelliğinin değerini not edin.
```azurecli-interactive
az login
```
CLI uzantısı için Azure veritabanı kullanarak PostgreSQL sanal ağ hizmet uç noktaları için yükleme `az extension add --name rdbms-vnet` komutu. 
```azurecli-interactive
az extension add --name rdbms-vnet
```

Çalıştırma `az extension list` CLI uzantısını yükleme doğrulamak için komutu.
```azurecli-interactive
az extension list
```
Komut çıktısı, yüklenen tüm uzantıları listeler. PostgreSQL CLI uzantısı için Azure veritabanı şöyledir:

 {"extensionType": "whl", "name": "ağa rdbms", "Sürüm": "10.0.0"}

> [!NOTE]
> Çalıştırma CLI uzantısını kaldırmak için `az extension remove -n rdbms-vnet` komutu. 

Birden fazla aboneliğiniz varsa kaynağın faturalanacağı uygun aboneliği seçin. [az account set](/cli/azure/account#az_account_set) komutunu kullanarak hesabınız altındaki belirli bir abonelik kimliğini seçin. Aboneliğinizin **az login** çıktısındaki **id** özelliğini abonelik kimliği yer tutucusuyla değiştirin.

- Hesabın, bir sanal ağ ve hizmet uç noktası oluşturmak için gerekli izinleri olmalıdır.

Hizmet uç noktaları sanal ağlarda birbirinden bağımsız olarak, sanal ağda yazma erişimine sahip bir kullanıcı tarafından yapılandırılabilir.

Azure hizmet kaynaklarını bir sanal ağ ile sınırlamak için kullanıcının eklenen alt ağlarda "Microsoft.Network/JoinServicetoaSubnet" iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.

[Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı Aboneliklerde olması halinde, kaynakların bu Önizleme sırasında aynı Active Directory (AD) kiracısı altında olması gerekir.

> [!IMPORTANT]
> Aşağıdaki örnek komut dosyasını çalıştırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okunacak önemle tavsiye edilir ya da hizmet uç noktalarını yapılandırma. **Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Sanal ağ hizmet uç noktalarını kullanan hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder. Bu hizmet etiketi, hizmetleri, PostgreSQL ve MySQL için Azure veritabanı Azure SQL veritabanı için de geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi isteğe bağlı olarak bir sanal ağ hizmet uç noktası için Azure SQL veritabanı ve PostgreSQL için Azure veritabanı dahil olmak üzere tüm Azure veritabanı hizmetleri için hizmet uç noktası trafiğini yapılandırır ve Alt ağdaki MySQL Server için Azure veritabanı. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>PostgreSQL veritabanı için Azure veritabanı oluşturma, sanal ağ, sanal ağ hizmet uç noktası oluşturma ve güvenli bir sanal ağ kuralı olan bir alt ağ için sunucu komut dosyası örneği
Bu örnek betikte, vurgulanan satırları değiştirerek yönetici kullanıcı adını ve parolasını özelleştirin. Kullanılan Subscriptionıd yerine `az account set --subscription` kendi abonelik tanımlayıcısı ile komutu.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]
