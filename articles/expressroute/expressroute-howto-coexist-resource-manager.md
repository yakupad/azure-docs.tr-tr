---
title: 'Bir arada var olabilen ExpressRoute ve Siteden Siteye VPN bağlantıları yapılandırma: Resource Manager: Azure | Microsoft Docs'
description: Bu makalede Resource Manager modeli için bir arada var olabilen ExpressRoute ve bir Siteden Siteye VPN bağlantısını nasıl yapılandıracağınız anlatılmaktadır.
documentationcenter: na
services: expressroute
author: charwen
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: charwen,cherylmc
ms.openlocfilehash: 9b0e19ac859d3f0185c42a79353651996fcbf631
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823572"
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Birlikte bulunan ExpressRoute bağlantıları ile Siteden Siteye bağlantıları yapılandırma
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell - Klasik](expressroute-howto-coexist-classic.md)
> 
> 

Siteden Siteye VPN ve ExpressRoute eşzamanlı bağlantılarını yapılandırmanın çeşitli avantajları vardır:

* ExpressRoute için güvenli bir yük devretme yolu olarak siteden siteye bir VPN yapılandırın. 
* Alternatif olarak ExpressRoute aracılığıyla bağlı olmayan sitelere bağlanmak için Siteden Siteye VPN’ler kullanabilirsiniz. 

İki senaryo için de yapılandırma adımları bu makalede verilmektedir. Bu makale Resource Manager dağıtım modelleri için geçerlidir ve PowerShell kullanır. Bu yapılandırma Azure portalında kullanılamaz.

>[!NOTE]
>ExpressRoute devresi üzerinde bir Siteden Siteye VPN oluşturmak istiyorsanız, lütfen [bu makaleye](site-to-site-vpn-over-microsoft-peering.md) bakınız.
>

## <a name="limits-and-limitations"></a>Sınırlar ve sınırlamalar
* **Geçiş yönlendirmesi desteklenmez.** Siteden Siteye VPN aracılığıyla bağlanan yerel ağınız ve ExpressRoute aracılığıyla bağlanan yerel ağınız arasında (Azure aracılığıyla) yönlendirme yapamazsınız.
* **Temel SKU ağ geçidi desteklenmez.** Hem [ExpressRoute ağ geçidi](expressroute-about-virtual-network-gateways.md) hem de [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) için Temel SKU olmayan bir ağ geçidi kullanmanız gerekir.
* **Yalnızca rota tabanlı VPN ağ geçidi desteklenir.** Rota tabanlı [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kullanmanız gerekir.
* **VPN ağ geçidiniz için statik rota yapılandırılmalıdır.** Yerel ağınız hem ExpressRoute hem de Siteden Siteye VPN’e bağlıysa Siteden Siteye VPN bağlantısını genel İnternet’e yönlendirebilmeniz için yerel ağınızda statik bir rotanın yapılandırılmış olması gerekir.

## <a name="configuration-designs"></a>Yapılandırma tasarımları
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Siteden siteye VPN’i ExpressRoute için bir yük devretme yolu olarak yapılandırma
Siteden siteye bir VPN bağlantısını ExpressRoute için yedek olarak yapılandırabilirsiniz. Bu bağlantı yalnızca Azure özel eşleme yoluna bağlı sanal ağlar için geçerlidir. Azure Microsoft eşlemeleri aracılığıyla erişilebilen hizmetler için VPN tabanlı yük devretme çözümü yoktur. ExpressRoute bağlantı hattı her zaman birincil bağlantıdır. Veriler yalnızca ExpressRoute bağlantı hattı başarısız olursa, Siteden Siteye VPN üzerinden akar. Asimetrik yönlendirmenin önüne geçmek için yerel ağ yapılandırmanız da Siteden Siteye VPN üzerinden ExpressRoute bağlantı hattını tercih etmelidir. ExpressRoute’u alan yönlendirmeler için daha yüksek yerel tercihleri ayarlayarak ExpressRoute yolunu tercih edebilirsiniz. 

> [!NOTE]
> Her iki yol da aynı olduğunda ExpressRoute bağlantı hattı Siteden Siteye VPN’ye tercih edilse de Azure paketin hedefine yönelik yolu seçmek için en uzun ön ek eşleşmesini kullanır.
> 
> 

![Bir arada var olma](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>ExpressRoute aracılığıyla bağlanılmayan sitelere bağlanmak için Siteden Siteye VPN yapılandırma
Ağınızı bazı sitelerin Azure’a Siteden Siteye VPN üzerinden doğrudan ve bazı sitelerin ExpressRoute üzerinden bağlanması için yapılandırabilirsiniz. 

![Bir arada var olma](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Sanal ağı, geçiş yönlendiricisi olarak yapılandıramazsınız.
> 
> 

## <a name="selecting-the-steps-to-use"></a>Kullanılacak adımları seçme
Aralarından seçim yapabileceğiniz iki farklı yordam kümesi vardır. Seçtiğiniz yapılandırma yordamı, bağlanmak istediğiniz mevcut bir sanal ağ olup olmadığına veya yeni bir sanal ağ oluşturmak isteyip istememenize bağlıdır.

* Bir VNet’im yok ve bir tane oluşturmam gerekiyor.
  
    Zaten bir sanal ağınız yoksa, bu yordam Resource Manager dağıtım modelini kullanarak yeni bir sanal ağ oluşturmak ve yeni ExpressRoute ve Siteden Siteye VPN bağlantıları oluşturmak için size yol gösterir. Sanal ağı yapılandırmak için, [Yeni bir sanal ağ ve bir arada var olabilen bağlantılar oluşturmak için](#new) bölümündeki adımları izleyin.
* Zaten bir Resource Manager dağıtım modeli VNet’im var.
  
    Mevcut bir Siteden Siteye VPN bağlantısı veya ExpressRoute bağlantısına sahip bir sanal ağınız zaten olabilir. Bu senaryoda ağ geçidi alt ağ maskesi /28 veya daha büyükse varolan ağ geçidini silmeniz gerekir. [Zaten olan bir VNet için aynı anda mevcut bağlantılar yapılandırma](#add) bölümü, ağ geçidini silme ve ardından yeni ExpressRoute ve Siteden Siteye VPN bağlantıları oluşturma işlemi boyunca size yol gösterir.
  
    Ağ geçidinizi silip yeniden oluşturursanız şirket içi ve dışı karışık bağlantılarınız için kesinti süresi olur. Ancak VM'leriniz ve hizmetleriniz bunu yapmak için yapılandırılmışsa, ağ geçidi yapılandırması sırasında yük dengeleyici üzerinden iletişim kurmaya devam eder.

## <a name="new"></a>Yeni bir sanal ağ ve bir arada var olabilen bağlantılar oluşturmak için
Bu yordamda, bir VNet oluşturma ve bir arada var olabilen Siteden Siteye ve ExpressRoute bağlantıları oluşturma işlemleri adım adım açıklanmıştır.

1. Azure PowerShell cmdlet’lerinin en son sürümünü yükleyin. Cmdlet'leri yükleme hakkında bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Bu yapılandırma için kullanacağınız cmdlet'ler tanıdıklarınızdan biraz farklı olabilir. Bu yönergelerde belirtilen cmdlet'leri kullandığınızdan emin olun.

2. Hesabınızda oturum açın ve ortamı ayarlayın.

  ```powershell
  Connect-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Ağ Geçidi Alt Ağı içeren bir sanal ağ oluşturun. Sanal ağ oluşturma hakkında daha fazla bilgi için bkz. [Sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Alt ağ oluşturma hakkında daha fazla bilgi için bkz. [Alt ağ oluşturma](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet)
   
   > [!IMPORTANT]
   > Ağ Geçidi Alt Ağı /27 veya daha kısa bir ön ek (örneğin /26 veya /25) olmalıdır.
   > 
   > 
   
    Yeni bir VNet oluşturun.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Alt ağlar ekleyin.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    VNet yapılandırmasını kaydedin.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="vpngw"></a>Ardından, Siteden Siteye VPN ağ geçidinizi oluşturun. VPN ağ geçidi yapılandırması hakkında daha fazla bilgi için bkz. [Siteden Siteye bağlantı ile VNet yapılandırma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). GatewaySku yalnızca *VpnGw1*, *VpnGw2*, *VpnGw3*, *Standard* ve *HighPerformance* VPN ağ geçitlerinde desteklenir. ExpressRoute-VPN Gateway ağ geçidi bir arada var olabilen yapılandırmaları, temel SKU'da desteklenmez. VpnType değeri *RouteBased* olmalıdır.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "VpnGw1"
  ```
   
    Azure VPN ağ geçidi, BGP yönlendirme protokolünü destekler. Aşağıdaki komuta -Asn anahtarını ekleyerek söz konusu Sanal Ağ için ASN (AS Numarası) belirtebilirsiniz. Bu parametreyi belirtmediğinizde AS numarası varsayılan olarak 65515 şeklinde ayarlanır.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "VpnGw1" -Asn $VNetASN
  ```
   
    BGP eşleme IP’sini ve Azure’un VPN ağ geçidi için kullandığı AS numarasını $azureVpn.BgpSettings.BgpPeeringAddress ve $azureVpn.BgpSettings.Asn’de bulabilirsiniz. Daha fazla bilgi için bkz. Azure VPN ağ geçidi için [BGP’yi yapılandırma](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md).
5. Bir yerel site VPN ağ geçidi varlığı oluşturun. Bu komut, şirket içi VPN ağ geçidinizi yapılandırmaz. Azure VPN ağ geçidinin bağlanabilmesi için ortak IP ve şirket içi adres alanı gibi yerel ağ geçidi ayarlarını paylaşmanıza olanak tanır.
   
    Yerel VPN cihazınız yalnızca statik yönlendirmeyi destekliyorsa statik rotaları aşağıdaki şekilde yapılandırabilirsiniz:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Yerel VPN cihazınız BGP’yi destekliyorsa ve dinamik yönlendirmeyi etkinleştirmek istiyorsanız yerel VPN cihazınızın kullandığı BGP eşleme IP’sini ve AS numarasını bilmeniz gerekir.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
6. Yerel VPN cihazınızı yeni Azure VPN ağ geçidine bağlanmak için yapılandırın. VPN cihazını yapılandırma hakkında daha fazla bilgi için bkz. [VPN Cihazı Yapılandırma](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

7. Azure’da Siteden Siteye ağ geçidini yerel ağ geçidine bağlayın.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```
 

8. Varolan bir ExpressRoute bağlantı hattına bağlanıyorsanız 8 ve 9. adımları atlayıp 10. adıma geçin. ExpressRoute bağlantı hatlarını yapılandırın. ExpressRoute bağlantı hattını yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md).


9. ExpressRoute bağlantı hattı üzerinde Azure özel eşlemeyi yapılandırın. ExpressRoute bağlantı hattı üzerinde Azure özel eşlemeyi yapılandırma hakkında daha fazla bilgi için bkz. [eşlemeyi yapılandırma](expressroute-howto-routing-arm.md)

10. <a name="gw"></a>ExpressRoute ağ geçidi oluşturun. ExpressRoute ağ geçidi yapılandırması hakkında daha fazla bilgi için bkz. [ExpressRoute ağ geçidi yapılandırması](expressroute-howto-add-gateway-resource-manager.md). GatewaySKU değeri *Standard*, *HighPerformance* veya *UltraPerformance* olmalıdır.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
11. ExpressRoute ağ geçidini ExpressRoute bağlantı hattına bağlayın. Bu adım tamamlandıktan sonra, ExpressRoute aracılığıyla şirket içi ağınız ve Azure arasında bağlantı kurulur. Bağlantı işlemi hakkında daha fazla bilgi için bkz.[VNets’i ExpressRoute’a bağlama](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```

## <a name="add"></a>Zaten mevcut bir VNet için bir arada var olabilen bağlantılar yapılandırma
Zaten bir sanal ağınız varsa, ağ geçidi alt ağ boyutunu kontrol edin. Ağ geçidi alt ağı /28 veya /29 ise, önce sanal ağ geçidi silmeniz ve ağ geçidi alt ağı boyutunu artırmanız gerekir. Bu bölümdeki adımlar bunu nasıl yapacağınızı gösterir.

Ağ geçidi alt ağı /27 veya daha büyükse ve sanal ağ ExpressRoute üzerinden bağlanıyorsa, aşağıdaki adımları atlayarak önceki bölümdeki ["4. Adım - Siteden Siteye VPN ağ geçidi oluşturma"](#vpngw) bölümüne geçebilirsiniz. 

> [!NOTE]
> Mevcut ağ geçidini sildiğinizde, bu yapılandırma üzerinde çalışırken, yerel şirket sanal ağ bağlantısını kaybeder. 
> 
> 

1. Azure PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. Cmdlet'leri yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Bu yapılandırma için kullanacağınız cmdlet'ler tanıdıklarınızdan biraz farklı olabilir. Bu yönergelerde belirtilen cmdlet'leri kullandığınızdan emin olun. 
2. Mevcut ExpressRoute veya Siteden Siteye VPN ağ geçidini silin.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Ağ Geçidi Alt Ağını silin.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. /27 veya daha büyük bir Ağ Geçidi Alt Ağı ekleyin.
   
   > [!NOTE]
   > Sanal ağınızda ağ geçidi alt ağı boyutunu artırmak için yeterli IP adresi kalmadıysa, daha fazla IP adresi alanı eklemeniz gerekir.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    VNet yapılandırmasını kaydedin.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. Bu noktada, hiçbir ağ geçidi olmayan bir VNet’e sahip olursunuz. Yeni ağ geçitleri oluşturmak ve bağlantılarınızı tamamlamak için, önceki adım kümesinde bulabileceğiniz [4. Adım - Bir Siteden Siteye VPN ağ geçidi oluşturma](#vpngw) bölümüyle devam edebilirsiniz.

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>VPN ağ geçidine noktadan siteye yapılandırması eklemek için
Bir arada var olan kurulumda VPN ağ geçidinize Noktadan Siteye yapılandırması eklemek için aşağıdaki adımları izleyebilirsiniz.

1. VPN İstemcisi adres havuzunu ekleyin.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. VPN ağ geçidiniz için VPN kök sertifikasını Azure’a yükleyin. Bu örnekte, kök sertifikasının aşağıdaki PowerShell cmdlet’lerinin çalıştığı yerel makinede saklandığı varsayılır.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Noktadan Siteye VPN hakkında daha fazla bilgi içini bkz. [Noktadan Siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
