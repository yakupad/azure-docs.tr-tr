---
title: Ekleme, değiştirme veya bir Azure sanal ağ alt ağını silmek | Microsoft Docs
description: Ekleme, değiştirme veya azure'da bir sanal ağ alt ağını silmek öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 26e01ccab3693c672130462104078c16526aa921
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991537"
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Ekleme, değiştirme veya bir sanal ağ alt ağı Sil

Ekleme, değiştirme veya bir sanal ağ alt ağını silmek öğrenin. Tüm Azure kaynaklarını bir sanal ağa dağıtılan bir sanal ağ içindeki bir alt ağa dağıtılır. Sanal ağlara yeniyseniz, bunları hakkında daha fazla bilgi edinebilirsiniz [sanal ağa genel bakış](virtual-networks-overview.md) veya tamamlayarak bir [öğretici](quick-create-portal.md). Oluşturmak için değiştirmek veya bir sanal ağı silmek, bkz: [sanal ağ yönetme](manage-virtual-network.md).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Azure hesabınız yoksa, kaydolmaya bir [ücretsiz deneme hesabınızı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici, Azure CLI Sürüm 2.0.31 gerektirir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure ile bir bağlantı oluşturmak için.

Oturum açın ya da Azure ile bağlandığınız hesabı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

## <a name="add-a-subnet"></a>Alt ağ ekleme

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden bir alt ağa eklemek istediğiniz sanal ağı seçin.
3. **AYARLAR** altında **Alt ağlar**’ı seçin.
4. Seçin **+ alt ağ**.
5. Aşağıdaki parametreler için değerleri girin:
    - **Ad**: ad, sanal ağ içinde benzersiz olmalıdır. Diğer Azure hizmetleriyle en büyük uyumluluk için bir harf adın ilk karakteri kullanmanızı öneririz. Örneğin, Azure Application Gateway, bir sayı ile başlayan bir ada sahip bir alt ağa dağıtılamaz.
    - **Adres aralığı**: sanal ağ adres alanı içinde benzersiz olmalıdır. Aralık ile diğer sanal ağ içindeki alt ağ adres aralıkları çakışamaz. Adres alanı, sınıfsız etki alanları arası yönlendirme (CIDR) gösterimi kullanılarak belirtilmelidir. Örneğin, adres alanı 10.0.0.0/16 ile bir sanal ağ içinde bir alt ağ adres alanı 10.0.0.0/24 tanımlayabilir. Belirtebileceğiniz en küçük /29, alt ağ için sekiz IP adreslerini sağlayan aralığındadır. Azure her alt ağda protokol uyumluluğu için ilk ve son adresi ayırır. Üç ek adresleri, Azure hizmet kullanımı için ayrılmıştır. Sonuç olarak, bir/29 alt ağı tanımlama adres alt ağda üç kullanılabilir IP adresi aralığı sonuçlanır. Bir sanal ağ VPN ağ geçidi bağlamayı planlıyorsanız, ağ geçidi alt ağı oluşturmanız gerekir. Daha fazla bilgi edinin [ağ geçidi alt ağları için belirli bir adres aralığı konuları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Alt ağ, belirli koşullar altında ekledikten sonra adres aralığını değiştirebilirsiniz. Bir alt ağ adres aralığı değiştirme konusunda bilgi edinmek için [alt ağ ayarları değiştirme](#change-subnet-settings).
    - **Ağ güvenlik grubu**: sıfır ya da alt ağı için gelen ve giden ağ trafiğini filtreleme için bir mevcut ağ güvenlik grubu bir alt ağa ilişkilendirebilirsiniz. Ağ güvenlik grubu, aynı abonelik ve konum sanal ağ mevcut olmalıdır. Daha fazla bilgi edinin [ağ güvenlik grupları](security-overview.md) ve [bir ağ güvenlik grubu oluşturma](tutorial-filter-network-traffic.md).
    - **Yol tablosu**: ağ trafiği diğer ağlara yönlendirme denetlemek için bir alt ağ için sıfır veya bir var olan yol tablosu ilişkilendirebilirsiniz. Rota tablosunu aynı abonelik ve konum sanal ağ mevcut olmalıdır. Daha fazla bilgi edinin [Azure yönlendirme](virtual-networks-udr-overview.md) ve [bir yol tablosu oluşturma](tutorial-create-route-table-portal.md)
    - **Hizmet uç noktalarını:** bir alt ağ için etkin sıfır veya daha fazla hizmet uç noktalarına sahip olabilir. Bir hizmet için hizmet uç noktası etkinleştirmek için hizmet veya hizmet uç noktalarından etkinleştirmek istediğiniz hizmetleri seçin **Hizmetleri** listesi. Konum, bir uç noktası için otomatik olarak yapılandırılır. Varsayılan olarak, hizmet uç noktaları sanal ağın bölgesi için yapılandırılır. Azure depolama için bölgesel yük devretme senaryolarını desteklemek amacıyla uç noktaları otomatik olarak için yapılandırılan [Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

    Hizmet uç noktasını kaldırmak için hizmet uç noktası için kaldırmak istediğiniz hizmeti seçimini kaldırın. Hizmet uç noktaları ve bunlar etkinleştirilebilir için hizmetler hakkında daha fazla bilgi için bkz. [sanal ağ hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md). Bir hizmet için hizmet uç noktası için etkinleştirdiğinizde, ayrıca hizmeti ile oluşturulan bir kaynak için alt ağ için ağ erişimini etkinleştirmeniz gerekir. Örneğin, hizmet uç noktası için etkinleştirirseniz *Microsoft.Storage*, ağ erişimi için ağ erişimi vermek istediğiniz tüm Azure depolama hesaplarına da etkinleştirmeniz gerekir. Hizmet uç noktası için etkin bir alt ağ erişimini etkinleştirme hakkında daha fazla ayrıntı için hizmet uç noktası için etkin hizmetin belgelerine bakın.

    Hizmet uç noktası için bir alt ağ etkin olduğunu doğrulamak için görüntüleme [geçerli rotalar](diagnose-network-routing-problem.md) alt ağdaki herhangi bir ağ arabirimi için. Bir uç nokta yapılandırıldığında, gördüğünüz bir *varsayılan* hizmetin adres ön eklerini ve nexthoptype değeri, yol **VirtualNetworkServiceEndpoint**. Yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
6. Seçtiğiniz sanal ağ alt ağı eklemek için seçin **Tamam**.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı oluşturma](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [Ekle-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>Alt ağ ayarlarını değiştir

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden ayarlarını değiştirmek için istediğiniz alt ağ içeren sanal ağı seçin.
3. **AYARLAR** altında **Alt ağlar**’ı seçin.
4. Alt ağlar listesinde ayarlarını değiştirmek için istediğiniz alt ağı seçin. Aşağıdaki ayarları değiştirebilirsiniz:

    - **Adres aralığı:** kaynak alt ağ içinde dağıtılırsa, adres aralığını değiştirebilirsiniz. Tüm kaynakları alt ağdaki mevcutsa, kaynakları başka bir alt ağa taşıma gerekir veya bunları alt ağdan silin. Taşıma veya bir kaynağı silme için uygulayacağınız adımlar, kaynağa bağlı olarak farklılık gösterir. Taşıma veya alt ağlardaki kaynaklar silme öğrenmek için taşımak veya silmek istediğiniz her bir kaynak türü için belgeleri okuyun. Kısıtlamalar için bkz. **adres aralığı** 5. adımında [bir alt ağ Ekle](#add-a-subnet).
    - **Kullanıcılar**: yerleşik roller veya kendi özel rollerinizi kullanarak alt ağ erişimi denetleyebilirsiniz. Rol ve alt ağa erişmek için kullanıcı atama hakkında daha fazla bilgi edinmek için [Azure kaynaklarınıza erişimi yönetmek için rol ataması kullanan](../role-based-access-control/role-assignments-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access).
    - **Ağ güvenlik grubu** ve **yol tablosu**: bkz. 5. adımı [bir alt ağ Ekle](#add-a-subnet).
    - **Hizmet uç noktalarını**: hizmet uç noktaları 5. adımında bkz [bir alt ağ Ekle](#add-a-subnet). Var olan bir alt ağ için hizmet uç noktası etkinleştirirken, kritik görev olmadığından alt ağdaki herhangi bir kaynak üzerinde çalıştığından emin olun. Hizmet uç noktaları olan varsayılan yol kullanarak alt ağdaki her ağ arabirimi yollara geçiş *0.0.0.0/0* adres ön eki ve sonraki atlama türü *Internet*, yeni bir yol ile kullanarak Adres ön ekleri, hizmet ve bir sonraki atlama türü *VirtualNetworkServiceEndpoint*. Geçiş sırasında açık TCP bağlantılarını sonlandırılabilir. Tüm ağ arabirimleri için trafik akışı yeni yol ile güncelleştirilene kadar hizmet uç noktası etkinleştirilmemiş. Yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
5. **Kaydet**’i seçin.

**Komutları**

- Azure CLI: [az ağ sanal ağ alt ağı güncelleştirme](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Bir alt ağı Sil

Yalnızca alt ağda hiçbir kaynaklar varsa, bir alt ağ silebilirsiniz. Alt ağdaki kaynaklar varsa, alt ağı silmeden önce alt ağdaki kaynakları silmeniz gerekir. Kaynak silme için uygulayacağınız adımlar, kaynak bağlı olarak farklılık gösterir. Alt ağlardaki olan kaynakları silmek öğrenmek için silmek istediğiniz her bir kaynak türü için belgeleri okuyun.

1. Portalın üst kısmındaki arama kutusuna girin *sanal ağlar* arama kutusuna. Zaman **sanal ağlar** arama sonuçlarında görünmesini, onu seçin.
2. Sanal ağlar listesinden silmek istediğiniz alt ağ içeren sanal ağı seçin.
3. **AYARLAR** altında **Alt ağlar**’ı seçin.
4. Alt ağlar listesinde seçin **...** , sağ tarafta, alt ağ için silmek istediğiniz
5. Seçin **Sil**ve ardından **Evet**.

**Komutları**

- Azure CLI: [az ağ vnet Sil](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-delete)
- PowerShell: [Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>İzinler

Alt ağlarda görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

|Eylem                                                                   |   Ad                                       |
|-----------------------------------------------------------------------  |   -----------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Bir sanal ağ alt okuyun              |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Bir sanal ağ alt güncelle  |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Bir sanal ağ alt ağı Sil            |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Bir sanal ağı'na katılın                     |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Bir alt ağ için hizmet uç noktasını girin     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Bir alt ağda sanal makineleri Al       |

## <a name="next-steps"></a>Sonraki adımlar

- Bir sanal ağ oluşturup alt ağları kullanarak [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure İlkesi](policy-samples.md) sanal ağlar için
