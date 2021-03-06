---
title: 'Bir sanal ağ geçidini silme: PowerShell: Azure Klasik | Microsoft Docs'
description: Klasik dağıtım modelinde PowerShell kullanarak bir sanal ağ geçidini silin.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: cherylmc
ms.openlocfilehash: 0c5d045cb949f5393bb2d9da54715b2b84229c36
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036297"
---
# <a name="delete-a-virtual-network-gateway-using-powershell-classic"></a>PowerShell (Klasik) kullanarak bir sanal ağ geçidini silme
> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-delete-vnet-gateway-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [Klasik - PowerShell](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Bu makalede, bir VPN ağ geçidi Klasik dağıtım modelinde PowerShell kullanarak silmenize yardımcı olur. Sanal ağ geçidi silindikten sonra artık kullanmadığınız öğeleri kaldırmak için ağ yapılandırma dosyasını değiştirin.

##<a name="connect"></a>1. adım: Azure'a bağlanma

### <a name="1-install-the-latest-powershell-cmdlets"></a>1. En son PowerShell cmdlet'lerini yükleyin.

Azure Hizmet Yönetimi (SM) PowerShell cmdlet'lerinin en son sürümünü indirip yeniden açın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

### <a name="2-connect-to-your-azure-account"></a>2. Azure hesabınıza bağlanın. 

PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Add-AzureAccount
```

## <a name="export"></a>2. adım: Dışarı aktarma ve ağ yapılandırma dosyasını görüntüleyin

Bilgisayarınızda bir dizin oluşturun ve sonra ağ yapılandırma dosyasını dizine aktarın. Her iki geçerli yapılandırma bilgilerini görüntülemek ve ağ yapılandırmasını değiştirmek için bu dosyayı kullanırsınız.

Bu örnekte, ağ yapılandırma dosyası C:\AzureNet dizinine aktarılır.

```powershell
Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
```

Dosyayı bir metin düzenleyiciyle açın ve klasik sanal adını görüntüleyin. Azure portalında sanal ağ oluşturduğunuzda, Azure kullanan tam adı portalda görünür değil. Örneğin, Azure portalında 'ClassicVNet1' adlandırılacak şekilde görünen bir VNet kadar uzun adlara sahip ağ yapılandırma dosyasında. Ad şuna benzer: 'Grup ClassicRG1 ClassicVNet1'. Sanal ağ adları olarak listelenen **' VirtualNetworkSite name ='**. Adları, PowerShell cmdlet'lerinizi çalıştırırken ağ yapılandırma dosyasında kullanın.

## <a name="delete"></a>3. adım: sanal ağ geçidini silme

Bir sanal ağ geçidini sildiğinizde, sanal ağdan sanal ağa ağ geçidi üzerinden tüm bağlantıları kesilir. Sanal ağa bağlanan P2S istemcileri varsa, uyarı vermeden kesilir.

Bu örnek, sanal ağ geçidini siler. Ağ yapılandırma dosyasında sanal ağın tam adını kullandığınızdan emin olun.

```powershell
Remove-AzureVNetGateway -VNetName "Group ClassicRG1 ClassicVNet1"
```

Başarılı olursa, dönüş gösterir:

```
Status : Successful
```

## <a name="modify"></a>4. adım: ağ yapılandırma dosyasını değiştirme

Bir sanal ağ geçidini sildiğinizde, ağ yapılandırma dosyasını cmdlet'e değiştirmez. Artık kullanılmayan öğeleri kaldırmak için dosyasını değiştirmeniz gerekir. Aşağıdaki bölümlerde, indirdiğiniz ağ yapılandırma dosyasını değiştirme yardımcı olur.

### <a name="lnsref"></a>Yerel ağ alanı başvurusu

Site başvuru bilgilerini kaldırmak için yapılandırma değişiklikleri yapmanız **ConnectionsToLocalNetwork/LocalNetworkSiteRef**. Bir yerel site başvuru Tetikleyicileri tünel silmek için Azure kaldırılıyor. Oluşturduğunuz yapılandırmasına bağlı olarak, olmayabilir bir **LocalNetworkSiteRef** listelenir.

```
<Gateway>
   <ConnectionsToLocalNetwork>
     <LocalNetworkSiteRef name="D1BFC9CB_Site2">
       <Connection type="IPsec" />
     </LocalNetworkSiteRef>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

Örnek:

```
<Gateway>
   <ConnectionsToLocalNetwork>
   </ConnectionsToLocalNetwork>
 </Gateway>
```

###<a name="lns"></a>Yerel ağ siteleri

Artık kullanmadığınız tüm yerel siteler kaldırın. Oluşturduğunuz yapılandırmasına bağlı olarak, yoksa olası bir **LocalNetworkSite** listelenir.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
  <LocalNetworkSite name="Site3">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>57.179.18.164</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

Bu örnekte, yalnızca Site3 kaldırdık.

```
<LocalNetworkSites>
  <LocalNetworkSite name="Site1">
    <AddressSpace>
      <AddressPrefix>192.168.0.0/16</AddressPrefix>
    </AddressSpace>
    <VPNGatewayAddress>5.4.3.2</VPNGatewayAddress>
  </LocalNetworkSite>
 </LocalNetworkSites>
```

### <a name="clientaddresss"></a>İstemci adres havuzu

Sanal ağınıza P2S bağlantısı varsa, olması bir **VPNClientAddressPool**. Sildiğiniz sanal ağ geçidi için karşılık gelen istemci adres havuzları kaldırın.

```
<Gateway>
    <VPNClientAddressPool>
      <AddressPrefix>10.1.0.0/24</AddressPrefix>
    </VPNClientAddressPool>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

Örnek:

```
<Gateway>
  <ConnectionsToLocalNetwork />
 </Gateway>
```

### <a name="gwsub"></a>GatewaySubnet

Silme **GatewaySubnet** sanal ağa karşılık gelir.

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
   <Subnet name="GatewaySubnet">
     <AddressPrefix>10.11.1.0/29</AddressPrefix>
   </Subnet>
 </Subnets>
```

Örnek:

```
<Subnets>
   <Subnet name="FrontEnd">
     <AddressPrefix>10.11.0.0/24</AddressPrefix>
   </Subnet>
 </Subnets>
```

## <a name="upload"></a>5. adım: ağ yapılandırma dosyasını karşıya yükleyin

Yaptığınız değişiklikleri kaydedin ve ağ yapılandırma dosyasını Azure'a yükleyin. Ortamınız için gerektiği gibi dosya yolu değiştirdiğinizden emin olun.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Başarılı olursa, dönüş bu örnektekine benzer bir şey gösterir:

```
OperationDescription        OperationId                      OperationStatus                                                
--------------------        -----------                      ---------------                                           
Set-AzureVNetConfig         e0ee6e66-9167-cfa7-a746-7casb9   Succeeded
```
