---
title: Azure standart Load Balancer'a genel bakış | Microsoft Docs
description: Azure Standard Load Balancer özelliklerine genel bakış
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2018
ms.author: kumud
ms.openlocfilehash: f8779af725346a456efe8e718cfc8ff3a91c72fc
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325260"
---
# <a name="azure-load-balancer-standard-overview"></a>Azure Load Balancer Standard genel bakış

Azure Load Balancer, uygulamalarınızı ölçeklendirmenize ve hizmetleriniz için yüksek kullanılabilirlik sağlar. Yük Dengeleyici gelen yanı sıra giden senaryoları için kullanılabilir ve düşük gecikme süreli, yüksek aktarım hızı sağlar ve en fazla akışlar tüm TCP ve UDP uygulamaları için milyonlarca ölçeklendirir. 

Bu makalede standart yük dengeleyici üzerinde odaklanır.  Azure Load Balancer için daha genel bir bakış için gözden [yük dengeleyiciye genel bakış](load-balancer-overview.md) de.

## <a name="what-is-standard-load-balancer"></a>Standard Load Balancer nedir?

Standart Load Balancer, temel Azure Load Balancer'a göre ayarlama genişletilmiş ve daha ayrıntılı bir özellik ile tüm TCP ve UDP uygulamaları için yeni bir yük dengeleyici ürünüdür.  Benzer olsa da, farklılıklar bu makalede ana hatlarıyla öğrenmeniz önemlidir.

Standard Load Balancer ortak veya iç yük dengeleyici olarak kullanabilirsiniz. Ve bir genel ve bir iç yük dengeleyici kaynağı için bir sanal makineye bağlanabilir.

Yük Dengeleyici kaynak işlevleri her zaman bir ön uç, bir kural, bir durum araştırması ve arka uç havuzu tanımı ifade edilir.  Bir kaynağın birden çok kural içerebilir. Sanal makineler, sanal makinenin NIC kaynağı arka uç havuzundan belirterek arka uç havuzuna yerleştirebilirsiniz.  Bu parametre ağ profili geçirilen ve sanal makine ölçek kümeleri kullanırken, genişletilmiş.

Sanal ağın kaynak için temel bir yönü kapsamıdır.  Temel Load Balancer bir kullanılabilirlik kümesi kapsamında bulunmakla birlikte, bir Standard Load Balancer bir sanal ağ kapsamıyla tamamen tümleşiktir ve tüm sanal ağ kavramları uygulayın.

Yük Dengeleyici kaynaklarının içinde Azure oluşturmak istediğiniz senaryoyu elde etmek için çok kiracılı altyapısını nasıl program ifade edebilirsiniz nesneleridir.  Yük Dengeleyici kaynakları ve gerçek altyapınız arasında doğrudan bir ilişki yoktur; bir yük dengeleyici oluşturmaya örneğini oluşturmaz, kapasite her zaman kullanılabilir ve hiçbir başlatma ya da dikkate alınması gereken gecikmeler ölçeklendirme vardır. 

>[!NOTE]
> Azure, tam olarak yönetilen Yük Dengeleme çözümlerinde senaryolarınız için paketi sağlar.  TLS sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği uygulama katmanı işleme başına arıyorsanız, gözden [Application Gateway](../application-gateway/application-gateway-introduction.md).  Arıyorsanız, Genel DNS için Yük Dengeleme, gözden [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Uçtan uca senaryolarınızı gerektiğinde bu çözümleri birleştirme avantajlı olabilir.

## <a name="why-use-standard-load-balancer"></a>Standard Load Balancer neden kullanmalısınız?

Standart Load Balancer uygulamalarınızı ölçeklendirmenizi ve küçük ölçekli dağıtımlardan büyük ve karmaşık çok bölgeli mimarilere kadar yüksek düzeyde kullanılabilirlik oluşturmanızı sağlar.

Standard Load Balancer ile temel yük dengeleyici arasındaki farklar genel bir bakış için aşağıdaki tabloyu gözden geçirin:

>[!NOTE]
> Yeni Tasarım, Standard Load Balancer benimseyin. 

[!INCLUDE [comparison table](../../includes/load-balancer-comparison-table.md)]

Gözden geçirme [yük dengeleyici için hizmet sınırları](https://aka.ms/lblimits), yanı [fiyatlandırma](https://aka.ms/lbpricing), ve [SLA](https://aka.ms/lbsla).


### <a name="backend"></a>Arka uç havuzu

Standart yük dengeleyici arka uç havuzları bir sanal ağdaki herhangi bir sanal makine kaynağa genişletir.  Bu, en fazla 1000 arka uç örnekleriyle içerebilir.  Bir NIC kaynağı, bir özellik olan IP yapılandırması, bir arka uç örneğidir.

Tek başına sanal makineler, kullanılabilirlik kümeleri veya sanal makine ölçek kümeleri, arka uç havuzu içerebilir.  Ayrıca, arka uç Havuzu'ndaki kaynakları karıştırabilirsiniz. Arka uç havuzundaki yük dengeleyici kaynak başına en fazla 150 kaynakları birleştirebilirsiniz.

Arka uç havuzu tasarlamak nasıl değerlendirirken için en az tasarlayabilirsiniz. daha fazla yönetim işlemlerinin süresini iyileştirmek için tek bir arka uç havuzu kaynakları sayısı.  Veri düzlemi performansı veya ölçeği fark yoktur.

## <a name="az"></a>Kullanılabilirlik alanları

Standart Load Balancer, kullanılabilirlik alanları kullanılabilir olduğu bölgelerde ek yetenekler destekler.  Bu özellikler, tüm standart yük dengeleyici için artımlı sağlar.  Kullanılabilirlik yapılandırmaları için genel ve iç standart yük dengeleyici kullanılabilir.

Bölgesel olmayan ön uçlar kullanılabilirlik alanları ile bir bölgede dağıtılan, varsayılan olarak bölgesel olarak yedekli hale gelir.   Bölgesel olarak yedekli bir ön uç, bölge hatası devam eder ve tüm bölgelerde adanmış altyapı tarafından aynı anda sunulur. 

Ayrıca, bir ön uç, belirli bir bölgeye garanti edebilir. Bölgesel bir ön uç kader ilgili bölgeyle paylaşır ve yalnızca tek bir bölge içinde adanmış altyapı tarafından sunulur.

Bölgeler arası Yük Dengeleme arka uç havuzu için sağlanır ve herhangi bir sanal ağ içindeki sanal makine kaynağı bir arka uç havuzunun parçası olabilir.

Gözden geçirme [kullanılabilirlik alanları ayrıntılı bir açıklaması becerileriyle ilgili](load-balancer-standard-availability-zones.md).

### <a name="diagnostics"></a> Tanılama

Standart yük dengeleyici Azure İzleyicisi üzerinden çok boyutlu ölçümleri sağlar.  Bu ölçümler, gruplandırılmış ve belirli bir boyut için ayrıştırılmış filtrelendi.  Bunlar, performans ve sistem durumu hizmeti güncel ve geçmiş bilgiler sağlar.  Kaynak durumu da desteklenir.  Desteklenen tanılama kısa bir genel bakış aşağıda verilmiştir:

| Ölçüm | Açıklama |
| --- | --- |
| VIP kullanılabilirlik | Load Balancer standart sürekli olarak veri yolundan bir bölge içindeki sanal makinenizin destekleyen tüm SDN yığınını için ön uç yük dengeleyiciye uygular. Sağlıklı örnekleri kaldığı sürece, ölçüm olarak uygulamanızın yük dengeli trafik aynı yolu izler. Ayrıca, müşterileriniz tarafından kullanılan veri yolu doğrulanır. Ölçüm uygulamanıza görünmez ve diğer işlemlerle etkilemez.|
| DIP kullanılabilirlik | Load Balancer standart, uygulama uç noktasının yapılandırma ayarlarınıza göre izler hizmeti yoklama dağıtılmış bir sistem durumu kullanır. Bu ölçüm bitiş noktası filtrelenmiş görünüm her bağımsız örnek uç noktası yük dengeleyicide başına havuz ya da bir toplama sağlar.  Yük Dengeleyici sistem durumunu, sistem durumu araştırması yapılandırması tarafından belirtildiği gibi uygulamanızın nasıl görüntülediğine görebilirsiniz.
| SYN paketleri | Load Balancer standart TCP bağlantılarını sonlandırmak veya desteklemez TCP veya UDP paket akışları ile etkileşim. Akışlar ve bunların el sıkışmaları her zaman kaynağı ile sanal makine örneği arasındadır. TCP protokolü senaryolarınızı daha iyi gidermek için SYN kullanmak yapabileceğiniz kaç TCP bağlantısı anlamak için paket sayaçları denemesi yapıldı. Ölçüm alınan TCP SYN paketlerin sayısını raporlar.|
| SNAT bağlantıları | Load Balancer standart genel IP adresi ön uç verdiğinizi giden akışlar sayısını raporlar. SNAT bağlantı noktaları exhaustible bir kaynaktır. Bu ölçüm, ne kadar yoğun olarak uygulamanız üzerinde SNAT giden kaynaklı akışlar için güvenmektedir bir göstergesini verebilirsiniz.  Başarılı ve başarısız giden SNAT akışlar için sayaçları bildirilir ve sorun giderme ve giden akış durumunu anlamak için kullanılabilir.|
| Bayt sayaçları | Load Balancer standart, ön uç başına işlenen veri bildirir.|
| Paket sayaçları | Load Balancer standart, ön uç başına işlenen paket bildirir.|

Gözden geçirme [detaylı tartışılması standart yük dengeleyici tanılama](load-balancer-standard-diagnostics.md).

### <a name="haports"></a>HA bağlantı noktaları

Standart Load Balancer, yeni bir kural türünü destekler.  

Yük Dengeleme kuralları, uygulama ölçeklendirme yapın ve yüksek oranda güvenilir yapılandırabilirsiniz. Akış Yük Dengeleme her kısa ömürlü bir iç standart Load Balancer'ın ön uç IP bağlantı noktası üzerinde başına bir HA bağlantı noktaları Yük Dengeleme kuralı, standart Load Balancer kullanırken sağlar.  Bu özellik, pratik ya da tek tek bağlantı noktalarını belirlemek için istenmeyen olduğu diğer senaryolar için kullanışlıdır.

Bir HA bağlantı noktaları Yük Dengeleme kuralı, ağ sanal Gereçleri ve gelen bağlantı noktalarının büyük aralıklar gerektiren tüm uygulama için Aktif-Pasif veya aktif-aktif n + 1 senaryoları oluşturmanıza olanak sağlar.  Durum araştırması, hangi arka uçlar yeni akışlar almalıdır belirlemek için kullanılabilir.  Bir bağlantı noktası aralığı senaryo benzetmek için bir ağ güvenlik grubu kullanabilirsiniz.

>[!IMPORTANT]
> Bir ağ sanal Gereci kullanmayı planlıyorsanız, ürün HA bağlantı noktaları ile edilmiş olan hakkında rehberlik için satıcınıza başvurun ve uygulama için kendi özel yönergeleri izleyin. 

Gözden geçirme [detaylı tartışılması HA bağlantı noktaları](load-balancer-ha-ports-overview.md).

### <a name="securebydefault"></a>Varsayılan olarak güvenli

Standart Load Balancer sanal ağın tam olarak eklendikten.  Sanal ağ özel, kapalı bir ağdır.  Standart Load balancer'ları ve standart genel IP adresleri bu sanal ağa sanal ağ dışında erişilmesine izin verecek şekilde tasarlandığından, bu kaynakları artık açmadan sürece varsayılan olarak. Bu, ağ güvenlik grupları (Nsg'ler) artık açıkça izin vermek için kullanılır ve beyaz liste trafiğe izin anlamına gelir.  Tüm sanal veri merkezi oluşturmak ve NSG'de nedir ve ne zaman kullanılabilir olması karar verebilirsiniz.  Bir NSG bir alt ağdaki veya NIC, sanal makine kaynağı, yoksa trafiği bu kaynağa erişmek için izin verilmiyor.

Nsg'leri ve bunları senaryonuz için uygulama hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/security-overview.md).

### <a name="outbound"></a> Giden bağlantılar

Yük Dengeleyici, gelen ve giden senaryolarını destekler.  Standart yük dengeleyici giden bağlantıları ile ilgili temel yük dengeleyici için önemli ölçüde farklılık gösterir.

Kaynak ağ adresi çevirisi (SNAT), sanal ağınızda özel, iç IP adreslerini ön yük dengeleyicinin genel IP adreslerinde eşleştirmek için kullanılır.

Standart yük dengeleyici için yeni bir algoritma tanıtır bir [daha sağlam, ölçeklenebilir ve tahmin edilebilir SNAT algoritması](load-balancer-outbound-connections.md#snat) ve yeni yetenekleri sağlar ve kaldırır belirsizlik zorlar açık yapılandırmaları etkileri yan yerine. Bu değişiklikler, yeni özellikler ortaya çıkmaya başladı izin vermek gereklidir. 

Standart Load Balancer ile çalışırken hatırlamak anahtar sacayakları şunlardır:

- tamamlandığında bir kuralı, yük dengeleyici kaynağını beraberinde getirir.  Azure'nın tüm programlama yapılandırmasını türetilir.
- birden çok ön uç kullanılabilir olduğunda, tüm ön kullanılır ve her ön uç kullanılabilir SNAT bağlantı noktalarının sayısı ile çarpar.
- seçin ve giden bağlantılar için kullanılacak belirli bir ön uç için istemiyorsanız kontrol edebilirsiniz.
- Giden senaryoları açık ve onu belirtilen kadar giden bağlantı yok.
- Yük Dengeleme kuralları nasıl SNAT programlanmıştır çıkarsayın. Yük Dengeleme kuralları belirli bir protokol olan. SNAT belirli bir protokoldür ve yapılandırma bunu yansıtmak yerine bir yan etkisi oluşturun.

#### <a name="multiple-frontends"></a>Birden çok ön uç
Görmeyi veya zaten giden bağlantılar için çok fazla istek yaşayan olan olduğundan daha fazla SNAT bağlantı noktaları isterseniz de artımlı SNAT bağlantı noktası Envanter ek ön uçlar, kuralları ve arka uç havuzları aynı sanal makineye yapılandırarak ekleyebilirsiniz kaynaklar.

#### <a name="control-which-frontend-is-used-for-outbound"></a>Hangi ön uç için kullanılan denetim giden
Yalnızca belirli ön uç IP adresinden kaynaklanan giden bağlantıları kısıtlamak istiyorsanız, isteğe bağlı olarak ifade giden eşleme kuralı üzerinde giden SNAT devre dışı bırakabilirsiniz.

#### <a name="control-outbound-connectivity"></a>Giden bağlantı denetimi
Standart Load Balancer sanal ağ bağlamında var.  Bir sanal ağ yalıtılmış, özel bir ağdır.  Genel bir IP adresi ile bir ilişki mevcut değilse, genel bağlantı izin verilmiyor.  Size ulaşabildiğimizden [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) içini ve yerel sanal ağ olduğundan.  Sanal ağınızın dışında bir hedefe giden bağlantı kurmasını istiyorsanız iki seçeneğiniz vardır:
- Örnek düzeyi genel IP adresi için sanal makine kaynağı olarak bir standart SKU genel IP adresi atayın veya
- Genel bir Standard Load Balancer arka uç havuzundaki sanal makine kaynağı yerleştirin.

Her ikisi de, sanal ağ dışındaki sanal ağa giden bağlantısı izin verir. 

Varsa, _yalnızca_ bir iç standart yük sanal makine kaynağınıza bulunduğu arka uç havuzuyla ilişkili dengeleyici varsa, sanal makinenizi yalnızca sanal ağ kaynakları ulaşabilir ve [VNet hizmeti Uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).  Giden bağlantı oluşturmak için önceki paragrafta açıklanan adımları takip edebilirsiniz.

Giden bağlantı önce standart SKU'ları kalır ile ilişkili olmayan bir sanal makine kaynağı.

Gözden geçirme [detaylı tartışılması giden bağlantılar](load-balancer-outbound-connections.md).

### <a name="multife"></a>Birden çok ön uç
Load Balancer, birden çok ön uç ile birden çok kural destekler.  Standart yük dengeleyici giden senaryoları için bu genişletir.  Temel olarak gelen bir Yük Dengeleme kuralı tersini giden senaryolardır.  Gelen Yük Dengeleme kuralını de giden bağlantılar için bir ilişkilendirme oluşturur. Standart Load Balancer, Yük Dengeleme kuralı ile bir sanal makine kaynakla ilişkili tüm ön kullanır.  Ayrıca, bir parametre, Yük Dengeleme kuralı ve Yük Dengeleme kuralı hiçbiri dahil olmak üzere belirli ön uçlar seçmeye imkan tanıyan giden bağlantı amacıyla bastırmak sağlar.

Karşılaştırma için temel yük dengeleyici tek bir ön uç rastgele seçer ve hangisinin Seçili denetim olanağı yoktur.

Gözden geçirme [detaylı tartışılması giden bağlantılar](load-balancer-outbound-connections.md).

### <a name="operations"></a> Yönetim işlemleri

Standart yük dengeleyici kaynakları tamamen yeni bir altyapı platformda mevcut.  Bu standart SKU'lar için önemli ölçüde daha hızlı yönetim işlemlerini etkinleştirir ve tamamlanma sürelerini genellikle 30 saniyeden kısa bir süre standart SKU kaynak başına bulunurlar.  Arka uç havuzları boyutu arttıkça, arka ucu için gerekli süre havuzu da artış değiştiğine dikkat edin.

Standart yük dengeleyici kaynakları değiştirebilir ve bir standart genel IP adresini bir sanal makineden daha hızlı bir şekilde diğerine taşıma.

## <a name="migration-between-skus"></a>SKU'ları arasında geçiş

SKU'ları değişebilir değildir. Bir kaynak SKU diğerine taşımak için bu bölümdeki adımları izleyin.

>[!IMPORTANT]
>Bu belgede bir SKU'ları arasındaki farkları ve kendi senaryonuza dikkatlice inceleyen tamamen gözden geçirin.  Senaryonuz hizalamak için ek değişiklikler yapmanız gerekebilir.

### <a name="migrate-from-basic-to-standard-sku"></a>Temel katmandan standart SKU için geçirme

1. Yeni bir standart kaynak (yük dengeleyici ve genel gerektiğinde IP'ler) oluşturun. Kuralları yeniden oluşturun ve tanımları araştırma.

2. Yeni veya mevcut bir NSG NIC veya alt ağ vermek istediğiniz diğer tüm trafiğin yanı sıra beyaz liste yük dengeli trafiği, araştırma güncelleştirilemiyor.

3. Temel SKU kaynakları (yük dengeleyici ve uygunsa genel IP'ler) tüm VM örneklerinden kaldırın. Bir kullanılabilirlik kümesindeki tüm VM örneklerini de kaldırdığınızdan emin olun.

4. Tüm sanal makine örnekleri için yeni standart SKU kaynakları ekleyin.

### <a name="migrate-from-standard-to-basic-sku"></a>Standart katmandan temel SKU için geçirme

1. Yeni bir temel kaynak (yük dengeleyici ve genel gerektiğinde IP'ler) oluşturun. Kuralları yeniden oluşturun ve tanımları araştırma. 

2. Standart SKU kaynakları (yük dengeleyici ve uygunsa genel IP'ler) tüm VM örneklerinden kaldırın. Bir kullanılabilirlik kümesindeki tüm VM örneklerini de kaldırdığınızdan emin olun.

3. Tüm sanal makine örnekleri için yeni temel SKU kaynakları ekleyin.

>[!IMPORTANT]
>
>Temel ve standart SKU'lar kullanımıyla ilgili sınırlamalar vardır.
>
>HA bağlantı noktaları ve Tanılama, standart SKU yalnızca standart SKU'da kullanılabilir. Standart SKU için temel SKU geçirme ve ayrıca bu özelliklerini korumak olamaz.
>
>Temel ve standart SKU şu makalede açıklanan birkaç fark sahip.  Anlama ve bunlar için hazırlama emin olun.
>
>Eşleşen SKU yük dengeleyici ve genel IP kaynakları için kullanılması gerekir. Temel SKU ve standart SKU kaynaklarının karışımına sahip olamaz. Tek başına sanal makineleri, bir kullanılabilirlik kümesi kaynağındaki sanal makineleri veya sanal makine ölçek kümesi kaynaklarını aynı anda iki SKU’ya iliştiremezsiniz.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Load Balancer standart tüm genel bulut bölgelerinde kullanılabilir.

## <a name="sla"></a>SLA

Standart Load Balancer, % 99,99 SLA ile kullanılabilir.  Gözden geçirme [standart yük dengeleyici SLA](https://aka.ms/lbsla) Ayrıntılar için.

## <a name="pricing"></a>Fiyatlandırma

Standart Load Balancer, Yük Dengeleme kuralları yapılandırılmış ve işlenen tüm gelen ve giden veri sayısına göre ücretlendirilir bir üründür. Standart fiyatlandırma bilgileri için yük dengeleyici, ziyaret [Load Balancer fiyatlandırması](https://aka.ms/lbpricing) sayfası.

## <a name="limitations"></a>Sınırlamalar

- SKU'ları değişebilir değildir. Mevcut bir kaynağı SKU'su değiştiremeyebilir.
- Tek başına sanal makine kaynağı, kaynak kullanılabilirlik kümesi veya sanal makine ölçek kümesi kaynak bir SKU, hiçbir zaman hem de başvurabilirsiniz.
- Bir yük dengeleyici kuralı, iki sanal ağlara yayılamaz.  Ön uçlar ve bunların ilgili arka uç örnekleriyle aynı sanal ağda bulunması gerekir.  
- Yük Dengeleyici ön uçlar küresel sanal ağ eşlemesi üzerinden erişilebilir.
- [Abonelik işlemleri taşıma](../azure-resource-manager/resource-group-move-resources.md) standart SKU LB ve PIP kaynaklar için desteklenmez.
- Web çalışanı rolü bir sanal ağ ve diğer Microsoft platform Hizmetleri olmadan pre-sanal ağ hizmetleri ve platform hizmetleri nasıl işlevi gelen bir yan etkisi nedeniyle yalnızca bir iç standart yük dengeleyici kullanıldığında erişilebilir. Kendisinin veya arka plandaki ilgili hizmet olarak, bu durumda değil platform bildirim olmadan değiştirebilirsiniz. Oluşturmanız gereken her zaman varsaymalısınız [giden bağlantı](load-balancer-outbound-connections.md) açıkça bir iç standart yük dengeleyici yalnızca kullanırken isterseniz.
- Yük Dengeleyici, Yük Dengeleme ve bu belirli IP protokolleri için bağlantı noktası iletme için TCP veya UDP bir üründür.  Yük Dengeleme kuralları ve gelen NAT kuralları TCP ve UDP için desteklenen ve ICMP gibi diğer IP protokolleri için desteklenmiyor. Aksi takdirde bir UDP veya TCP akışı yükü ile etkileşime veya yanıt veya yük dengeleyici değil sonlandır. Bir proxy değil. Bir ön uç bağlantı başarılı doğrulama gerçekleştirmeniz gereken bant bağlantısı ile bir Yük Dengeleme veya gelen NAT kuralı (TCP veya UDP) kullanılan aynı protokol _ve_ en az bir sanal makinelerinizin gerekir oluşturmak bir yanıt için bir istemci bir ön uç yanıtı görmek için.  Ön uç yük Dengeleyiciden bir bant dışı yanıt almadıktan hiçbir sanal makine yanıt verebilmesi gösterir.  Bir yük dengeleyici sanal makine yanıt verebilmesi ön uç ile etkileşim kurmak mümkün değildir.  Bu durum giden bağlantılar için de geçerlidir burada [bağlantı noktası maske SNAT](load-balancer-outbound-connections.md#snat) olduğu TCP ve UDP; yalnızca ICMP dahil olmak üzere diğer IP protokolleri de başarısız olur.  Azaltmak için bir örnek düzeyi genel IP adresi atayın.
- Sağlayan ortak yük Dengeleyiciler aksine [giden bağlantılar](load-balancer-outbound-connections.md) sanal ağ içindeki özel IP adresleri için ortak IP adresleri aşamasından geçme, iç yük dengeleyici giden çevrilmemesine kaynağı ön uç için her ikisi de olarak bir iç yük dengeleyicisinin özel IP adres alanı bağlantılardır.  Bu çeviri gerekli olduğu değil, benzersiz, iç IP adresi alanı içindeki SNAT tükenmesi olasılığını ortadan kaldırır.  Arka uç havuzundaki bir VM'den giden bir akışı hangi havuzda bulunduğu iç yük dengeleyicinin ön uç bir akışa çalışırsa, yan etkisi olan _ve_ eşlendi geri kendisine, akışın iki Bacak eşleşmiyor ve akışın başarısız olur .  Akış ön uç için akışı oluşturduğunuz arka uç havuzundaki aynı sanal makine için yeniden eşleyemiyorsanız, akışın başarılı olur.   Kendisine geri akışı eşler giden akış VM'den ön uç için oluşmuş görünür ve karşılık gelen akışta VM'den kendisine oluşmuş görünür. Konuk işletim sisteminin açısından bakıldığında, sanal makinenin içinde aynı akışın gelen ve giden bölümleri eşleşmiyor. TCP yığınına, kaynak ve hedef eşleşmeyen gibi aynı akışı parçası olacak şekilde bu yarısının aynı akışı tanımaz.  Arka uç havuzundaki herhangi bir VM akışı eşlendiği akışı yarısının eşleşir ve VM akışa başarılı bir şekilde yanıt verebilir.  Bu senaryo için aralıklı bağlantı zaman aşımları belirtisidir. Bu senaryo güvenilir bir şekilde elde etmek için kullanabileceğiniz birkaç yaygın geçici çözümler vardır (akışlar bir arka uç havuzundan arka uç için kaynak havuzları ilgili iç yük dengeleyici ön uç) içeren bir üçüncü taraf proxy'nin arkasında iç yük ya da ekleme Dengeleyici veya [DSR stili kurallarını kullanarak](load-balancer-multivip-overview.md).  Elde edilen senaryo azaltmak için bir genel yük dengeleyici kullanabilirken potansiyeli [SNAT tükenmesi](load-balancer-outbound-connections.md#snat) ve dikkatli bir şekilde yönetilen sürece kaçınılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanma hakkında bilgi edinin [standart Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)
- Daha fazla bilgi edinin [kullanılabilirlik](../availability-zones/az-overview.md).
- Hakkında bilgi edinin [standart yük dengeleyici tanılama](load-balancer-standard-diagnostics.md).
- Hakkında bilgi edinin [çok boyutlu ölçümler desteklenen](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) tanılama yenilikleri [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md).
- Kullanma hakkında bilgi edinin [giden bağlantılar için yük dengeleyici](load-balancer-outbound-connections.md)
- Hakkında bilgi edinin [standart Load Balancer ile yüksek kullanılabilirlik bağlantı noktaları Yük Dengeleme kuralları](load-balancer-ha-ports-overview.md)
- Kullanma hakkında bilgi edinin [birden çok ön uç yük Dengeleyiciyle](load-balancer-multivip-overview.md)
- Hakkında bilgi edinin [sanal ağlar](../virtual-network/virtual-networks-overview.md).
- Daha fazla bilgi edinin [ağ güvenlik grupları](../virtual-network/security-overview.md).
- Hakkında bilgi edinin [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
- Başka bir tuşa bazıları hakkında bilgi edinin [ağ özelliklerinden](../networking/networking-overview.md) azure'da.
- Daha fazla bilgi edinin [yük dengeleyici](load-balancer-overview.md).
