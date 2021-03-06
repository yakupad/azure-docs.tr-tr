---
title: Service Fabric kümesi kapasite planlaması | Microsoft Docs
description: Service Fabric kümesi kapasite planlaması konuları. NodeType, işlemler, dayanıklılık ve güvenilirlik katmanları
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: ''
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: chackdan
ms.openlocfilehash: ae670eca3d655e16ddf55da2e2538ba96b7e0115
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126060"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Service Fabric kümesi kapasite planlaması konuları
Herhangi bir üretim dağıtımı için kapasite planlaması önemli bir adımdır. Bu işlemin bir parçası olarak dikkate almanız gereken öğelerden bazıları aşağıda verilmiştir.

* Düğüm türleri kümenizi ile başlatmak için gereken sayısı
* Her düğüm türünün (boyut, birincil, internet'e yönelik, sayısı VM'ler gibi) özellikleri
* Kümenin güvenilirlik ve dayanıklılık özellikleri

> [!NOTE]
> En düşük düzeyde tüm gözden geçirmeniz gereken **izin** planlaması sırasında ilke değerleri yükseltin. Değerleri uygun şekilde ayarlandığından emin olun ve kümeniz daha sonra değiştirilemez sistem yapılandırma ayarları nedeniyle yazma azaltmak için budur. 
> 

Bize kısaca bu öğelerin her birini gözden geçirin.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Düğüm türleri kümenizi ile başlatmak için gereken sayısı
Öncelikle, oluşturmakta olduğunuz küme için kullanılacak neler olduğunu şekil gerekir.  Ne tür uygulamaların bu kümesi dağıtmayı planlıyor musunuz? Küme amacına açık değilse, en olası olmayan henüz kapasite planlama işlemi girmek için hazır.

Düğüm türleri kümenizi ile başlatmak için gereken sayıda kurun.  Her düğüm türü, bir sanal makine ölçek kümesine eşlenir. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Bu nedenle düğüm türü sayısı kararı temelde aşağıdaki önemli noktalar için gelir:

* Uygulamanızı birden çok hizmetlere sahip olmadığı ve herhangi biri genel veya internet'e yönelik olmam gerekir mi? Normal uygulama, istemciden gelen giriş alan bir ön uç ağ geçidi hizmeti ve iletişim kuran bir veya daha fazla arka uç Hizmetleri ile ön uç hizmetleri içerir. Bu nedenle bu durumda, en az iki düğüm türleri sonunda.
* Farklı altyapı gereksinimleri gibi daha büyük RAM veya daha yüksek CPU çevrimleri (uygulamanızı ayarlama yapan) hizmetlerinizi var mı? Örneğin, dağıtmak istediğiniz uygulamanın ön uç hizmeti ve arka uç hizmeti içerdiğini bize varsayın. Ön uç hizmeti (örneğin, D2 VM boyutları) bağlantı noktalarının açık internet'e sahip daha küçük sanal makinelerinde çalıştırabilirsiniz.  Arka uç hizmeti, hesaplama yoğun ve internet olmayan daha büyük Vm'leri (D4, D6, D15 gibi VM boyutlarıyla) çalıştırma gerekiyor ancak yaşıyor.
  
  Bir düğüm türünde, tüm hizmetleri koymak karar verebilirsiniz ancak bu örnekte, bunları bir kümede iki düğüm türleri ile yerleştirmenizi öneririz.  Bu, her düğüm türü, internet bağlantısı veya VM boyutu gibi farklı özelliklere sahip olmasını sağlar. VM sayısı ölçeklendirilebilir bağımsız olarak da.  
* Geleceği tahmin etmeyi çünkü bildiğiniz bilgileri gidin ve uygulamalarınız ile başlaması gereken düğüm türleri sayısını seçin. Her zaman ekleyebilir veya daha sonra düğüm türlerini kaldırın. Service Fabric kümesi en az bir düğüm türü olmalıdır.

## <a name="the-properties-of-each-node-type"></a>Her düğüm türünün özellikleri
**Düğüm türü** bulut hizmetlerindeki roller gibi çalışır görülebilir. Düğüm türleri VM boyutlarını, VM'lerin sayısını ve bunların özelliklerini tanımlayın. Bir Service Fabric kümesinde tanımlanan her düğüm türü eşlenen bir [sanal makine ölçek kümesi](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).  
Her düğüm türü ayrı bir ölçek kümesi ölçeklendirilebilir veya Aşağı bağımsız olarak, farklı bağlantı noktası kümeleri açık olan ve farklı kapasite ölçümleri yapılabilir ' dir. Düğüm türleri ve sanal makine ölçek kümeleri arasındaki ilişkiler hakkında daha fazla bilgi için nasıl örneklerinden birinde RDP için yeni Aç bağlantı noktaları ve benzeri bkz [Service Fabric küme düğümü türlerini](service-fabric-cluster-nodetypes.md).

Service Fabric kümesi birden fazla düğüm türü oluşabilir. Bu olay tek bir birincil düğüm türü küme oluşur ve bir veya daha fazla birincil olmayan düğüm türleri.

Tek bir düğüm türü, yalnızca sanal makine ölçek kümesi başına 100 düğüm aşamaz. Sanal makine ölçek kümeleri hedeflenen ölçeğe ulaşmak için ve otomatik ölçeklendirme olamaz automagically eklemeniz gerekebilir sanal makine ölçek kümeleri ekleyin. Yerinde sanal makine ölçek kümeleri ekleme dinamik bir kümeye bir görevdir ve yaygın olarak kullanıcıların yeni küme oluşturma sırasında sağlanan uygun düğümü türleri ile sağlama sonuçlanır. 

### <a name="primary-node-type"></a>Birincil düğüm türü

Service Fabric sistem hizmetlerinin (örneğin, Küme Yöneticisi hizmeti veya görüntü Store hizmet) birincil düğüm türünde yerleştirilir. 

![İki düğüm türleri olan bir küme ekran görüntüsü][SystemServices]

* **VM boyutu için alt** için birincil düğüm türü tarafından belirlenir **dayanıklılık katmanı** belirleyin. Varsayılan dayanıklılık katmanı Bronz gösterir. Bkz: [kümenin dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) daha fazla ayrıntı için.  
* **En düşük VM sayısı** için birincil düğüm türü tarafından belirlenir **güvenilirlik katmanını** belirleyin. Varsayılan güvenilirlik katmanını Silver ' dir. Bkz: [güvenilirliği kümenin](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) daha fazla ayrıntı için.  

Azure Resource Manager şablonundan birincil düğüm türü ile yapılandırılmış `isPrimary` altında özniteliği [düğüm türü tanımı](https://docs.microsoft.com/en-us/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object).

### <a name="non-primary-node-type"></a>Olmayan birincil düğüm türü

Birden çok düğüm türü ile bir kümede, rest, birincil olmayan ve bir birincil düğüm türü yoktur.

* **VM boyutu için alt** için birincil olmayan düğüm türleri tarafından belirlenir **dayanıklılık katmanı** belirleyin. Varsayılan dayanıklılık katmanı Bronz gösterir. Bkz: [kümenin dayanıklılık özelliklerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) daha fazla ayrıntı için.  
* **En düşük VM sayısı** birincil olmayan düğüm türlerinde biridir. Ancak, bu düğüm türü çalıştırmak istediğiniz uygulama/hizmetleri çoğaltmasına göre bu sayı seçmeniz gerekir. Küme dağıttıktan sonra bir düğüm türünde VM sayısı artırılabilir.

## <a name="the-durability-characteristics-of-the-cluster"></a>Kümenin dayanıklılık özellikleri
Dayanıklılık katmanı, sanal makinelerinizin temel Azure altyapısıyla sahip ayrıcalıkları sisteme belirtmek için kullanılır. Birincil düğüm türü, sistem hizmetleri ve durum bilgisi olan hizmetlerinizin çekirdek gereksinimleri etkileyen tüm VM düzeyi altyapı istek (örneğin, bir VM yeniden başlatma, VM yeniden görüntü oluşturma ya da VM geçişi) duraklatmak Service Fabric bu ayrıcalığı sağlar. Birincil olmayan düğüm türleri bu ayrıcalık, durum bilgisi olan hizmetler için çekirdek gereksinimleri etkileyen tüm VM düzeyi altyapı istekler (örneğin, VM yeniden başlatma, VM yeniden görüntü oluşturma ve VM geçişi) duraklatmak Service Fabric verir.

| Dayanıklılık katmanı  | Gerekli en düşük VM sayısı | Desteklenen VM SKU'ları                                                                  | VMSS için yaptığınız güncelleştirmeler                               | Güncelleştirmeleri ve Azure tarafından başlatılan bakım                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Altın             | 5                              | Tek bir müşteriye (örneğin, L32s GS5, G5, DS15_v2, D15_v2) ayrılmış tam düğümlü SKU'ları | Service Fabric kümesi tarafından onaylanmış kadar ertelendi | Önceki hatalardan kurtarmak çoğaltmaları için ek süre vermek amacıyla UD başına 2 saat için duraklatıldı |
| Gümüş           | 5                              | Sanal makineleri tek çekirdekli veya üzeri                                                        | Service Fabric kümesi tarafından onaylanmış kadar ertelendi | Önemli bir süre boyunca Gecikmeli                                                    |
| Bronz           | 1                              | Tümü                                                                                | Service Fabric kümesi tarafından Gecikmeli değil           | Önemli bir süre boyunca Gecikmeli                                                    |

> [!WARNING]
> Çalışan Bronz dayanıklılığa sahip düğüm türleri elde _ayrıcalıkların olmadığı_. Bu durum bilgisiz iş yüklerinizi etkileyen altyapı işler değil durduruldu veya kaldırılacak geciktirilmiş, hangi iş yüklerinizi etkileyebilecek anlamına gelir. Yalnızca Bronz yalnızca durum bilgisiz iş yükleri çalıştıran düğümü türleri için kullanın. Üretim iş yükleri çalıştıran Silver veya yukarıda önerilir. 
>

**Silver veya Gold dayanıklılık düzeyleri kullanmanın avantajları**
 
- Bir ölçeklendirme işlemi gereken adım sayısını azaltır (diğer bir deyişle, düğümü devre dışı bırakma ve kaldırma ServiceFabricNodeState çağırıldı otomatik olarak).
- Bir müşteri tarafından başlatılan yerinde VM SKU değiştirme işlemi ya da Azure altyapı işlemler nedeniyle veri kaybı riskini azaltır.

**Silver veya Gold dayanıklılık düzeyleri kullanmanın dezavantajları**
 
- Dağıtımlar, sanal makine ölçek kümesi ve diğer ilgili Azure kaynaklarını Gecikmeli, zaman aşımına uğrayabilir veya tamamen sorunları, kümenizdeki veya altyapı düzeyinde tarafından engellendi. 
- Sayısını artıran [çoğaltma yaşam döngüsü olaylarını](service-fabric-reliable-services-lifecycle.md) (örneğin, birincil takasları) nedeniyle Azure altyapı işlemleri sırasında düğüm deactivations otomatik.
- Azure platformu yazılım güncelleştirme veya donanım bakım etkinlikleri gerçekleşen sırasında süreler için hizmet düğümlerinde alır. Bu etkinlikleri sırasında devre dışı bırakılması/devre dışı durumuna sahip düğümleri görebilirsiniz. Bu, kümenizin kapasitesi geçici olarak azaltır, ancak kullanılabilirlik kümesi veya uygulamaları etkilememesi gerekir.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Silver veya Gold dayanıklılık düzeyleri kullanıldığı durumlar için öneriler

Silver veya Gold dayanıklılık beklediğiniz ölçek için durum bilgisi olan hizmetler barındıran tüm düğüm türleri için kullanın (VM örneği sayısını azaltmak) sık sık ve dağıtım işlemlerini geciktirileceği tercih ediyorsanız ve bu ölçek açma basitleştirme yerine getirilmesini kapasite işlemler. (VM içeren örneklere ekleme) ölçek genişletme senaryoları dayanıklılık katmanı ettiğiniz yürütülmez, yalnızca ölçek de yapar.

### <a name="changing-durability-levels"></a>Dayanıklılık düzeylerini değiştirme
- Silver veya Gold dayanıklılık düzeyine sahip düğüm türleri için Bronz düşürülemez.
- Silver veya Gold Bronz ' yükseltme birkaç saat sürebilir.
- Dayanıklılık düzeyini değiştirirken, hem Service Fabric uzantısı yapılandırmasında sanal makine ölçek kümesi kaynağınıza ve Service Fabric kümesi kaynağınızın düğüm türü tanımında güncelleştirdiğinizden emin olun. Bu değerlerin eşleşmesi gerekir.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Düğümü için işletimsel önerileri için silver veya gold dayanıklılık düzeyi ayarlamak yazın.

- Küme ve uygulamalar her zaman durumunun iyi kalmasını sağlamak ve uygulamalar için tüm yanıt emin [hizmet çoğaltması yaşam döngüsü olaylarını](service-fabric-reliable-services-lifecycle.md) (derleme çoğaltma takılmış gibi) zamanında.
- (Ölçek artırılabilir/azaltılabilir) VM SKU değişiklik yapmak için daha güvenli şekilde benimseme: bir sanal makine ölçek kümesi sanal makine SKU'su değiştirme doğası gereği güvenli olmayan bir işlemdir ve bu nedenle, mümkünse kaçınılmalıdır. Sık karşılaşılan sorunları önlemek için izlemeniz gereken süreç şöyledir.
    - **Birincil olmayan düğüm türleri:** önerilen yeni sanal makine ölçek kümesi oluşturma, yeni sanal makine ölçek kümesi/düğüm türü eklemek ve ardından eski sanal makine ölçek kümesi örneği azaltmak için hizmet yerleştirme kısıtlaması Değiştir 0 (düğümlerin kaldırılması kümenin güvenilirlik etkilemez emin olmak için budur) bir anda bir düğüm sayısı.
    - **Birincil düğüm türü için:** birincil düğüm türündeki sanal makine SKU'su değiştirmeyin bizim önerilir. Birincil düğüm türü SKU desteklenmiyor değiştiriliyor. Kapasite yeni SKU sebebi, daha fazla örnek eklenmesi önerilir. Bu mümkün değil, yeni küme oluşturma ve [uygulama durumunu geri yükle](service-fabric-reliable-services-backup-restore.md) (varsa) eski kümenizden. Herhangi bir sistem hizmet durumunu geri yüklemek gerekmez, uygulamalarınızı yeni kümenize dağıttığınızda oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların kümenizde çalışan uygulamalarınızı yeni kümeye dağıtın, geri yüklemek için hiçbir şey vardır. Desteklenmeyen bir rotayı ve sanal makine SKU'su değiştirmek istediğiniz karar verirseniz, ardından belgelenir sanal makine ölçek kümesi yeni SKU yansıtacak şekilde Model tanımı. Kümenizi yalnızca bir düğüm türü varsa, daha sonra durum bilgisi olan tüm uygulamalarınızı tüm yanıt emin olun [hizmet çoğaltması yaşam döngüsü olaylarını](service-fabric-reliable-services-lifecycle.md) vakitli ve hizmet çoğaltma yeniden (yapı içinde çoğaltma takılmış gibi) beş dakikadan kısa bir süre (Gümüş dayanıklılık düzeyi için) süresidir. 

    > [!WARNING]
    > VM SKU boyutu en az Gümüş dayanıklılık çalışmıyor önerilen sanal makine ölçek kümeleri için değiştirmeyi. VM SKU boyutunu değiştirerek verileri yıkıcı yerinde altyapı bir işlemdir. Gecikme veya bu değişikliği izlemek için en azından bazı kabiliyeti olmadan işlem durum bilgisi olan hizmetler için veri kaybına neden veya durum bilgisiz iş yükleri için bile diğer öngörülemeyen operasyonel sorunlara neden olabilir. 
    > 
    
- En az bir etkin Silver veya Gold dayanıklılık düzeyine sahip tüm sanal makine ölçek kümesi için beş düğüm sayısı korur.
- Her sanal makine ölçek Silver veya Gold dayanıklılık düzeyi ile Service Fabric kümesinde kendi düğüm türü eşlenmelidir. Eşleme birden fazla VM ölçek kümeleri için tek bir düğüm türü düzgün çalışmasını Service Fabric kümesi ve Azure altyapı arasında koordinasyon gereksinimini olabildiğince engeller.
- Rastgele VM örneklerini silmek değil, her zaman sanal makine ölçek kümesi ölçek özelliği aşağı kullanın. Rastgele VM örneklerinin silme işlemi, UD ve FD üzerinden yayılan VM örneğinde dengede değil oluşturma bir olasılığına sahiptir. Bu dengesizliği sistemleri düzgün bir şekilde Yük Dengeleme Hizmeti hizmeti örnekleri çoğaltmaları arasından olanağı olumsuz yönde etkileyebilir.
- (VM örneklerini kaldırarak) içinde ölçeklendirme yapılır yalnızca bir düğümün aynı anda otomatik ölçeklendirme kullanırken, ardından kuralları ayarlayın. Aynı anda birden fazla örneğini ölçeklendirme güvenli değildir.
- Silme veya birincil düğüm türünde VM serbest bırakılıyor, hiçbir zaman ne güvenilirlik katmanını gerektirir aşağıda ayrılmış VM sayısı azaltmanız gerekir. Bu işlemler, bir ölçek kümesindeki bir dayanıklılık düzeyi Silver veya Gold ile süresiz olarak engellenir.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Kümenin güvenilirlik özellikleri
Güvenilirlik katmanı çoğaltmaları birincil düğüm türünde bu kümede çalıştırmak istediğiniz sistem hizmetlerinin sayısını ayarlamak için kullanılır. Daha fazla sayıda yineleme, daha güvenilir, kümenizin sistem hizmetleri şunlardır.  

Güvenilirlik katmanı, şu değerleri alabilir:

* Platinum - sistem hizmetlerini çalıştıran bir hedef çoğaltma dokuz sayısını ayarlayın.
* Altın - sistem hizmetlerini çalıştıran bir hedef çoğaltma yedi sayısını ayarlayın.
* Silver - sistem hizmetlerini çalıştıran bir hedef çoğaltma beş sayısını ayarlayın. 
* Bronz - sistem hizmetlerini çalıştıran bir hedef çoğaltma üç ayarlayın.

> [!NOTE]
> Seçtiğiniz güvenilirlik katmanını birincil düğüm türünüz olmalıdır düğüm sayısı alt sınırı belirler. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Güvenilirlik katmanı için öneriler

Artırır veya azaltırsanız, küme (tüm düğüm türleri VM örnekleri toplamı), kümenizin bir katmandan diğerine güvenilirlik güncelleştirmeniz gerekir. Bunun yapılması, Sistem Hizmetleri çoğaltma değiştirmek için sayınızın Küme yükseltme tetikler. Kümeye düğüm ekleme gibi diğer herhangi bir değişiklik yapmadan önce tamamlamak için devam eden yükseltme için bekleyin.  Service Fabric Explorer veya çalıştırarak yükseltmesinin ilerleme durumunu izleyebilirsiniz [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

Güvenilirlik katmanı seçme öneri aşağıdadır.

| **Küme boyutu** | **Güvenilirlik katmanı** |
| --- | --- |
| 1 |Güvenilirlik katmanı parametreyi belirtmezseniz, sistem hesaplar |
| 3 |Bronz |
| 5 veya 6|Gümüş |
| 7 veya 8 |Altın |
| 9 ve üstü |Platinum |

## <a name="primary-node-type---capacity-guidance"></a>Birincil düğüm türü - Kapasite Kılavuzu

Birincil düğüm türü kapasite planlama Kılavuzu şu şekildedir:

- **Tüm üretim iş yüklerini Azure'da çalıştırmak için sanal makine örneği sayısını:** 5 en az bir birincil düğüm türü boyutunu belirtmeniz gerekir. 
- **Test iş yüklerini Azure'da çalıştırmak için sanal makine örneği sayısını** 1 veya 3 en düşük birincil düğüm türü boyutu belirtebilirsiniz. Bir düğüm kümesi çalıştıran özel bir yapılandırma ve bu nedenle, bu küme olarak ölçeklendirilmesini desteklenmiyor. Tek düğümlü bir küme, güvenilirlik ve bu nedenle Resource Manager şablonunuzu, sahip olduğunuz Kaldır/değil söz konusu yapılandırmayı belirtmek (yapılandırma değeri ayarı yok değil yeterli). Portal ayarlanan bir düğüm kümesi ayarlama, daha sonra yapılandırma otomatik olarak dikkate. Bir ile üç düğümlü kümeler, üretim iş yüklerini çalıştırmak için desteklenmez. 
- **Sanal makine SKU'su:** birincil düğüm türü olduğundan Sistem Hizmetleri çalıştırdığı için gereken genel yoğun dikkate al yüklemek, seçtiğiniz sanal makine SKU'su planı kümesine yerleştirmek. Ne burada bilmem göstermek-birincil düğüm türü, "Lungs", oxygen, beyin için sağlanan korumanın olduğunu düşündüğünüz bir benzerliği işte ve beyin yeterli oxygen almazsa, bu nedenle, gövde alternatife. 

Küme kapasitesi gereksinimlerini belirlenir olduğundan, kümedeki çalıştırmayı planladığınız iş yüküne göre ancak İşte yardımcı olacak geniş kapsamlı Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sunamıyoruz

Üretim iş yükleri için: 

- Kümelerinize ayrılması önerilir ikincil NodeType uygulamanızı dağıtmak için birincil NodeType sistem hizmetleri ve yerleştirme kısıtlamaları kullanın.
- Önerilen sanal makine SKU'su, standart D3 veya standart D3_V2 veya en az 14 GB'lık yerel SSD eşdeğerini değerdir.
- En düşük desteklenen sanal makine SKU'su standart D1 veya standart D1_V2 ya da en az 14 GB'lık yerel SSD eşdeğerini kullanılır. 
- 14 GB yerel SSD en düşük gereksinimdir. Bizim en az 50 GB önerilir. Özellikle Windows kapsayıcıları ne zaman çalışan, iş yükleriniz için daha büyük disklerin gereklidir. 
- Kısmi çekirdek gibi standart A0 VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU, performans nedenleriyle üretim iş yükleri için desteklenmiyor.

> [!WARNING]
> Şu anda birincil düğüm üzerinde çalışan bir küme VM SKU boyutu değiştirilmesi desteklenmiyor. Bu nedenle birincil düğüm türü VM SKU dikkatli bir şekilde, kapasite gelecekteki ihtiyaçlarınızı dikkate alarak seçin. Şu anda birincil düğüm türünüz için yeni bir VM SKU (küçük veya büyük) olan doğru kapasiteye sahip yeni bir küme oluşturmak için taşımak için desteklenen tek yolu dağıtmak ve (varsa) uygulama durumunu geri yüklenmesi, uygulamalarınızı gelen [ yedeklemeleri'en son hizmet](service-fabric-reliable-services-backup-restore.md) eski kümeden önlemlerin. Herhangi bir sistem hizmet durumunu geri yüklemek gerekmez, yeni kümenize Uygulama dağıtırken yeniden oluşturulur. Yalnızca olsaydı tüm bunu daha sonra durum bilgisiz uygulamaların kümenizde çalışan uygulamalarınızı yeni kümeye dağıtın, geri yüklemek için hiçbir şey vardır.
> 

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Olmayan birincil düğüm türü - durum bilgisi olan iş yükleri için kapasite Kılavuzu

Bu kılavuzu kullanarak Service fabric durum bilgisi olan iş yükleri için olan [güvenilir koleksiyonlar veya reliable Actors](service-fabric-choose-framework.md) birincil olmayan düğüm türü çalıştırmakta olduğunuz.

**Sanal makine örneği sayısını:** durum bilgisi olan üretim iş yükleri için en az ve hedef çoğaltma sayısının 5 ile çalıştırmanızı önerilir. Bu, kararlı durumda, her hata etki alanı ve yükseltme etki alanında (çoğaltma kümesinden) çoğaltmasıyla düştüğünden emin anlamına gelir. Birincil düğüm türü için tüm güvenilirlik katmanını kavramı, sistem hizmetleri için bu ayarı belirtmek için bir yoldur. Bu nedenle aynı göz önünde bulundurarak, durum bilgisi olan hizmetler için geçerlidir.

Bu nedenle, durum bilgisi olan iş yükleri içinde çalıştırıyorsanız, üretim iş yükleri için en az önerilen olmayan birincil düğüm türü boyutu 5 ise.

**Sanal makine SKU'su:** VM SKU için seçtiğiniz planladığınız her bir düğümüne yerleştirmek için yoğun yük dikkate gerekir bu düğüm türü, uygulama hizmetleri çalıştığı, olduğundan. Nodetype kapasite ihtiyaçlarını, ancak İşte yardımcı olacak geniş kapsamlı Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sunamıyoruz için kümedeki çalıştırmayı planladığınız iş yüküne göre belirlenir

Üretim iş yükleri için 

- Önerilen sanal makine SKU'su, standart D3 veya standart D3_V2 veya en az 14 GB'lık yerel SSD eşdeğerini değerdir.
- En düşük desteklenen sanal makine SKU'su standart D1 veya standart D1_V2 ya da en az 14 GB'lık yerel SSD eşdeğerini kullanılır. 
- Kısmi çekirdek gibi standart A0 VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU Performans nedeniyle üretim iş yükleri için özellikle olmayan desteklenmiyor.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Olmayan birincil düğüm türü - durum bilgisiz iş yükleri için kapasite Kılavuzu

Bu kılavuz, birincil olmayan nodetype üzerinde çalıştırdığınız durum bilgisiz iş yükleri.

**Sanal makine örneği sayısını:** durum bilgisiz olduğundan üretim iş yükleri için en düşük desteklenen olmayan birincil düğüm türü boyutu 2'dir. Bu, uygulamanızı ve hizmetinizi vererek bir sanal makine örneği kaybı varlığını sürdürmesi için iki durum bilgisiz örneklerini çalıştırmanıza olanak sağlar. 

**Sanal makine SKU'su:** VM SKU için seçtiğiniz planladığınız her bir düğümüne yerleştirmek için yoğun yük dikkate gerekir bu düğüm türü, uygulama hizmetleri çalıştığı, olduğundan. Düğüm türü, kapasite ihtiyaçlarını, ancak İşte yardımcı olacak geniş kapsamlı Kılavuzu, belirli iş yükü için nitel Kılavuzu ile çalışmaya başlama sunamıyoruz için kümedeki çalıştırmayı planladığınız iş yüküne göre belirlenir

Üretim iş yükleri için 

- Önerilen sanal makine SKU'su, standart D3 veya standart D3_V2 veya eşdeğer değerdir. 
- En düşük desteklenen sanal makine SKU'su standart D1 veya standart D1_V2 veya eşdeğer kullanılır. 
- Kısmi çekirdek gibi standart A0 VM SKU'ları üretim iş yükleri için desteklenmiyor.
- Standart A1 SKU, performans nedenleriyle üretim iş yükleri için desteklenmiyor.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
Son kapasite planlaması ve bir küme oluşturma sonra aşağıdaki okuyun:

* [Service Fabric küme güvenliği](service-fabric-cluster-security.md)
* [Service Fabric kümesini ölçekleme](service-fabric-cluster-scaling.md)
* [Olağanüstü Durum Kurtarma planlaması](service-fabric-disaster-recovery.md)
* [NodeType sanal makine ölçek ilişkisi](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
