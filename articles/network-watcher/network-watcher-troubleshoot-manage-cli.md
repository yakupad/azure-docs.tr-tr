---
title: Azure sanal ağ geçidi ve bağlantı - Azure CLI 2.0 sorunlarını giderme | Microsoft Docs
description: Bu sayfa, Azure Ağ İzleyicisi'ni kullanmayı açıklar Azure CLI 2.0 sorun giderme
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: 5f843b42a108968e2fbefacddcd22f331a04691e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091110"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a>Sanal ağ geçidi ve Azure Ağ İzleyicisi Azure CLI 2.0 kullanarak bağlantı sorunlarını giderme

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ İzleyicisi, ağ kaynaklarınıza azure'da anlamak için bağlantılı olarak çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorunlarını giderme. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API çağrılabilir. Çağrıldığında, Ağ İzleyicisi, bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulguları döndürür.

Bu makalede, Windows, Mac ve Linux için kullanılabilir olduğu kaynak yönetimi dağıtım modeli için Azure CLI 2. 0'da, sunduğumuz yeni nesil CLI kullanılmıştır.

Bu makaledeki adımları gerçekleştirmek için yapmanız [Azure komut satırı arabirimi için Mac, Linux ve Windows (Azure CLI) yükleme](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo, zaten uyguladığınız adımları varsayar [Ağ İzleyicisi oluşturma](network-watcher-create.md) Ağ İzleyicisi oluşturmak için.

Desteklenen ağ geçidi türleri ziyaret listesini [desteklenen ağ geçidi türleri](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Genel Bakış

Kaynak sorunlarını giderme özelliği sağlar bağlantı ile sanal ağ geçitleri ile ortaya çıkan sorunları giderme. Kaynak sorunlarını gidermek için bir istek yapıldığında, günlükleri gönderildiğini sorgulanabilir ve inceledi. İnceleme işlemi tamamlandıktan sonra sonuçlar döndürülür. Sorun giderme isteği uzun kaynak, hangi birden çok dakika sürebilir bir sonuç döndürmek için ister. Sorun giderme gelen günlükler, belirtilen depolama hesabında bir kapsayıcıda depolanır.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Bir sanal ağ ağ geçidi bağlantısı

Bu örnekte, kaynak sorun giderme olan çalıştığı bir bağlantıda. Ayrıca bir sanal ağ geçidi geçirebilirsiniz. Aşağıdaki cmdlet'i bir kaynak grubundaki vpn bağlantıları listeler.

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

Bağlantının adını aldıktan sonra kendi kaynak kimliğini almak için şu komutu çalıştırabilirsiniz:

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kaynak durumu hakkında daha fazla veri kaynağı sorunlarını giderme döndürür, ayrıca gözden geçirilmesi için bir depolama hesabı günlükleri kaydeder. Bu adımda, bir depolama hesabı oluşturuyoruz, mevcut bir depolama hesabı zaten varsa bunu kullanabilirsiniz.

1. Depolama hesabı oluşturma

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. Depolama hesabı anahtarları alma

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. Kapsayıcı oluşturma

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Kaynak Ağ İzleyicisi sorun giderme çalıştırın

Kaynakları ile ilgili sorunları giderme `az network watcher troubleshooting` cmdlet'i. Biz cmdlet'ini kaynak grubu, Ağ İzleyicisi, depolama hesabı kimliği bağlantının kimliği adını geçirebilir ve sorun giderme depolamak için blob yolu sonuçlanır.

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

Cmdlet'i çalıştırdıktan sonra Ağ İzleyicisi sistem durumu doğrulamak için kaynak inceler. Shell sonuçları döndürür ve belirtilen depolama hesabında günlükleri sonuçlarını depolar.

## <a name="understanding-the-results"></a>Sonuçlarını anlama

Eylem metin sorunu hakkında genel rehberlik sağlar. Bir eylem sorununu gerçekleştirilebiliyorsa bir bağlantı ile ek yönergeler sağlanır. Bu durumda hiçbir ek yönergeler olduğunda, bir destek olayı açmak için URL'yi isteğin yanıtını verir.  Yanıt ve dahil edilen nedir özellikleri hakkında daha fazla bilgi için ziyaret [Ağ İzleyicisi sorun giderme genel bakış](network-watcher-troubleshoot-overview.md)

Azure depolama hesaplarından dosyaları indirme ile ilgili yönergeler için başvurmak [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilen başka bir Depolama Gezgini aracıdır. Aşağıdaki bağlantıda Depolama Gezgini hakkında daha fazla bilgi burada bulunabilir: [Depolama Gezgini](http://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

Ayarları durdurma VPN bağlantısının değiştirilmiş olup [ağ güvenlik grupları yönet](../virtual-network/manage-network-security-group.md) söz konusu olabilecek ağ güvenlik grubu ve güvenlik kuralları izlemek için.
