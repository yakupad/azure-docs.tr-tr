---
title: Oluşturma, değiştirme veya silme Azure genel bir IP adresi | Microsoft Docs
description: Oluşturma, değiştirme veya genel bir IP adresi silme öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 4345199ed952b6d0e044d4ac99c29c47c477780d
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287077"
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Oluşturma, değiştirme veya genel bir IP adresi silme

Bir ortak IP adresi ve oluşturmak, değiştirmek ve silmek nasıl hakkında bilgi edinin. Bir ortak IP adresi, kendi yapılandırılabilir ayarlar ile bir kaynaktır. Genel IP adresleri destekleyen bir Azure kaynağı genel bir IP adresi atayarak sağlar:
- Internet'ten gelen iletişimi kaynağa Azure sanal makineler (VM), Azure uygulama ağ geçitleri, Azure yük dengeleyicileri, Azure VPN ağ geçitleri ve diğerleri gibi. Hala Internet'ten VM'ler gibi bazı kaynaklar VM, VM bir yük dengeleyici arka uç havuzu bir parçasıdır ve yük dengeleyici genel IP adresi atanır sürece atanmış bir ortak IP adresi yoksa iletişim kurabilir. Belirli bir Azure hizmeti için bir kaynağı genel bir IP adresi atanmış veya olup bunun ile farklı bir Azure kaynağı ortak IP adresi bildirilmesi belirlemek için hizmet belgelerine bakın. 
- Tahmin edilebilir bir IP adresi kullanarak Internet bağlantısını giden. Örneğin, bir sanal makine, bir ortak IP adresi olmadan Internet'e giden kendisine atanmış, ancak Azure tarafından öngörülemeyen genel bir adres için varsayılan olarak çevrilmiş ağ adresi kendi adresidir iletişim kurabilir. Bir kaynağa genel bir IP adresi atayarak hangi IP adresi giden bağlantı için kullanılan bilmenizi sağlar. Tahmin edilebilir olsa, seçilen atama yöntemine bağlı olarak adresi değiştirebilirsiniz. Daha fazla bilgi için bkz: [genel bir IP adresi oluşturma](#create-a-public-ip-address). Azure kaynaklarını giden bağlantılar hakkında daha fazla bilgi için bkz: [giden bağlantılar anlamak](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümdeki adımları gerçekleştirmeden önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, kaydolun bir [ücretsiz deneme sürümü hesabı](https://azure.microsoft.com/free).
- Portalı kullanarak, açık https://portal.azure.comve Azure hesabınızda oturum.
- Bu makalede görevleri tamamlamak için PowerShell komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.
- Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu öğretici Azure CLI Sürüm 2.0.31 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure ile bir bağlantı oluşturmak için.

Hesap oturum açın veya ile azure'a bağlanmak için atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) içinde listelenen uygun eylemleri atanan [izinleri ](#permissions).

Nominal bir ücret ortak IP adresine sahip. Fiyatlandırma görüntülemek için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. 

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. Portal üst, sol köşesinde seçin **+ kaynak oluşturma**.
2. Girin *genel IP adresi* içinde *Market arama* kutusu. Zaman **genel IP adresi** arama sonuçlarında görünür.
3. Altında **genel IP adresi**seçin **oluşturma**.
4. Girin ya da altında aşağıdaki ayarları için değerleri seçin **ortak IP adresi oluştur**seçeneğini belirleyip **oluşturma**:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |Ad|Evet|Adı kaynak grubun içinde benzersiz olmalıdır.|
    |SKU|Evet|SKU'ları giriş önce oluşturulan tüm genel IP adresleri **temel** SKU genel IP adresleri.  Genel IP adresi oluşturulduktan sonra SKU değiştirilemiyor. Tek başına sanal makine, sanal makinelerin bir kullanılabilirlik kümesi ya da sanal makine ölçek kümeleri içinde temel veya standart SKU'ları kullanabilirsiniz.  Kullanılabilirlik kümeleri veya ölçek kümeleri içinde sanal makineler arasında SKU'ları karıştırma izin verilmiyor. **Temel** SKU: kullanılabilirlik bölgeyi destekleyen bir bölgede bir ortak IP adresi oluşturuyorsanız **kullanılabilirlik bölge** ayar *hiçbiri* varsayılan olarak. Ortak IP adresi için belirli bir bölgenin güvence altına almak için bir kullanılabilirlik bölgeyi seçmek seçebilirsiniz. **Standart** SKU: A standart SKU genel IP sanal makine ya da bir yük dengeleyici ön uç ilişkili olabilir. Kullanılabilirlik bölgeyi destekleyen bir bölgede bir ortak IP adresi oluşturuyorsanız **kullanılabilirlik bölge** ayar *bölge olarak yedekli* varsayılan olarak. Kullanılabilirlik bölgeler hakkında daha fazla bilgi için bkz: **kullanılabilirlik bölge** ayarı. Standart SKU, standart yük dengeleyici adresine ilişkilendirirseniz gereklidir. Standart yük Dengeleyiciler hakkında daha fazla bilgi için bkz: [Azure yük dengeleyici standart SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.|
    |IP sürümü|Evet| IPv4 veya IPv6 seçin. Ortak IPv4 adreslerinin bazı Azure kaynakları atanabilir olsa da, bir IPv6 ortak IP adresi yalnızca bir Internet'e yönelik Yük Dengeleyici atanabilir. Yük Dengeleyici Yük Dengelemesi IPv6 trafiği için Azure sanal makineler. Daha fazla bilgi edinmek [Yük Dengeleme IPv6 trafiğini sanal makinelere](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Seçtiyseniz **standart SKU**, seçmek için seçeneğinden yararlanamazsınız *IPv6*. Yalnızca bir IPv4 adresi kullanırken oluşturabileceğiniz **standart SKU**.|
    |IP adresi ataması|Evet|**Dinamik:** genel IP adresi için ilişkili tamamlandıktan sonra bir sanal makine ve sanal makineye bağlı bir ağ arabirimi ilk kez başlatıldığında yalnızca dinamik adresler atanır. Ağ arabiriminin bağlı olduğu sanal makine (serbest bırakıldığında) durdurulursa dinamik adresler değiştirebilirsiniz. Sanal makine yeniden veya durduruldu (ancak değil serbest) adresi aynı kalır. **Statik:** statik adresleri genel IP adresi oluşturulduğu zaman atanır. Sanal makine durduruldu (serbest bırakıldığında) durumda yerleştirirseniz bile statik adresleri değiştirmeyin. Ağ arabirimi silindiğinde adresi yalnızca yayımlanır. Ağ arabirimi oluşturulduktan sonra atama yöntemi değiştirebilirsiniz. Seçerseniz *IPv6* için **IP sürüm**, atama yöntemi *dinamik*. Seçerseniz *standart* için **SKU**, atama yöntemi *statik*.|
    |Boşta kalma zaman aşımı (dakika)|Hayır|Etkin tutma iletileri göndermek için istemcileri kullanmadan bir TCP veya HTTP bağlantısını açık tutmak için kaç dakika. IPv6 için seçerseniz **IP sürüm**, bu değer değiştirilemez. |
    |DNS ad etiketi|Hayır|Adı (tüm abonelikleri ve tüm müşteriler arasında) oluşturmak Azure konum içinde benzersiz olmalıdır. Ada sahip bir kaynağa bağlanabilmesi için azure otomatik olarak adı ve IP adresi, DNS'de kaydeder. Azure varsayılan bir alt ağı gibi ekler *location.cloudapp.azure.com* (konum seçtiğiniz konum olduğu) adına, tam DNS adı oluşturmak için sağladığınız. Her iki adres sürümü oluşturmayı seçerseniz, aynı DNS adı IPv4 ve IPv6 adreslerine atanır. Azure'nın varsayılan DNS A IPv4 ve IPv6 AAAA ad kayıtlarını içerir ve DNS adı arandığında her iki kayıt ile yanıt verir. İstemci ile iletişim kurmak için adres (IPv4 veya IPv6) seçer. Varsayılan son ek ile DNS ad etiketini kullanmak yerine veya buna ek olarak, genel IP adresine çözümlenen bir özel son ek ile DNS adını yapılandırmak için Azure DNS hizmetini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure genel IP adresiyle Azure DNS kullanma](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
    |Bir IPv6 (ya da IPv4) adresi oluşturma|Hayır| IPv6 ya da IPv4 görüntülenip görüntülenmeyeceğini ne için seçtiğiniz üzerinde bağımlı **IP sürüm**. Örneğin, **IPv4** için **IP sürüm**, **IPv6** burada görüntülenir. Seçerseniz *standart* için **SKU**, bir IPv6 adresi oluşturma seçeneğiniz yoktur.
    |Adı (size işaretlediyseniz görünür **bir IPv6 (ya da IPv4) adresi oluşturma** onay kutusunu)|Evet seçeneğini belirlerseniz, **IPv6 oluşturmak** (veya IPv4) onay kutusu.|Ad için girdiğiniz adından farklı olmalıdır ilk **adı** bu listede. Bir IPv4 ve IPv6 adresi oluşturmayı seçerseniz, iki ayrı ortak IP adresi kaynakları, kendisine atanmış her IP adresi sürümüne sahip bir portal oluşturur.|
    |IP adresi ataması (size işaretlediyseniz görünür **bir IPv6 (ya da IPv4) adresi oluşturma** onay kutusunu)|Evet seçeneğini belirlerseniz, **IPv6 oluşturmak** (veya IPv4) onay kutusu.|Onay kutusu diyorsa **bir IPv4 adresi oluşturma**, bir atama yöntemini seçin. Onay kutusu diyorsa **bir IPv6 adresi oluşturma**, bunu olması gerektiği bir atama yöntemi seçemezsiniz **dinamik**.|
    |Abonelik|Evet|Aynı bulunmalıdır [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) genel IP adresi ilişkilendirmek istediğiniz kaynak olarak.|
    |Kaynak grubu|Evet|Aynı veya farklı bulunabilir [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) genel IP adresi ilişkilendirmek istediğiniz kaynak olarak.|
    |Konum|Evet|Aynı bulunmalıdır [konumu](https://azure.microsoft.com/regions), genel IP ilişkilendirmek istediğiniz kaynak olarak adresi bölge da denir.|
    |Kullanılabilirlik bölgesi| Hayır | Desteklenen bir konum seçin, bu ayar yalnızca görünür. Desteklenen konumlar listesi için bkz: [kullanılabilirlik bölgeleri genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Seçtiyseniz **temel** SKU, *hiçbiri* sizin için otomatik olarak seçilir. Belirli bir bölgenin garanti tercih ederseniz, belirli bir bölgenin seçebilirsiniz. Her iki seçenek bölge olarak yedekli değil. Seçtiyseniz **standart** SKU: bölge olarak yedekli sizin için otomatik olarak seçilir ve veri yolu bölge hatalarına karşı dayanıklı hale getirir. Bölge hatalarına karşı dayanıklı değil, belirli bir bölgenin güvence altına almak tercih ederseniz, belirli bir bölgenin seçebilirsiniz.

**Komutları**

Portal, iki ortak IP adresi kaynakları (bir IPv4 ve bir IPv6) oluşturma seçeneği sağlar ancak aşağıdaki CLI ve PowerShell komutlarını bir kaynak için bir IP sürümü veya başka bir adres ile oluşturun. İki ortak IP adresi kaynakları, her IP sürümü için bir tane isterseniz, farklı adlar ve sürümler için genel IP adresi kaynakları belirtme komutu, iki kez çalıştırmanız gerekir.

|Aracı|Komut|
|---|---|
|CLI|[az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)|
|PowerShell|[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Görüntüleme, ayarlarını değiştirmek veya bir ortak IP adresini Sil

1. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *genel IP adresi*. Zaman **ortak IP adresleri** arama sonuçlarında görünecek, onu seçin.
2. Görüntülemek istediğiniz ayarlarını değiştirin ya da listeden silmek ortak IP adresini seçin.
3. Görüntüleme, silmek veya genel IP adresini değiştirmek isteyip istemediğinizi bağlı olarak aşağıdaki seçeneklerden birini tamamlayın.
    - **Görünüm**: **genel bakış** bölüm ilişkili için (adresi bir ağ arabirimiyle ilişkilendirilmiş ise) ağ arabirimi gibi ortak IP adresi için anahtar ayarlarını gösterir. Portal sürümü (IPv4 veya IPv6) adresinin görüntülemez. Sürüm bilgilerini görüntülemek için genel IP adresini görüntülemek için PowerShell veya CLI komutunu kullanın. IP adresi sürümü IPv6 ise, atanan adresi portal, PowerShell veya CLI tarafından görüntülenmez.
    - **Silme**: genel IP adresini silmek için seçin **silmek** içinde **genel bakış** bölümü. Adres geçerli bir IP yapılandırmasıyla ilişkilendirilmiş ise silinemiyor. Adresi şu anda bir yapılandırması ile ilişkili ise, seçin **ilişkiyi** IP yapılandırması adresinden ilişkilendirmesini kaldırmak.
    - **Değişiklik**: seçin **yapılandırma**. 4. adımda bilgileri kullanarak ayarları değiştirme [genel bir IP adresi oluşturma](#create-a-public-ip-address). Bir IPv4 adresi atamanın statik olan dinamik olarak değiştirmek için önce ilişkili olduğu IP yapılandırması gelen ortak IPv4 adresi ilişkilendirmesini kaldırmanız gerekir. Dinamik ve select atama yöntemi daha sonra değiştirebilirsiniz **ilişkilendirmek** IP ilişkilendirmek için aynı IP yapılandırması, farklı bir yapılandırma veya adresine ilkenin ilişkisi bırakılabilir. İçinde bir ortak IP adresi ilişkilendirmesini kaldırmak **genel bakış** bölümünde, select **ilişkiyi**.

    >[!WARNING]
    >Atama yöntemi statik olan dinamik olarak değiştirdiğinizde, genel IP adresi atanmış IP adresi kaybedersiniz. Statik veya dinamik adresleri ve (bir tanımladıysanız) tüm DNS ad etiketi arasında bir eşleme Azure ortak DNS sunucularını bakım yaparken, sanal makine durduruldu (serbest bırakıldığında) durumda kaldıktan sonra başlatıldığında dinamik bir IP adresi değiştirebilirsiniz. Adresinin değişmesini engellemek için bir statik IP adresi atayın.

**Komutları**

|Aracı|Komut|
|---|---|
|CLI|[az ağ ortak IP listesi](/cli/azure/network/public-ip#az-network-public-ip-list) listesi genel IP adresleri için [az ağ ortak IP Göster](/cli/azure/network/public-ip#az-network-public-ip-show) ayarları; göstermek için [az ağ ortak IP güncelleştirmesi](/cli/azure/network/public-ip#az-network-public-ip-update) güncelleştirmek için; [az ağ ortak IP silme](/cli/azure/network/public-ip#az-network-public-ip-delete) silmek için|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) bir ortak IP adres nesnesini almak ve ayarlarını görüntülemek için [kümesi AzureRmPublicIpAddress](/powershell/module/azurerm.network/set-azurermpublicipaddress) ayarlarını; güncelleştirmek için [Kaldır AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) silmek için|

## <a name="assign-a-public-ip-address"></a>Bir ortak IP adresi atayın

Bir ortak IP adresi aşağıdaki kaynaklara atama hakkında bilgi edinin:

- A [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (oluştururken) VM veya bir [var olan VM](virtual-network-network-interface-addresses.md#add-ip-addresses)
- [Internet'e yönelik Yük Dengeleyici](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure uygulama ağ geçidi](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Bir Azure VPN ağ geçidi kullanarak siteden siteye bağlantı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure sanal makine ölçek kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>İzinler

Ortak IP adreslerini görevleri gerçekleştirmek için hesabınızı atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) uygun eylemleri atanan rolü aşağıdaki tabloda listelenen:

| Eylem                                                             | Ad                                                           |
| ---------                                                          | -------------                                                  |
| Microsoft.Network/publicIPAddresses/read                           | Bir ortak IP adresi okuma                                          |
| Microsoft.Network/publicIPAddresses/write                          | Bir ortak IP adresi güncelle                           |
| Microsoft.Network/publicIPAddresses/delete                         | Bir ortak IP adresini Sil                                     |
| Microsoft.Network/publicIPAddresses/join/action                    | Bir ortak IP adresi bir kaynağa ilişkilendirme                    |

## <a name="next-steps"></a>Sonraki adımlar

- Genel bir IP adresini kullanarak oluşturmak [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyaları veya Azure kullanarak [Resource Manager şablonları](template-samples.md)
- Oluşturma ve uygulama [Azure ilke](policy-samples.md) için ortak IP adresleri
