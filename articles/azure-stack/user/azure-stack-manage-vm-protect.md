---
title: Azure yığında dağıtılan Vm'leri koruma | Microsoft Docs
description: Azure yığın üzerinde dağıtılan sanal makineleri koruma hakkında yönergeler.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/14/2018
ms.author: jeffgilb
ms.reviewer: hector.linares
ms.openlocfilehash: 734ee0e6ffb0dab660a2b63b431780208e0e0484
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34165273"
---
# <a name="protect-virtual-machines-deployed-on-azure-stack"></a>Azure yığın üzerinde dağıtılan sanal makineleri koruma

Bu makalede, kullanıcılarınızın Azure yığında dağıtmak sanal makineler (VM'ler) korumaya yönelik bir plan geliştirmek için bir kılavuz olarak kullanın.

Veri kaybı ve Planlanmamış kapalı kalma süresi karşı korumak için kullanıcı uygulamaları ve verileri için bir yedekleme kurtarma ve olağanüstü durum kurtarma planı uygulamak için gerekir. Bu plan her uygulama için benzersiz olabilir ancak kuruluşunuzun kapsamlı iş sürekliliği ve olağanüstü durum kurtarma (BC/DR) stratejisi tarafından oluşturulan bir çerçeve izler. İyi bir başlangıç noktasıdır [Azure için esnek uygulamalar tasarlama](https://docs.microsoft.com/azure/architecture/resiliency), sağlayan genel desenleri ve uygulamalar için uygulama kullanılabilirliğini ve esnekliğini.

>[!IMPORTANT]
> Yedekleme kurtarma ve olağanüstü durum kurtarma planlarınızı düzenli olarak test edin. Bu emin olmak için gerekir:
> * Planları çalışma
> * Planları, hala için tasarlanmış olan gereksinimlerini.

## <a name="azure-stack-infrastructure-recovery"></a>Azure yığın altyapı kurtarma

Kullanıcıların kendi sanal makineleri ayrı ayrı Azure yığın altyapı Hizmetleri'nden korumak sizin sorumluluğunuzdadır.

Kurtarma planı Azure yığın altyapı hizmetleri için **yok** kurtarma kullanıcı VM'ler, depolama hesapları veya veritabanlarını içerir. Uygulama sahibi olarak uygulamalar ve veriler için bir kurtarma planı uygulamak için sorumlu.

Azure yığın bulut uzun bir süre çevrimdışı olduğunda veya içinde planlama kurtarma olmasına gerek kalıcı olarak kurtarılamaz, koyun:

* En düşük kapalı kalma sağlar
* Veritabanı sunucuları gibi kritik VM'ler tutar
* Uygulamaları kullanıcı isteklerine hizmet tutmak için etkinleştirir

Azure yığın bulut operatörü, Azure yığın altyapının ve Hizmetleri için bir kurtarma planı oluşturmak için sorumludur. Daha fazla bilgi için makaleyi okuyun [geri dönülemez veri kaybına karşı kurtarmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-backup-recover-data).

## <a name="sourcetarget-combinations"></a>Kaynak/hedef birleşimler

Her Azure yığın bulut bir veri merkezine dağıtılır. Ayrı bir ortamda, uygulamalarınızı kurtarmak için gereklidir. Kurtarma Ortamı farklı bir veri merkezinde başka bir Azure yığın Bulut veya Azure genel bulutunda olabilir. Kurtarma Ortamı'uygulamanız için verileri gizlilik gereksinimlerine ve veri egemenliği belirler. Her uygulama için korumayı etkinleştirme gibi her biri için belirli bir kurtarma seçeneğini seçme esnekliğine sahiptir. Başka bir veri merkezine veri yedekleyen bir Abonelikteki uygulamalar olabilir. Başka bir abonelikte Azure genel bulut veri çoğaltabilirsiniz.

Her uygulama için hedef belirlemek her bir uygulama için yedekleme kurtarma ve olağanüstü durum kurtarma stratejinizi planlayın. Bir kurtarma planı düzgün depolama kapasitesi gerekli içi boyut ve genel bulut tüketimini proje kuruluşunuz yardımcı olur.

|  | Genel Azure | CSP veri merkezine dağıtılır ve CSP tarafından işletilen azure yığını | Müşteri veri merkezine dağıtılır ve müşteri tarafından işletilen azure yığını |
|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **CSP veri merkezine dağıtılır ve CSP tarafından işletilen azure yığını** | Kullanıcı Vm'lerinin işletilen CSP Azure yığını dağıtılır. Kullanıcı Vm'lerinin yedekten geri yüklenmiş veya doğrudan Azure devredilir. | CSP Azure yığın birincil ve ikincil örnekleri kendi veri merkezlerinde çalışır. Kullanıcı Vm'lerinin geri veya iki Azure yığın örnekleri arasında devredilir. | CSP Azure yığın birincil sitede çalışır. Müşterinin veri merkezi geri yükleme veya yük devretme hedefidir. |
| **Müşteri veri merkezine dağıtılır ve müşteri tarafından işletilen azure yığını** | Kullanıcı VM'ler için dağıtılan Azure yığın müşterisi çalıştırılır. Kullanıcı Vm'lerinin yedekten geri yüklenmiş veya doğrudan Azure devredilir. | Müşteri Azure yığın birincil ve ikincil örnekleri kendi veri merkezlerinde çalışır. Kullanıcı Vm'lerinin geri veya iki Azure yığın örnekleri arasında devredilir. | Müşteri Azure yığın birincil sitede çalışır. CSP'ın datacenter geri yükleme veya yük devretme hedefidir. |

![Kaynak hedef birleşimler](media\azure-stack-manage-vm-backup\vm_backupdataflow_01.png)

## <a name="application-recovery-objectives"></a>Uygulama Kurtarma hedefleri

Kuruluşunuz her uygulama için dayanabileceği kapalı kalma süresi ve veri kaybı miktarı belirlemeniz gerekir. Kapalı kalma süresi ve veri kaybı niceleme tarafından kuruluşunuzdaki bir olağanüstü durum etkisini en aza indiren bir kurtarma planı oluşturabilirsiniz. Her uygulama için göz önünde bulundurun:

 - **Kurtarma süresi hedefi (RTO)**  
RTO uygulama sonra bir olay kullanılamıyor olabilir maksimum kabul edilebilir zamanı gelmiştir. Örneğin, bir RTO 90 dakika, uygulamasının çalışır duruma 90 dakika içinde başından bir olağanüstü durum geri yükleme mümkün olması gerektiği anlamına gelir. Düşük RTO varsa, bölgesel bir kesintinin karşı korumak için bekleme sürekli çalıştıran ikinci bir dağıtım tutabilirsiniz.
 - **Kurtarma noktası hedefi (RPO)**  
RPO, olağanüstü durum sırasında kabul edilebilir veri kaybı en uzun süresi ' dir. Örneğin, verileri başka bir veritabanına çoğaltma yapmadan tek bir veritabanında depoluyor ve saatlik yedeklemeler gerçekleştiriyorsanız bir saate kadar varan bir süreye ait verileri kaybedebilirsiniz.

RTO ve RPO iş gereksinimleridir. Uygulamanın RTO ve RPO tanımlamak için bir risk değerlendirmesi yürütün.

Başka bir ölçümüdür **saati kurtarmak** (MTTR) olan bir hatadan sonra uygulama geri yüklemek için geçen ortalama süre. MTTR, bir sistem için ampirik bir değerdir. MTTR, RTO’yu aşıyorsa bir hata halinde sistemin tanımlanmış RTO içerisinde geri yüklenmesi mümkün olmayacağından kabul edilemez iş kesintileri ortaya çıkar.

### <a name="backup-restore"></a>Yedekleme geri yükleme

VM tabanlı uygulamalar için en yaygın koruma şeması yedekleme yazılımını kullanmaktır. Genellikle bir VM'yi yedekleme konusunda, işletim sistemi, işletim sistemi yapılandırması, uygulama ikili dosyaları ve uygulama verilerini içerir. Yedeklemeleri bir birimleri, disk veya tüm VM anlık görüntüsü tarafından oluşturulur. Azure yığını ile gelen konuk işletim sistemini veya Azure yığın depolama biriminden bağlamında yedekleme davranabilirsiniz ve API işlem. Azure yığın hiper yönetici düzeyinde alma yedeklemeleri desteklemez.
 
![Yedekleme geri](media\azure-stack-manage-vm-backup\vm_backupdataflow_03.png)

Uygulama Kurtarma aynı buluta veya yeni bir bulut bir veya daha fazla sanal makineleri geri yükleme gerektirir. Bir bulut veri merkeziniz veya ortak bulut hedefleyebilirsiniz. Seçtiğiniz bulut tamamen denetiminizi içinde ve veri gizliliği ve Egemenlik gereksinimlerinize göre dayanır.
 
 - RTO: Kapalı kalma süresi saat cinsinden ölçülür.
 - RPO: Değişken veri kaybı (bağlı olarak yedekleme sıklığı)
 - Dağıtım topolojisi: Etkin/pasif

#### <a name="planning-your-backup-strategy"></a>Yedekleme stratejinizi planlama

Yedekleme stratejinizi planlama ve ölçeklendirme gereksinimlerine tanımlama korunması gerekmediğini VM örneği sayısını niceleme ile başlar. Bir ortamda tüm sunucular genelinde tüm Vm'leri yedekleme ortak bir stratejidir. Ancak, Azure yığınla yedeklenmesi gereken bazı VM'ler vardır. Örneğin, bir ölçek kümesindeki sanal makineleri dönün ve bazen bildirilmeksizin Git kısa ömürlü kaynakları olarak kabul edilir. Korunması gereken herhangi bir dayanıklı veri bir veritabanı veya nesne deposu gibi ayrı bir havuzda depolanır.

Azure yığında Vm'leri yedekleme ilgili önemli noktalar:

 - **Kategorilere ayırma**
    - Burada kullanıcılar için bir VM yedeğinin kabul model göz önünde bulundurun.
    - Uygulamalar veya iş üzerindeki etkileri önceliğini göre bir kurtarma hizmet düzeyi sözleşmesi (SLA) tanımlayın.
 - **Ölçeklendirme**
    - Düzenlenmiş yedeklemeleri çok sayıda yeni VM'ler (Yedekleme gerekliyse) devreye alma sırasında göz önünde bulundurun.
    - Verimli bir şekilde yakalayabilir ve kaynak içeriği çözüm üzerinde en aza indirmek için yedek veri aktaran yedekleme ürünleri değerlendirin.
    - Verimli bir şekilde ortamdaki tüm sanal makineler arasında tam yedeklemeler için gereken en aza indirmek için artımlı veya değişiklik yedekleri kullanarak yedek verileri depolamak yedekleme ürünleri değerlendirin.
 - **Geri yükleme**
    - Yedekleme ürünleri sanal diskler, mevcut bir VM'yi veya içindeki tüm VM kaynak uygulama verilerini ve ilişkili sanal diskler geri yükleyebilirsiniz. Gereksinim duyduğunuz geri yükleme düzeni nasıl, uygulamayı geri planınıza bağlıdır ve kurtarma için uygulama zamanı etkiler. Örneğin, SQL Server'dan şablonu yeniden dağıt ve tüm VM veya VM'ler kümesini geri yerine veritabanlarını geri yüklemek daha kolay olabilir.

### <a name="replicationmanual-failover"></a>Çoğaltma/elle yük devretme

Yüksek kullanılabilirliği destekleyen bir alternatif için başka bir bulut uygulama Vm'leriniz çoğaltılması ve el ile yük devretme sırasında kullanan yaklaşımdır. İşletim sistemi, uygulama ikili dosyaları ve uygulama verilerinin çoğaltılmasını VM düzeyinde veya konuk işletim sistemi düzeyinde gerçekleştirilebilir. Yük devretme, uygulamanın parçası olmayan ek yazılım kullanılarak yönetilir.

Bu yaklaşım ile bir buluta dağıtılan uygulama ve kendi VM bir bulut için çoğaltılır. Bir yük devretme tetikleniyorsa, ikincil sanal makineleri ikinci bulutta açık gerekir. Bazı senaryolarda, yük devretme bunları VM'ler ve ekler disklere oluşturur. Bu işlem belirli başlatma sırası gerektiren özellikle bir çok katmanlı uygulaması ile tamamlanması uzun zaman alabilir. Uygulama isteklerine hizmet başlatmaya hazır önce çalıştırılması gereken adımları da olabilir.

![Çoğaltmayı elle yük devretme](media\azure-stack-manage-vm-backup\vm_backupdataflow_02.png)

 - RTO: Kapalı kalma süresi dakika cinsinden ölçülür.
 - RPO: Değişken veri kaybı (bağlı olarak çoğaltma sıklığı)
 - Dağıtım topolojisi: Etkin/pasif beklemeyi
 
### <a name="high-availabilityautomatic-failover"></a>Yüksek kullanılabilirlik/otomatik yük devretme

İşinizi birkaç saniye veya kapalı kalma süresi ve en düşük düzeyde veri kaybı dakikası burada dayanabileceği uygulamalar için yüksek kullanılabilirlik yapılandırmasıyla göz önünde bulundurmanız gerekir. Yüksek kullanılabilirlik uygulamaları hızla ve otomatik olarak hataları kurtarmak için tasarlanmıştır. Yerel donanım hataları için Azure yığın altyapı raf üstü anahtar iki üst kullanarak fiziksel ağda yüksek kullanılabilirlik uygular. İşlem düzeyi hataları Azure yığın bir ölçek birimi birden çok düğüm kullanır. VM düzeyinde düğüm hatalarını uygulamanızı almayan emin olmak için hata etki alanları ile birlikte ölçek kümeleri kullanabilirsiniz.

Ölçek kümeleri ile birlikte, yüksek kullanılabilirlik yerel olarak destekleyen veya kümeleme yazılımı kullanımını desteklemek uygulamanız gerekir. Örneğin, Microsoft SQL Server yüksek kullanılabilirlik için zaman uyumlu tamamlama modunu kullanarak veritabanlarını yerel olarak destekler. Ancak, yalnızca zaman uyumsuz çoğaltma destekleyebiliyorsa, ardından olacaktır bazı veri kaybı. Uygulamaları, uygulamanın otomatik yük devretme kümeleme yazılımı nerede işleme bir yük devretme kümesine de dağıtılabilir.

Bu yaklaşımı kullanarak, uygulama yalnızca bir buluta etkindir, ancak yazılım için birden çok bulut dağıtılır. Diğer Bulutları beklemeyi modda yük devretme tetiklendiğinde uygulamayı başlatmak hazır olur.

 - RTO: Kapalı kalma süresini saniye cinsinden ölçülür.
 - RPO: En düşük düzeyde veri kaybı
 - Dağıtım topolojisi: etkin/etkin bekleme

### <a name="fault-tolerance"></a>Hataya dayanıklılık

Azure yığın fiziksel artıklık ve altyapı hizmeti kullanılabilirlik yalnızca bir donanıma karşı korumaya, bu tür bir disk, güç, ağ bağlantı noktası veya düğüm hataları/hataları düzeyi. Ancak, uygulamanızın her zaman kullanılabilir olması gerekir ve hiçbir zaman herhangi bir veri kaybedebilir, hata toleransı, uygulamanızda yerel olarak uygulamak veya hataya dayanıklılık sağlamak için ek yazılım kullanmak gerekir.

İlk olarak, düğüm düzeyi arızalarına karşı koruma sağlamak için sanal makineleri ölçek kullanılarak dağıtılan uygulama ayarlar emin olmak gerekir. Kesinti olmadan isteklere hizmet devam edebilmek için çevrimdışı duruma geçiyor bulut karşı korumak için aynı uygulama zaten farklı bir buluta dağıtılması gerekir. Bu model, genellikle bir etkin-etkin dağıtım denir.

Her Azure yığın bulut Bulutlar her zaman bir altyapı açısından etkin olarak kabul edilir şekilde birbirinden bağımsız olduğunu aklınızda bulundurun. Bu durumda, uygulamanın birden çok etkin örnekler için bir veya daha fazla etkin bulut dağıtılır.

 - RTO: Kapalı kalma süresi
 - RPO: Hiçbir veri kaybı
 - Dağıtım topolojisi: etkin/etkin

### <a name="no-recovery"></a>Kurtarma yok

Bazı uygulamalar, ortamınızdaki Planlanmamış kapalı kalma süresi veya veri kaybına karşı koruma gerekmeyebilir. Örneğin, VM'ler geliştirme için kullanılır ve test genellikle gerekmez kurtarılır. Bu bir uygulama veya belirli bir VM'yi koruma olmadan yapmak için bir karardır. Azure yığın yedekleme veya çoğaltma VM'lerin temel altyapısından sağlamaz. Benzer şekilde Azure, her aboneliklerinizden biri her bir VM için koruma katılımı yapmanız gerekir.

 - RTO: kurtarılamaz
 - RPO: Tam veri kaybının

## <a name="recommended-topologies"></a>Önerilen topolojiler

Azure yığın dağıtımınız için önemli noktalar:

|     | Öneri | Yorumlar |
|-------------------------------------------------------------------------------------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme/VMs, veri merkezinizde zaten dağıtılmış bir dış yedekleme hedefine geri yükleme | Önerilen | Var olan yedekleme altyapısı ve işletimsel becerileri yararlanın. Ek VM örnekleri korumak hazır olması için Yedekleme Altyapısı boyut emin olun. Yedekleme Altyapısı kaynağınız yakın olmadığından emin olun. ' % S'kaynağına Azure yığını, ikincil bir Azure yığın örneği ya da Azure sanal makineleri geri yükleyebilirsiniz. |
| Yedekleme/VMs Azure yığını tarafından ayrılmış bir dış yedekleme hedefine geri yükleme | Önerilen | Azure yığını için yeni yedekleme altyapısı veya sağlama özel yedekleme altyapı satın alabilirsiniz. Yedekleme Altyapısı kaynağınız yakın olmadığından emin olun. ' % S'kaynağına Azure yığını, ikincil bir Azure yığın örneği ya da Azure sanal makineleri geri yükleyebilirsiniz. |
| Genel Azure veya bir güvenilen hizmet sağlayıcısı için doğrudan sanal makineleri yedekleme/geri yükleme | Önerilen | Veri gizliliği ve Mevzuat gereklilikleri karşılamak sürece genel Azure veya bir güvenilen hizmet sağlayıcısı Yedeklemelerinizin depolayabilirsiniz. İdeal olarak, geri yüklediğinizde işletimsel deneyimi tutarlılık sürdürmenizi hizmet sağlayıcısı ayrıca Azure yığın çalışıyor. |
| Replicate/yük devretme VM'ler için ayrı bir Azure yığın örneği | Önerilen | Yük devretme durumda genişletilmiş uygulama kapalı kalma süresi kaçınmak için ikinci Azure yığın Bulutu tam olarak işlevsel olması gerekir. |
| Replicate/yük devretme Vm'lere doğrudan Azure veya bir güvenilen hizmet sağlayıcısı | Önerilen | Veri gizliliği ve Mevzuat gereklilikleri karşılamak sürece genel Azure veya bir güvenilen hizmet sağlayıcısı için verilerinizi çoğaltabilirsiniz. İdeal olarak yük devretme sonrasında işletimsel deneyimi tutarlılık almak için hizmet sağlayıcısı ayrıca Azure yığın çalışıyor. |
| Yedekleme hedefi olan uygulama verilerinizi aynı Azure yığın buluta dağıtma | Önerilmez | Aynı Azure yığın buluttaki yedeklemelerini depolamak kaçının. Planlanmamış kapalı kalma süresi bulutun, birincil veri ve yedekleme verileri tutabilirsiniz. Yedekleme hedefi (yedekleme ve geri yükleme için en iyi duruma getirme amacıyla) bir sanal gereç olarak dağıtmak seçerseniz, tüm verileri sürekli olarak bir dış yedekleme konumuna kopyalanır emin olmalısınız. |
| Fiziksel yedekleme uygulaması Azure yığın çözüm yüklendiği aynı raf ile dağıtma | Desteklenmiyor | Bu anda, özgün çözümde parçası olmayan raf anahtarları dön diğer cihazları bağlanamıyor. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure yığında dağıtılan kullanıcı Vm'leri korumak için genel yönergeler sağlanır. Kullanıcı Vm'lerinin korumak için Azure Hizmetleri kullanma hakkında daha fazla bilgi için bkz:

 - [Dosyalar ve uygulamalar Azure yığın üzerinde yedekleme için Azure Yedekleme'yi kullanın](https://docs.microsoft.com/azure/backup/backup-mabs-files-applications-azure-stack)
 - [Azure yığın Azure yedekleme sunucusu desteği](https://docs.microsoft.com/azure/backup/ ) 
 - [Azure yığın Azure Site Recovery desteği](https://docs.microsoft.com/azure/site-recovery/)  

Azure yığında VM koruma sağlar iş ortağı ürün hakkında daha fazla bilgi için bkz "[uygulamaları ve verileri Azure yığında koruma](https://azure.microsoft.com/blog/protecting-applications-and-data-on-azure-stack/)."
