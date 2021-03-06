---
title: Azure'da VM yedekleme altyapınızı planlama
description: Azure sanal makinelerini yedeklemek planlama yaparken önemli noktalar
services: backup
author: markgalioto
manager: carmonm
keywords: vm'leri yedekleme, sanal makineleri yedekleme
ms.service: backup
ms.topic: conceptual
ms.date: 7/26/2018
ms.author: markgal
ms.openlocfilehash: b6288cd51cbbe36297235a65fb55c0d9c92101b6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283715"
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Azure’da sanal makine yedekleme altyapınızı planlama
Bu makalede, performans ve sanal makine yedekleme altyapınızı planlama yapmanıza yardımcı olması için kaynak önerileri sağlar. Ayrıca, yedekleme hizmeti önemli yönlerini tanımlar; Bu görünüşler Mimarinizi, belirlemede önemli kapasite planlaması ve zamanlama. Belirttiyseniz [ortamınızı hazırladığınız](backup-azure-arm-vms-prepare.md), planlama, sonraki adıma başlamadan önce [Vm'lerini yedekleme](backup-azure-arm-vms.md). Azure sanal makineleri hakkında daha fazla bilgiye ihtiyacınız varsa bkz [sanal makineler belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Azure nasıl yaptığını sanal makineleri yedekleyin?
Ne zaman Azure Backup hizmeti yedekleme uzantısını zaman içinde nokta anlık görüntüsünü almak için hizmet Tetikleyiciler zamanlanan saatte bir yedekleme işi başlatır. Azure Backup hizmeti kullandığı _VMSnapshot_ Windows, uzantı ve _VMSnapshotLinux_ Linux'ta uzantısı. Uzantının ilk VM yedeklemesi sırasında yüklenir. Uzantıyı yüklemek için VM çalıştırılması gerekir. VM çalışmıyorsa Backup hizmeti, temel alınan depolamanın anlık görüntüsünü alır (VM durduğunda herhangi bir uygulama yazma işlemi gerçekleşmediği için).

Windows VM'lerinin anlık görüntüsü alınırken, Backup hizmeti Birim Gölge Kopyası Hizmeti (VSS) ile eşgüdümlü çalışarak sanal makine disklerinin tutarlı bir anlık görüntüsünü elde eder. Linux sanal makinelerini yedekleme yapıyorsanız, sanal makine anlık görüntüsünü alırken tutarlılığını sağlamak için kendi özel komut dosyaları yazabilirsiniz. Bu betikleri çağırma hakkında ayrıntılar bu makalenin sonraki bölümlerinde verilmiştir.

Azure Backup hizmeti anlık görüntüyü aldıktan sonra veriler kasaya aktarılır. Verimliliği maksimuma çıkarmak için hizmet yalnızca bir önceki yedeklemeden itibaren değişmiş olan veri bloklarının aktarımını yapar.

![Azure sanal makine yedekleme mimarisi](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

> [!NOTE]
> 1. Azure Backup yedekleme işlemi sırasında geçici disk sanal makineye bağlı içermez. Daha fazla bilgi için blog bakın [geçici depolama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Depolama düzeyinde anlık görüntü ve kasa için bu anlık görüntüsü aktarır azure yedekleme alır değiştirmeyin depolama hesabı anahtarlarını yedekleme işi tamamlanana kadar.
> 3. Premium VM'ler için Azure Backup, anlık görüntü depolama hesabına kopyalar. Yedekleme hizmeti yeterli IOPS veri aktarmak için kasaya kullandığından emin olmak için budur. Bu ek kopyasını bir depolama boyutu ayrılan VM göre ücretlendirilir. 
>

### <a name="data-consistency"></a>Veri tutarlılığı
Yedekleme ve geri yükleme iş açısından kritik verilerin, verileri oluşturan uygulamalar yedeklenmelidir olgu ile kritik verileri karmaşık iş çalışıyor. Bunu ele almak için Azure Backup uygulamayla tutarlı yedeklemeler yapılmasını hem Windows hem de Linux Vm'leri destekler
#### <a name="windows-vm"></a>Windows VM
Azure yedekleme, Windows Vm'lerinde VSS tam yedeklemeler gerçekleştirir (daha fazla bilgi edinin [tam yedekleme VSS](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). VSS kopyalama yedeklemeleri etkinleştirmek için aşağıdaki kayıt defteri anahtarını VM'de ayarlanması gerekir.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Linux VM'leri
Azure Backup, bir komut dosyası altyapısı sağlar. Uygulama tutarlılığı Linux Vm'lerini yedeklerken emin olmak için özel öncesi ve yedekleme iş akışı ve ortam denetimi sonrası komut dosyalarını oluşturun. Azure yedekleme, VM anlık görüntüsü almadan önce ön betik çağırır ve VM anlık görüntü işi tamamlandıktan sonra sonrası betiği çağırır. Daha fazla ayrıntı için [ön betik ve son betik kullanarak uygulama ile tutarlı VM yedekleri](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Azure Backup yalnızca çağırır müşteri tarafından yazılan öncesi ve sonrası betikler. Betik öncesi ve sonrası Komut başarıyla çalışırsa, Azure Backup kurtarma noktası uygulama tutarlı işaretler. Ancak, müşterinin özel betikler kullanarak uygulama tutarlılığı için sonuçta sorumludur.
>


Bu tablo, tutarlılık türlerini açıklar ve Azure VM sırasında altında ortaya koşulları yedekleme ve geri yükleme yordamlarını.

| Tutarlılık | VSS tabanlı | Açıklama ve ayrıntıları |
| --- | --- | --- |
| Uygulama tutarlılığı |Windows için Evet|Uygulama tutarlılığı, sağlar gibi iş yükleri için idealdir:<ol><li> VM *önyükleme*. <li>Var. *bozulma*. <li>Var. *veri kaybı olmadan*.<li> Veri, uygulamanın yedekleme VSS veya ön/son betik kullanarak--ilgili olarak veri kullanan uygulama tutarlıdır.</ol> <li>*Windows Vm'leri*-en Microsoft iş yükleri, veri tutarlılığı için ilgili iş yüküne özgü eylemlerini gerçekleştirmek VSS yazıcılarının sahiptir. Örneğin, Microsoft SQL Server işlem günlüğü dosyasını ve veritabanına yazma doğru şekilde yapıldığını sağlayan bir VSS yazıcısı olduğu. Azure Windows VM yedeklemeleri için bir uygulama ile tutarlı kurtarma noktası oluşturmak için yedekleme uzantısı gerekir VSS iş akışı çağırın ve VM anlık görüntüsü almadan önce tamamlayın. Azure VM anlık doğru olması tüm Azure VM uygulamaların VSS yazıcılarının de tamamlamanız gerekir. (Bilgi [VSS Temelleri](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) ve Ayrıntılar içinde ayrıntılı olarak inceleyin [nasıl çalıştığını](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Linux Vm'leri*-müşteriler yürütebilir [özel betik öncesi ve sonrası betik uygulama tutarlılığı sağlamak için](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Dosya sistemi tutarlılığı |Evet - Windows tabanlı bilgisayarlar için |Kurtarma noktası olduğu iki senaryo vardır *dosya sistemi ile tutarlı*:<ul><li>Azure'da Linux VM yedekleri öncesi-script/sonrası-script veya öncesi-script/sonrası-script başarısız olursa olmadan. <li>Azure'da Windows VM'ler için yedekleme sırasında VSS hatası.</li></ul> Bu her iki durumda yapılabilir en iyi şekilde emin olmak için verilmiştir: <ol><li> VM *önyükleme*. <li>Var. *bozulma*.<li>Var. *veri kaybı olmadan*.</ol> Geri yüklenen veriler kendi "yukarı düzeltme" mekanizması uygulamak uygulamaları gerekir. |
| Kilitlenme tutarlılığı |Hayır |Bu durum, bir sanal makine (ya da bir yazılım veya donanım sıfırlama) aracılığıyla "kilitlenme" yaşayan eşdeğerdir. Kilitlenme tutarlılığı, genellikle Azure sanal makine yedekleme sırasında kapatıldığında oluşur. Kilitlenmeyle tutarlı kurtarma noktası çevresinde veri tutarlılığını garanti depolama ortamına--işletim sistemi veya uygulamanın perspektifinden ya da sağlar. Zaten diskte yedekleme sırasında var olan veriler yakalanır ve yedeklendi. <br/> <br/> Varken garanti, genellikle, işletim sistemini başlatır ve ardından disk kontrol ederek yordamı, bozulma hataları düzeltmek için chkdsk gibi. Herhangi bir bellek içi verileri veya diske aktarılmamış yazma kaybolur. Verileri geri alma yapılmalıdır durumunda uygulama genellikle kendi doğrulama mekanizmasına izler. <br><br>İşlem günlüğü girişleri veritabanında mevcut değil varsa veri geri tutarlı olana kadar örnek olarak, veritabanı yazılımına yapar. Veri (Dağıtılmış birimler gibi) birden çok sanal diskler arasında yayılır, kilitlenmeyle tutarlı kurtarma noktası için veri doğruluğunu garanti sağlar. |

## <a name="performance-and-resource-utilization"></a>Performans ve kaynak kullanımı
Dağıtılan şirket içi yedekleme yazılımı gibi kapasite ve kaynak kullanımı için ihtiyaçlarını Azure Vm'lerini yedekleme zaman için planlamalısınız. [Azure depolama sınırlarını](../azure-subscription-service-limits.md#storage-limits) çalışan iş yükleri için en az etkisi olan en yüksek performansı elde etmek için VM dağıtımları nasıl tanımlayın.

Aşağıdaki Azure depolama sınırları yedekleme performansını planlarken dikkat edin:

* Depolama hesabı başına en fazla çıkışı
* Depolama hesabı başına toplam istek oranı

### <a name="storage-account-limits"></a>Depolama hesabı sınırları
Yedekleme verilerini bir depolama hesabından kopyalanır, giriş/çıkış işlemi (IOPS) ve çıkış (veya aktarım hızı) depolama hesabı ölçümleri ekler. Aynı zamanda, sanal makineler de IOPS ve aktarım hızı kullanıyor. Yedekleme ve sanal makine trafiği depolama hesabı sınırlarınızı aşmamak emin olmaktır.

### <a name="number-of-disks"></a>Disk sayısı
Yedekleme işlemi, bir yedekleme işinin tamamlanması mümkün olan en kısa sürede çalışır. Bunun yapılması mümkün olduğunca fazla kaynak tüketir. Ancak, tüm g/ç işlemleri tarafından sınırlandırılmıştır *hedef performans düzeyleri tek Blob*, saniyede 60 MB'lık bir sınır vardır. Girişimiyle hızını en üst düzeye çıkarmak için yedekleme işlemi her sanal makinenin diskleri yeniden dener *paralel*. Bir VM dört disk varsa, tüm dört disklerini paralel yedekleme hizmeti çalışır. **Diskleri sayısı** , desteklenen depolama hesabı yedekleme trafiği belirlemede en önemli faktör olan.

### <a name="backup-schedule"></a>Yedekleme zamanlaması
Performansı etkileyecek olan ek bir etmen **yedekleme zamanlaması**. Tüm VM'lerin aynı anda yedeklenir şekilde ilkeler yapılandırırsanız, trafiği Başınızı zamanladınız. Yedekleme işlemi, tüm disklerde paralel yedekleme dener. Bir depolama hesabından yedekleme trafiğini azaltmak için farklı zaman örtüşme ile günün farklı sanal makinelerini yedekleme.

## <a name="capacity-planning"></a>Kapasite planlaması
Önceki Etkenler bir araya getirildiğinde, depolama hesabı kullanım gereksinimlerini planlamanız gerekir. İndirme [VM yedek kapasite Excel elektronik tablosuna](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) disk ve yedekleme zamanlaması seçenekleri etkisini görmek için.

### <a name="backup-throughput"></a>Yedekleme aktarım hızı
Yedeklenmekte olan her bir disk için Azure Backup disk üzerindeki blokları okur ve yalnızca değiştirilen verileri (artımlı yedeklemeyi) depolar. Aşağıdaki tabloda, ortalama yedekleme hizmeti aktarım hızı değerleri gösterir. Aşağıdaki veriler kullanılarak, belirli bir boyutun bir diski yedeklemek için gereken süre miktarını tahmin edebilirsiniz.

| Yedekleme işlemi | En iyi olasılık aktarım hızı |
| --- | --- |
| İlk yedekleme |160 MB/sn |
| Artımlı yedekleme (DR) |640 Mbps <br><br> Aktarım hızını ciddi ölçüde düştüğü, (yedeklenmesi gereken) değiştirilen verileri disk arasında paylaştırılır.|

## <a name="total-vm-backup-time"></a>Toplam VM yedekleme zamanı
Okuma ve veri kopyalama, çoğunlukla yedekleme harcanır ancak diğer işlemler bir sanal Makineyi yedeklemek için gereken toplam süreyi katkıda bulunun:

* İçin gereken süre [yükleme veya güncelleştirme yedekleme uzantısı](backup-azure-arm-vms.md).
* Anlık görüntü süresi, bir anlık görüntü tetiklemek için geçen süredir. Anlık görüntüleri zamanlanmış yedekleme zamanını yakın tetiklenir.
* . Sıra bekleme süresi. Yedekleme hizmeti, birden çok müşteriyi yedeklerden işliyor olduğundan, yedekleme veya kurtarma Hizmetleri kasası için yedekleme verileri anlık görüntüden kopyalama hemen başlayabilir. Yoğun kez yüklenemiyor, işlenmekte olan yedeklerini nedeniyle sekiz saate kadar beklemeyi uzatabilirsiniz. Ancak, toplam VM yedekleme zamanını 24 saatten daha kısa bir süre için günlük yedekleme ilkelerini olur. <br>
**Bu, yalnızca artımlı yedeklemeler için ve ilk yedekleme için geçerli bir tutar. İlk yedekleme süresi orantılıdır ve verilerin boyutuna bağlı olarak 24 saatten uzun olabilir ve zaman yedeği alınır.**
* Veri aktarım süresini, depolama kasası için yedekleme hizmeti işlem önceki yedeklemeden artımlı değişiklikler ve bu değişiklikleri aktarmak gereken zamanı.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Neden miyim gözleme longer(>12 hours) yedekleme zamanı?
Yedekleme iki aşamadan oluşur: anlık görüntü alma ve anlık görüntüleri kasasına aktarma. Backup hizmeti, depolama için iyileştirir. Anlık görüntü verileri bir kasaya aktarırken hizmeti, yalnızca artımlı değişiklikler önceki anlık görüntüden aktarır.  Artımlı değişiklikleri belirlemek için hizmet bloğu sağlama toplamını hesaplar. Bir blok değiştirilirse, blok kasaya gönderilecek bir blok olarak tanımlanır. Ardından hizmet tatbikatları her veri aktarmak için en aza indirmek fırsatlar arar tanımlanan blokları, daha fazla. Hizmet, tüm değiştirilen blokları değerlendirme sonra değişiklikleri birleştirir ve bunları kasaya gönderir. Bazı eski uygulamalarda parçalanmış, küçük yazma işlemlerini depolama için uygun değildir. Anlık görüntü birçok küçük, parçalanmış yazma içeriyorsa, hizmet uygulamaları tarafından yazılan verilerin işlenmesi ek zaman harcadığı. Önerilen uygulama yazma, Azure sanal makine içinde çalışan uygulamalar için en az 8 KB'lık bloğudur. Uygulamanız değerinden 8 KB'lik bir blok kullanıyorsa, yedekleme performansı parametreden etkilenir. Uygulamanızın performansını artırmak için ayarlama hakkında bilgi için bkz: [uygulamalarını Azure depolama ile en iyi performans için ayarlama](../virtual-machines/windows/premium-storage-performance.md). Makaleyi yedekleme performansı Premium depolama örnekler kullansa standart depolama diskleri için geçerli bir kılavuzdur.

## <a name="total-restore-time"></a>Toplam geri yükleme süresi
Geri yükleme işlemi iki ana alt görevden oluşur: Seçilen müşteri depolama hesabına geri kasadan veri kopyalama ve sanal makine oluşturuluyor. Verileri kasadan kopyalama, Azure'da yedeklemelerini dahili olarak depolandığı ve müşteri depolama hesabı depolandığı bağlıdır. Verileri kopyalamak için harcanan süre bağlıdır:
* Hizmet işlemleri işleri aynı anda birden çok müşterilerden geri. sıra bekleme süresi - geri yükleme istekleri bir kuyruğa koymak olduğundan.
* Veri kopyalama saati - verileri kasadan müşteri depolama hesabına kopyalanır. Geri yükleme süresi bağlıdır üzerinde IOPS ve aktarım hızı, Azure Backup hizmeti seçili müşteri depolama hesabına alır. Geri yükleme işlemi sırasında kopyalama süresini azaltmak için diğer uygulama yazar ve okur ile yüklenmemiş bir depolama hesabı seçin.

## <a name="best-practices"></a>En iyi uygulamalar
Sanal makineleri için yedeklemeleri yapılandırılırken bu yöntemler aşağıdaki öneririz:

* Aynı anda yedekleme için aynı bulut hizmetindeki 10'dan fazla Klasik Vm'si zamanlama yok. Aynı bulut hizmetinden birden çok sanal makinelerini yedeklemek istiyorsanız, bir saatlik olarak yedekleme başlangıç zamanlarını basamaklandırmak.
* Aynı anda tek bir kasadan yedeklemek için 100'den fazla VM zamanlamayın. 
* VM yedeklemeleri yoğun olmayan saatlerde zamanlayın. Bu şekilde yedekleme hizmeti Kasası'na müşteri depolama hesabından veri aktarmak için IOPS kullanır.
* Bir ilke farklı depolama hesaplara yaymak vm'lerde uygulanır emin olun. En fazla 20 öneririz toplam disklerden tek bir depolama hesabında, aynı yedekleme zamanlaması tarafından korunabilir. 20 diskiniz daha büyük bir depolama hesabında varsa, bu sanal makineler için yedekleme işlemi aktarımı aşaması sırasında gerekli IOPS almak için birden çok ilke paylaştırın.
* Aynı depolama hesabı için Premium depolama alanında çalışan bir VM geri yüklemeyin. Geri yükleme işlemi işlemi, yedekleme işlemi ile örtüşür, yedekleme için kullanılabilir IOPS azaltır.
* VM yedekleme yığını V1 üzerinde Premium VM yedeklemesi için Azure Backup hizmeti anlık görüntü depolama hesabı ve aktarım veriler için kasa için depolama hesabındaki kopyalanan bu konumdan böylelikle toplam depolama hesabı yalnızca %50 ayırmak önerilir.
* Yedekleme 2.7 için emin olun, python sürümü Linux vm'lerinde etkin

## <a name="data-encryption"></a>Veri şifrelemesi
Azure Backup, Yedekleme işleminin bir parçası olarak verilerini şifrelemez. Ancak, sanal Makinenin içindeki verileri şifrelemek ve korumalı verileri sorunsuz bir şekilde yedekleme (daha fazla bilgi edinin [şifrelenmiş verilerin yedekleme](backup-azure-vms-encryption.md)).

## <a name="calculating-the-cost-of-protected-instances"></a>Korumalı örnekler maliyetini hesaplama
Azure Backup yedeklenen Azure sanal makineleri olan konusu [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/). Korunan örnekler hesaplaması dayanır *gerçek* geçici depolama hariç olmak üzere sanal makinede--tüm verilerin toplamıdır sanal makine boyutu.

VM'lerin yedeklenmesi için fiyatlandırma sanal makineye bağlı her veri diski için desteklenen en büyük boyut temel almaz. Fiyatlandırma veri diski depolanan gerçek verileri temel alır. Benzer şekilde, Azure Yedekleme'de her kurtarma noktası gerçek verileri toplamı olduğu depolanan veri miktarı yedekleme alanı faturada dayanır.

Örneğin, bir standart A2 boyutu en fazla 1 TB'lık iki ek veri diskleri olan sanal makine yararlanın. Aşağıdaki tabloda bu disklerin her biri üzerinde depolanan gerçek veri sunar:

| Disk türü | Maksimum boyut | Gerçek veri yok |
| --------- | -------- | ----------- |
| İşletim sistemi diski |1023 GB |17 GB |
| Yerel disk / geçici disk |135 GB |5 GB (yedekleme için dahil değil) |
| Veri diski 1 |1023 GB |30 GB |
| Veri diski 2 |1023 GB |0 GB |

Gerçek Boyut sanal makinenin, 17 GB + 30 GB + 0 GB = 47 GB bu durumda olur. Bu korumalı örnek boyutu (GB 47) başına aylık fatura olur. Sanal makinede veri miktarı büyüdükçe, korumalı örnek boyutu için faturalandırma değişiklikleri uygun şekilde kullanılır.

Faturalandırma, ilk başarılı yedekleme tamamlanana kadar başlatılmaz. Bu noktada, hem depolama hem de korunan örnekler için faturalandırma başlar. Sanal makine için bir kasada depolanan yedekleme tüm veriler var olduğu sürece, faturalandırma devam eder. Sanal makinede korumayı durdurun, ancak sanal makine yedekleme verilerini bir kasada var ise, faturalandırma devam eder.

Belirtilen bir sanal makine faturalandırması, yalnızca koruma durduruldu ve tüm yedekleme verileri silinir durdurur. Koruma durduğunda ve yedekleme etkin iş yok, en son başarılı VM yedekleme boyutu için aylık fatura kullanılan korumalı örnek boyutu haline gelir.

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
* [Sanal makineleri yedekleme](backup-azure-arm-vms.md)
* [Sanal makine yedeklemeyi yönetme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md)
* [VM yedekleme sorunlarını giderme](backup-azure-vms-troubleshoot.md)
