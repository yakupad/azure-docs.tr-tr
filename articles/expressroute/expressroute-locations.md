---
title: 'Bağlantı sağlayıcıları ve konumları: Azure ExpressRoute | Microsoft Docs'
description: Bu makale, sunulan hizmetlerin konumları ve Azure bölgelerine nasıl bağlanılacağı hakkında ayrıntılı bir genel bakış sağlar. Bağlantı sağlayıcısına göre sıralanır.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: c878513a-d594-42ad-8b0e-403efd0c4b25
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2018
ms.author: jaredro
ms.openlocfilehash: 35d15811f31e64d0c27a23da802a16ce2e4570aa
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113992"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute ortakları ve eşleme konumları

> [!div class="op_single_selector"]
> * [Sağlayıcıya Göre Konumlar](expressroute-locations.md)
> * [Konuma Göre Sağlayıcılar](expressroute-locations-providers.md)


Bu makaledeki tablolar ExpressRoute bağlantı sağlayıcıları, ExpressRoute coğrafi kapsamı, ExpressRoute üzerinden desteklenen Microsoft bulut hizmetleri ve ExpressRoute Sistem Tümleştiricileri (SIs) hakkında bilgi sağlar.

## <a name="partners"></a>ExpressRoute bağlantı sağlayıcıları
ExpressRoute tüm Azure bölgeleri ve konumları arasında desteklenir. Aşağıdaki harita Azure bölgeleri ve ExpressRoute konumlarının listesini sağlar. ExpressRoute konumları birkaç hizmet sağlayıcının sahip olduğu Microsoft eşlerine başvurur.

![Konum eşleme][0]

Coğrafi bölge içindeki en az bir ExpressRoute konumuna bağlanırsanız coğrafi bölge içindeki tüm bölgeler arasında Azure hizmetlerine erişebileceksiniz.

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Bir coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında eşleme.
Aşağıdaki tablo, coğrafi bölge içindeki Azure bölgeler ile ExpressRoute konumları arasında yapılan eşlemeyi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **Kuzey Amerika** |Doğu ABD, Batı ABD, Doğu ABD 2, Batı ABD 2, Orta ABD, Orta Güney ABD, Orta Kuzey ABD, Batı Orta ABD, Orta Kanada, Doğu Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, New York, San Antonio, Seattle, Silikon Vadisi, Washington, Montreal, Quebec City, Toronto |
| **Güney Amerika** |Güney Brezilya |Sao Paulo |
| **Avrupa** |Fransa Orta, Fransa Güney, Kuzey Avrupa, Batı Avrupa, UK Batı, UK Güney |Amsterdam, Dublin, Londra, Newport(Galler), Paris |
| **Asya** |Doğu Asya, Güneydoğu Asya |Hong Kong, Singapur, Singapur2 |
| **Japonya** |Batı Japonya, Doğu Japonya |Osaka, Tokyo |
| **Avustralya** |Güneydoğu Avustralya, Doğu Avustralya |Melbourne, Sidney |
| **Australia Government** | Avustralya Orta, Avustralya Orta 2 |Kanberra, Kanberra2 | 
| **Hindistan** |Batı Hindistan, Orta Hindistan, Güney Hindistan |Chennai, Mumbai |
| **Güney Kore** |Kore Orta, Kore Güney |Busan, Seul |
| **Güney Afrika** |[Güney Afrika Batı +, Güney Afrika Kuzey +](https://blogs.microsoft.com/blog/2017/05/18/microsoft-deliver-microsoft-cloud-datacenters-africa/) |Cape Town, Johannesburg |

 **+** çok yakında anlamına geliyor

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Ulusal bulutlar için bölgeler ve coğrafi sınırlar
Aşağıdaki tablo ulusal bulutlar için bölgeler ve coğrafi sınırlar hakkında bilgi sağlar.

| **Jeopolitik bölge** | **Azure bölgeleri** | **ExpressRoute konumları** |
| --- | --- | --- |
| **US Government bulutu** |ABD Devleti Arizona, ABD Devleti Iowa, ABD Devleti Texas, ABD DEvleti Virginia, US DoD Orta, US DoD Doğu  |Chicago, Dallas, New York, Seattle, Silikon Vadisi, Washington DC |
| **Çin** |Kuzey Çin, Doğu Çin |Pekin, Şangay |
| **Almanya** |Orta Almanya, Doğu Almanya |Berlin, Frankfurt |

Coğrafi bölgeler arasındaki bağlantı standart ExpressRoute SKU’da desteklenmiyor. Genel bağlantıyı desteklemek için ExpressRoute premium eklentisini etkinleştirmeniz gerekir. Ulusal bulut ortamlarına bağlantı desteklenmiyor. Bu tür bir ihtiyaç ortaya çıkarsa bağlantı sağlayıcınız ile çalışabilirsiniz.

## <a name="locations"></a>Bağlantı sağlayıcı konumları

Aşağıdaki tabloda hizmet sağlayıcısına göre konumlar gösterilmektedir. Kullanılabilir sağlayıcıları konuma göre görüntülemek istiyorsanız bkz. [Konuma göre hizmeti sağlayıcılar](expressroute-locations-providers.md#locations).


### <a name="production-azure"></a>Üretim Azure
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365 ve Dynamics 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/cloud-services-applications/azure-expressroute/)** |Destekleniyor |Destekleniyor |Melbourne, Sidney |
| **[Airtel](http://www.airtel.in/creatingsmiles/)** | Destekleniyor | Destekleniyor | Chennai, Mumbai |
| **[Aryaka Networks](http://www.aryaka.com/)** |Destekleniyor |Destekleniyor |Amsterdam, Dallas, Hong Kong, Silikon Vadisi, Singapur, Tokyo, Washington DC |
| **[Ascenty Data Centers](https://ascenty.com/servicos/cloud-connect/microsoft-expressroute/)** |Destekleniyor |Destekleniyor |Sao Paulo |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Toronto, Washington DC |
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Destekleniyor |Destekleniyor |Montreal, Toronto, Quebec City |
| **[British Telecom](https://globalservices.bt.com/en/solutions/products/bt-compute-for-microsoft-azure)** |Destekleniyor |Destekleniyor |Amsterdam, Hong Kong, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **[C3ntro](https://c3ntro.com/)** |Çok yakında |Çok yakında |Miami |
| **CDC** | Destekleniyor | Destekleniyor | Kanberra, Kanberra2 |
| **[CenturyLink Cloud Connect](http://www.centurylink.com/cloudconnect)** |Destekleniyor |Destekleniyor |Las Vegas, New York, San Antonio, Silikon Vadisi, Tokyo, Toronto |
| **China Telecom Global** |Destekleniyor |Desteklenmiyor |Hong Kong |
| **[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** |Destekleniyor |Destekleniyor |Dallas, Montreal, Toronto |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Destekleniyor |Destekleniyor |Amsterdam, Dublin, Londra, Paris, Tokyo |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Destekleniyor |Destekleniyor |Chicago, Silikon Vadisi, Washington DC |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Destekleniyor |Destekleniyor |Chicago, Denver, Los Angeles, New York, Silikon Vadisi, Washinton DC |
| **eir** |Destekleniyor |Destekleniyor |Dublin|
| **Epsilon Global Communications** |Destekleniyor |Destekleniyor |Singapur |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Destekleniyor |Destekleniyor |Amsterdam, Atlanta, Chicago, Dallas, Dublin,  Hong Kong, Londra, Los Angeles, Melbourne, Miami, New York, Osaka, Paris, Sao Paulo, Seattle, Silikon Vadisi, Singapur, Sidney, Tokyo, Toronto, Washington DC |
| **euNetworks** |Destekleniyor |Destekleniyor |Amsterdam, Londra |
| **GÉANT** |Destekleniyor |Destekleniyor |Amsterdam |
| **[Genel Bulut Değişimi (GCX)] (http://globalcloudxchange.com/cloud-platform/cloud-x-fusion/cloud-x-fusion-for-azure/)** | Destekleniyor| Destekleniyor | Chennai, Mumbai |
| **[InterCloud](https://www.intercloud.com/)** |Destekleniyor |Destekleniyor |Amsterdam, Londra, Paris, Singapur, Washington DC |
| **Internet2** |Destekleniyor |Destekleniyor |Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Destekleniyor |Destekleniyor |Osaka, Tokyo |
| **[Internet Solutions - Cloud Connect](https://www.is.co.za/solution/cloud-connect/)** |Destekleniyor |Destekleniyor |Cape Town, Johannesburg, Londra |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Destekleniyor |Destekleniyor |Amsterdam, Dublin, Londra, Paris |
| **[IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)**|Destekleniyor |Destekleniyor | Silicon Valley, Toronto |
| **Jisc** |Destekleniyor |Destekleniyor |Londra |
| **[KINX](https://www.kinx.net/service/network/cloudhub/ms-expressroute/?lang=en)** |Destekleniyor |Destekleniyor |Seul |
| **[KPN](http://www.kpn.com/cloudconnect)** | Destekleniyor | Destekleniyor | Amsterdam | 
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Londra, Newport, Sao Paulo, Seattle, Silikon Vadisi, Singapur, Washington |
| **LG CNS** |Destekleniyor |Destekleniyor |Busan, Seul |
| **[Liquid Telecom](https://www.liquidtelecom.com/products-and-services/cloud.html)** |Destekleniyor |Destekleniyor |Cape Town, Johannesburg |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Destekleniyor |Destekleniyor |Amsterdam, Atlanta, Chicago, Dallas, Denver, Dublin, Hong Kong, Las Vegas, London, Los Angeles, Melbourne, Miami, New York, Quebec City, San Antonio, Seattle, Silicon Valley, Singapore, Singapore2, Sydney, Toronto, Washington DC |
| **MTN** |Destekleniyor |Destekleniyor |Londra |
| **[Neutrona Networks](http://www.neutrona.com/index.php/azure-expressroute/)** |Destekleniyor |Destekleniyor |Dallas, Miami, Sao Paulo |
| **[Next Generation Data](http://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Destekleniyor |Destekleniyor |Newport(Galler) |
| **NEXTDC** |Destekleniyor |Destekleniyor |Melbourne, Sidney |
| **[NTT Communications](http://www.ntt.com/en/services/network/virtual-private-network.html)** |Destekleniyor |Destekleniyor |Amsterdam, Hong Kong, Londra, Los Angeles, Osaka, Singapur, Sidney, Tokyo, Washington DC |
| **[NTT EAST](https://flets.com/cloudgateway/crossconnect/)** |Destekleniyor |Destekleniyor |Tokyo |
| **[NTT SmartConnect](http://cloud.nttsmc.com/cxc/azure.html)** |Destekleniyor |Destekleniyor |Osaka |
| **[Optus](https://www.optus.com.au/enterprise/networking/network-connectivity/express-link)** |Destekleniyor |Destekleniyor |Melbourne+ , Sidney |
| **[Orange](http://www.orange-business.com/en/products/business-vpn-galerie)** |Destekleniyor |Destekleniyor |Amsterdam, Hong Kong, Londra, Paris, Silikon Vadisi, Singapur, Sidney, Washington DC |
| **[PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)** |Destekleniyor |Destekleniyor |Chicago, Silikon Vadisi |
| **PCCW Global Limited** |Destekleniyor |Destekleniyor |Hong Kong |
| **[Sejong Telecom](https://www.sejongtelecom.net/en/pages/service/cloud_ms)** |Destekleniyor |Destekleniyor |Seul |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Destekleniyor |Destekleniyor |Chennai, Mumbai |
| **[SingTel](http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Destekleniyor |Destekleniyor |Singapur, Singapur2 |
| **[Softbank](http://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Destekleniyor |Destekleniyor |Osaka, Tokyo |
| **[Sprint](https://business.sprint.com/solutions/cloud-networking/)** |Destekleniyor |Destekleniyor |Chicago, Silikon Vadisi, Washington DC |
| **[Tata Communications](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Destekleniyor |Destekleniyor |Amsterdam, Chennai, Hong Kong, Londra, Mumbai, Silikon Vadisi, Singapur, Washington DC |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Destekleniyor |Destekleniyor |Amsterdam, Sao Paulo |
| **[Telehouse - KDDI](http://www.telehouse.net/solutions/cloud-services/cloud-link)** |Destekleniyor |Destekleniyor |Londra |
| **Telenor** |Destekleniyor |Destekleniyor |Amsterdam, Londra |
| **[Telia Carrier](https://teliacarrier.com/our-services/connectivity/cloud-connect.html?title=Cloud%20Connect)** | Destekleniyor | Destekleniyor |Londra, Washington |
| **Telmex Uninet**| Çok yakında | Çok yakında | Dallas+ |
| **[Telstra Corporation](http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Destekleniyor |Destekleniyor |Melbourne, Sidney |
| **[Telus](http://www.telus.com)** |Destekleniyor |Destekleniyor |Montreal, Toronto |
| **[Teraco](https://www.teraco.co.za/services/africa-cloud-exchange/)** |Destekleniyor |Destekleniyor |Cape Town, Johannesburg |
| **[UOLDIVEO](http://www.uoldiveo.com.br/solucoes/cloud.html#rmcl)** |Destekleniyor |Destekleniyor |Sao Paulo |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas, Hong Kong, Londra, Silikon Vadisi, Singapur, Sidney, Tokyo, Washington DC |
| **[Vodafone](http://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Destekleniyor |Desteklenmiyor |Londra |
| **[Zayo](http://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Destekleniyor |Destekleniyor |Amsterdam, Chicago, Dallas+, Londra, Los Angeles, Montreal, New York, Silikon Vadisi, Toronto, Washington |

 **+** çok yakında anlamına geliyor

### <a name="national-cloud-environment"></a>Ulusal bulut ortamı

### <a name="us-government-cloud"></a>US Government bulutu
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Destekleniyor |Destekleniyor |Chicago, Washington DC |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Destekleniyor |Destekleniyor |Chicago, Dallas, New York, Seattle, Silikon Vadisi, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Destekleniyor |Destekleniyor |Chicago, New York+, Silikon Vadisi, Washington |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Destekleniyor | Destekleniyor | Chicago, Dallas |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Destekleniyor |Destekleniyor |Chicago, Dallas, New York, Washington DC |

### <a name="china"></a>Çin
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **China Telecom** |Destekleniyor |Desteklenmiyor |Pekin, Şangay |

Daha fazla öğrenmek için, bkz. [Çin’de ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Almanya
| **Hizmet sağlayıcı** | **Microsoft Azure** | **Office 365** | **Konumlar** |
| --- | --- | --- | --- |
| **[Colt](http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Destekleniyor |Desteklenmiyor |Berlin+, Frankfurt |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** |Destekleniyor |Desteklenmiyor |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Destekleniyor |Desteklenmiyor |Berlin |
| **Interxion** |Destekleniyor |Desteklenmiyor |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Destekleniyor  | Desteklenmiyor | Berlin |
| **T-Systems** |Destekleniyor |Desteklenmiyor |Berlin |

## <a name="connectivity-through-exchange-providers"></a>Exchange Sağlayıcıları Üzerinden Bağlantı

Bağlantı sağlayıcınız önceki bölümlerde listelenmemişse hala bağlantı oluşturabilirsiniz.

* Yukarıdaki tabloda yer alan değişimlerin herhangi birine bağlı olup olmadığını görmek için bağlantı sağlayıcınıza başvurun. Değişim sağlayıcıları tarafından sunulan hizmetler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kontrol edebilirsiniz. Birkaç bağlantı sağlayıcı Ethernet değişimlerine zaten bağlı.
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](http://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/services/cloud-connectivity/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](http://www.nextdc.com/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Bağlantı sağlayıcınızı, ağınızı seçtiğiniz eşleme konumuna genişletmesini sağlayın.
  * Bağlantı sağlayıcınızın bağlantınızı yüksek oranda kullanılabilir şekilde genişlettiğinden emin olun, böylece hiç tek nokta arızası olmaz.
* Microsoft’a bağlanmak için bağlantı sağlayınız olarak değişime sahip bir ExpressRoute bağlantı hattı sipariş edin.
  * Bağlantı kurmak için [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)’daki adımları izleyin.

## <a name="connectivity-through-additional-service-providers"></a>Diğer Hizmet Sağlayıcılar Üzerinden Bağlantı

| **Bağlantı sağlayıcı** | **Exchange** | **Konumlar** |
| --- | --- | --- |
| **[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapur |
| **[Airgate Technologies, Inc.](http://airgate.ca/cloud-express/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](http://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://www.arteria-net.com/business/service/cloud/sca/)** |Equinix |Tokyo |
| **[Axtel](http://alestra.mx/landing/expressrouteazure/)** |Equinix |Dallas|
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Londra |
| **[BICS](https://bics.com/bics-solutions-suite/cloud-connect/)** | Equinix | Amsterdam, Frankfurt, Londra, Singapur, Washington DC |
| **[BroadBand Tower, Inc.](http://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokyo |
| **[C3ntro Telecom](http://www.c3ntro.com/data/cloud-conectivity/)** | Equinix, Megaport | Dallas |
| **[Cinia](https://www.cinia.fi/en/services/connectivity-services/direct-public-cloud-connection.html)** | Equinix, Megaport | Frankfurt, Hamburg |
| **[Cogeco Peer 1](https://www.cogecopeer1.com/en/)**| Equinix | Montreal, Toronto |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)** | Equinix | Dallas, Silikon Vadisi, Washington DC | 
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](http://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | Londra, Singapur, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](http://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londra |
| **[Fastweb S.p.A](http://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Gtt Communications Inc](https://www.gtt.net/wp-content/uploads/2017/04/EtherCloud-Data-Sheet.pdf)** |Equinix | Washington DC |
| **[HSO](http://www.hso.co.uk/products/cloud-direct)** |Equinix | Londra, Slough |
| **[IVedha Inc](http://www.ivedha.com/cloud/manage-azure-cloud/express-route-4/)**| Equinix | Toronto |
| **LGA Telecom** |Equinix |Singapur|
| **[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure)** |Equinix | Chicago, New York, Washington DC |
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hong Kong |
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sidney |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Amsterdam |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[NexGen Networks](http://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Londra |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Telecity | Amsterdam, Frankfurt |
| **[Post](https://www.teralinksolutions.com/cloud-connectivity/cloudbridge-to-azure-expressroute/)**|Equinix | Amsterdam |
| **[Proximus](https://www.proximus.be/en/id_b_cl_proximus_external_cloud_connect/companies-and-public-sector/discover/magazines/expert-blog/proximus-external-cloud-connect.html)**|Equinix | Amsterdam, Dublin, Londra, Paris |
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Telecity | Londra | 
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Amsterdam |
| **[ThinkTel](http://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[Transtelco](http://www.transtelco.net/tcloud/microsoft)** |Equinix | Dallas, Los Angeles |  
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapur |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](http://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silikon Vadisi, Washington DC |
| **Zain** |Equinix |Londra|
| **[Zertia](http://www.zertia.es/index.php/novedades)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Veri Merkezi Sağlayıcıları Üzerinden Bağlantı
| **Sağlayıcı** | **Exchange** |
| --- | --- |
| **[Cyrus One](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | Megaport |
| **[EdgeConnex](http://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport |
| **[RagingWire Data Centers](http://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach |
| **[T5 Datacenters](http://t5datacenters.com/network-cloud-connect/)** | IX Reach |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Ulusal Araştırma ve Eğitim Ağları (NREN) Üzerinden Bağlantı

| **Sağlayıcı**|
| --- |
| **AARNET**| 
| **GÉANT aracılığıyla DeIC**|
| **GÉANT aracılığıyla GARR**|
| **GÉANT**|
| **GÉANT aracılığıyla HEAnet**|
| **Internet2**|
| **JISC**|
| **GÉANT aracılığıyla RedIRIS**|
| **SINET**|
| **GÉANT aracılığıyla Surfnet**|

* Bağlantı sağlayıcınız bu listede yoksa, lütfen yukarıda listelenen ExpressRoute Exchange İş Ortaklarından herhangi birine bağlı olup olmadığınızı denetleyin.

## <a name="expressroute-system-integrators"></a>ExpressRoute sistem tümleştiricileri
İhtiyaçlarınıza uyan özel bağlantıyı etkinleştirme ağınızın ölçeğine bağlı olarak zorlu olabilir. ExpressRoute’a yönelik ekleme işleminde size yardımcı olmak üzere aşağıdaki tabloda listelenen herhangi bir sistem tümleştirici ile çalışabilirsiniz.

| **Sistem tümleştirici** | **Continent** |
| --- | --- |
| **[Altogee](https://altogee.be/diensten/express-route/)** | Avrupa |
| **[Avanade Inc.](http://www.avanade.com/)** | Asya, Avrupa, Kuzey Amerika, Güney Amerika |
| **[Bright Skies GmbH](http://bskies.io/expressroute)** | Avrupa
| **[Ensyst](http://www.ensyst.com.au)** | Asya
| **[Equinix Professional Services](http://www.equinix.com/services/consulting/)** | Kuzey Amerika |
| **[FlexManage](http://www.flexmanage.com/cloud)** | Kuzey Amerika |
| **[Inframon](http://www.inframon.com/partner/microsoft/)** | Avrupa |
| **[Lightstream](https://www.ltstream.com/expressroute)** | Kuzey Amerika |
| **[The IT Consultancy Group](http://itconsult.com.au/microsoft-expressroute)** | Avustralya |
| **[MOQdigital](http://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Avustralya |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Avrupa (Almanya) |
| **[Nelite](https://www.nelite.com/offres-services/)** | Avrupa |
| **[New Signature](https://newsignature.com/technologies/express-route/)** | Avrupa |
| **[OneAs1a](http://www.oneas1a.com/connectivity.html)** | Asya |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Avrupa |
| **[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Kuzey Amerika |
| **[Presidio](http://info.presidio.com/microsoft-azure-expressroute)** | Kuzey Amerika |
| **[sol-tec](https://www.sol-tec.com/services)** | Avrupa |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Avustralya |


## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Konum eşleme"
