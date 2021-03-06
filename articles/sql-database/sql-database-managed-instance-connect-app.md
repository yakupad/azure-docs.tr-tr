---
title: Azure SQL veritabanı yönetilen örneği, uygulama bağlayın | Microsoft Docs
description: Bu makalede, uygulamanızı Azure SQL veritabanı yönetilen örneğine bağlanma açıklanır.
ms.service: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.custom: managed instance
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: c9d656908d265aeb6143e857b0ea4f635203bdd9
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258737"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Uygulamanızı Azure SQL Veritabanı Yönetilen Örneği'ne bağlayın

Bugün nasıl ve nerede uygulamanızı barındırmak verirken birden çok seçeneğiniz vardır. 
 
Azure App Service veya Azure App Service ortamı, sanal makine, sanal makine ölçek kümesi gibi Azure'nın tümleşik sanal ağ (VNet) seçeneklerinden bazılarını kullanılarak uygulamayı bulutta barındırmak tercih edebilirsiniz. Ayrıca hibrit bulut yaklaşımı benimseyin ve uygulamaları şirket içi tutun. 
 
Oluşturduğunuz her seçenek bir yönetilen örneğe (Önizleme) bağlanabilirsiniz.  

![yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)  

## <a name="connect-an-application-inside-the-same-vnet"></a>Aynı sanal ağ içindeki bir uygulama bağlama 

Bu tasarımıdır senaryodur. Sanal makineler VNet içinde içinde farklı alt ağlarda olsalar bile birbirlerine doğrudan bağlanabilirsiniz. Tüm Azure uygulama ortamı veya sanal makine içinde uygulama bağlanmak için gereken bağlantı dizesi uygun şekilde ayarlanmış olması anlamına gelir.  
 
Bağlantı kurulamıyor durumda ayarlamak uygulama alt ağda bir ağ güvenlik grubu olup olmadığını denetleyin. Bu durumda, yeniden yönlendirme için bağlantı noktası aralığını 11000 12000 yanı sıra SQL bağlantı noktası 1433 üzerinde giden bağlantı açmanız gerekir. 

## <a name="connect-an-application-inside-a-different-vnet"></a>Farklı bir sanal ağ içindeki bir uygulama bağlama 

Yönetilen örnek kendi Vnet'ine özel IP adresi olduğundan bu biraz daha karmaşık bir senaryodur. Bağlanmak için bir uygulama yönetilen örneği dağıtıldığı sanal ağa erişimi gerekir. Bu nedenle, ilk, uygulama ve yönetilen örnek VNet arasında bir bağlantı yapmanız gerekir. Sanal ağlar bu senaryonun işe yaraması için sırayla aynı abonelikte olması gerekmez. 
 
Sanal ağları bağlama için iki seçenek vardır: 
- [Azure sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) 
- VNet-VNet VPN ağ geçidi ([Azure portalında](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md)) 
 
Eşleme Microsoft omurga ağı bu nedenle, bağlantı açısından kullandığından tercih bir eşleme seçenektir, eşlenmiş VNet ve aynı VNet içindeki sanal makineler arasında gecikme dikkat çekici fark yoktur. VNet eşlemesi, aynı bölgede ağlara sınırlıdır.  
 
> [!IMPORTANT]
> Yönetilen örneği için sanal ağ eşleme senaryo ağlara nedeniyle aynı bölgede sınırlı [sınırlamalar genel sanal ağ eşlemesi](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). 

## <a name="connect-an-on-premises-application"></a>Şirket içi uygulamaya bağlanma 

Yönetilen örnek, yalnızca özel bir IP adresi erişilebilir. Şirket içinden erişmek için uygulama ve yönetilen örnek sanal ağ arasında siteden siteye bağlantı yapmanız gerekir. 
 
Şirket içi Azure Vnet'e bağlanma iki seçenek vardır: 
- Siteden siteye VPN bağlantısı ([Azure portalında](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)) 
- [ExpressRoute](../expressroute/expressroute-introduction.md) bağlantı  
 
Şirket içi Azure bağlantısı başarıyla kuruldu ve yönetilen örnek bağlantı kuramıyor, güvenlik duvarınızı yeniden yönlendirme için bağlantı noktası aralığını 11000 12000 yanı sıra SQL bağlantı noktası 1433 üzerinde giden bağlantı olup olmadığını denetleyin. 

## <a name="connect-an-azure-app-service-hosted-application"></a>Azure App Service, barındırılan uygulamayı bağlama 

Azure App Service'ten erişmek için öncelikle uygulamayı yönetilen örnek Vnet'iniz arasında bağlantı yapmak gerekir böylece yönetilen örneği yalnızca özel bir IP adresi erişilebilir. Bkz: [uygulamanızı bir Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md).  
 
Sorun giderme için bkz: [sorun giderme sanal ağlar ve uygulamaları](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Bir bağlantı kurulamıyorsa deneyin [ağ yapılandırmasını senkronize](sql-database-managed-instance-sync-network-configuration.md). 
 
Tümleştirildiğinde, Azure App Service ağ yönetilen örneğini sanal ağa eşlenen Azure App Service, yönetilen örneğe bağlanma özel bir durum oluşturur. Bu durumda, ayarlanması için şu yapılandırmayı gerektirir: 

- Yönetilen örnek sanal ağ geçidi sahip olmamalıdır  
- Yönetilen örnek sanal ağ kullanım uzak ağ geçitlerini seçenek kümesi olmalıdır 
- Eşlenmiş VNet izin ağ geçidi geçiş seçeneği ayarlanmış olması gerekir 
 
Bu senaryo, aşağıdaki diyagramda gösterilmiştir:

![tümleşik uygulama eşlemesi](./media/sql-database-managed-instance/integrated-app-peering.png)
 
## <a name="connect-an-application-on-the-developers-box"></a>Geliştiriciler kutusunda uygulamayı bağlama 

Yönetilen örnek yalnızca özel bir IP adresi, geliştirici kutusundan erişmek için bu nedenle erişilebilir, önce Geliştirici box'ınızı ve yönetilen örnek VNet arasında bir bağlantı yapmanız gerekir.  
 
Yerel Azure sertifika kimlik doğrulaması makaleleri kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma ([Azure portalında](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md)) ayrıntılı olarak gösterilmiştir nasıl yapılabilir.  

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örneği hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md).
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren bir öğretici için bkz [bir yönetilen örnek oluşturma](sql-database-managed-instance-create-tutorial-portal.md).
