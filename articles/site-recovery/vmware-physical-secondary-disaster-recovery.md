---
title: VMware Vm'lerini veya fiziksel sunucuları Azure Site Recovery ile ikincil bir siteye olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: VMware Vm'leri veya Windows ve Linux fiziksel sunucuları Azure Site Recovery ile ikincil bir siteye olağanüstü durum kurtarma ayarlamayı öğrenin.
services: site-recovery
author: nsoneji
manager: gauarvd
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 251e2b1f8785408bf441bcbcf3d0fcbdd767a358
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38479491"
---
# <a name="set-up-disaster-recovery-of-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Şirket içi VMware sanal makinelerini veya fiziksel sunucuları ikincil bir siteye olağanüstü durum kurtarmayı ayarlama

Inmage Scout içinde [Azure Site Recovery](site-recovery-overview.md) VMware siteleri arasında şirket içi gerçek zamanlı çoğaltma sağlar. Inmage Scout Azure Site Recovery hizmeti aboneliklerine de dahildir. 


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- [Gözden geçirme](vmware-physical-secondary-support-matrix.md) tüm bileşenleri için destek gereksinimleri.
- Çoğaltmak istediğiniz makineleri ile uyumlu olduğundan emin [makine desteği çoğaltılan](vmware-physical-secondary-support-matrix.md#replicated-vm-support).


## <a name="create-a-vault"></a>Kasa oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="choose-a-protection-goal"></a>Koruma hedefi seçin

Çoğaltma ve nereye çoğaltmak için ne seçin.

1. Tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
2. Seçin **kurtarma sitesine** > **Evet, VMware vSphere Hypervisor ile**. Daha sonra, **Tamam**'a tıklayın.
3. İçinde **Scout Kurulumu**, Inmage Scout 8.0.1 GA yazılımı ve kayıt anahtarını indirin. Tüm bileşenler için kurulum dosyalarını indirilen .zip dosyasına dahil edilir.

## <a name="download-and-install-component-updates"></a>Bileşen Güncelleştirmeleri indirmek ve yüklemek

 Gözden geçirin ve en son yükleme [güncelleştirmeleri](#updates). Güncelleştirmeleri sunucuları aşağıdaki sırayla yüklü olması gerekir:

1. RX sunucu (varsa)
2. Yapılandırma sunucusu
3. İşlem sunucusu
4. Ana hedef sunucuları
5. vContinuum sunucuları
6. Kaynak sunucuyu (hem Windows hem de Linux sunucuları)

Güncelleştirmeleri gibi yükleyin:

> [!NOTE]
>Tüm Scout bileşenleri dosya güncelleştirme sürümüne güncelleştirme .zip dosyasını aynı olmayabilir. Eski sürümü olduğunu hiçbir değişiklik bileşen bu güncelleştirmenin önceki bir güncelleştirme bu yana gösterir.

İndirme [güncelleştirme](https://aka.ms/asr-scout-update6) .zip dosyası. Dosya şu bileşenler bulunur: 
  - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
  - CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe
  - UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
  - UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
  - vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe
  - UA update4 BITS RHEL5 OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
1. .Zip dosyalarını ayıklayın.
2. **RX sunucu**: kopyalama **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** RX sunucuya ve ayıklayın. Ayıklanan klasöründe Çalıştır **/Install**.
3. **Yapılandırma sunucusu ve işlem sunucusu**: kopyalama **CX_Windows_8.0.6.0_GA_Update_6_13746667_18Sep17.exe** yapılandırma sunucusu ve işlem sunucusu. Çalıştırmak için çift tıklayın.<br>
4. **Windows ana hedef sunucusu**: Birleşik aracıyı güncelleştirin, kopyalayın **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** sunucu. Çalıştırmak için çift tıklayın. Ayrıca, kaynak sunucu için aynı birleşik aracı güncelleştirme geçerlidir. Kaynak güncelleştirme 4'e güncelleştirilmemişse birleşik aracı güncelleştirmeniz.
  Güncelleştirme, ana uygulama gerekmez. hedef hazırlanmış ile **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** en son değişiklikleri yeni GA yükleyici olarak.
5. **vContinuum sunucusu**: kopyalama **vCon_Windows_8.0.6.0_GA_Update_6_11525767_21Sep17.exe** sunucu.  VContinuum sihirbazını kapattığınız emin olun. Çalıştırmak için dosyaya çift tıklayın.
    Güncelleştirme ile hazırladığınız ana hedef uygulamak gerekmez **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** en son değişiklikleri yeni GA yükleyici olarak.
6. **Linux ana hedef sunucusu**: Birleşik aracıyı güncelleştirin, kopyalayın **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** ana hedef sunucu ve ayıklayın. Ayıklanan klasöründe Çalıştır **/Install**.
7. **Kaynak sunucu Windows**: Birleşik aracıyı güncelleştirin, kopyalayın **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** kaynak sunucuya. Çalıştırmak için dosyaya çift tıklayın. 
    Kaynak sunucuda, güncelleştirme 4'e zaten güncelleştirildi veya kaynak aracısının en son temel Yükleyici ile yüklü güncelleştirme 5 Aracısı'nı yüklemeniz gerekmez **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe**.
8. **Linux kaynak sunucu**: Birleşik Aracı'nı güncelleştirmek için birleşik aracı dosyasını ilgili sürümünü Linux sunucuya kopyalayın ve ayıklayın. Ayıklanan klasöründe Çalıştır **/Install**.  Örnek: RHEL 6,7 64 bit sunucu kopyalama **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** sunucuya ve ayıklayın. Ayıklanan klasöründe Çalıştır **/Install**.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. VMware siteleri hedef ve kaynak arasında çoğaltmayı ayarlayın.
2. Yükleme, koruma ve kurtarma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

   * [Sürüm notları](https://aka.ms/asr-scout-release-notes)
   * [Uyumluluk matrisi](https://aka.ms/asr-scout-cm)
   * [Kullanıcı Kılavuzu](https://aka.ms/asr-scout-user-guide)
   * [RX Kullanıcı Kılavuzu](https://aka.ms/asr-scout-rx-user-guide)
   * [Hızlı Yükleme Kılavuzu](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Güncelleştirmeler

### <a name="site-recovery-scout-801-update-6"></a>Site Recovery Scout 8.0.1 güncelleştirmesi 6 
Güncelleştirme: 12 Ekim 2017

İndirme [Scout güncelleştirme 6](https://aka.ms/asr-scout-update6).

Scout güncelleştirme 6 toplu bir güncelleştirmesidir. Bu güncelleştirme 1 için güncelleştirme 5 yanı sıra yeni düzeltmeleri ve geliştirmeleri aşağıda açıklanan tüm düzeltmeleri içerir. 

#### <a name="new-platform-support"></a>Yeni platform desteği
* Kaynak Windows Server 2016 için destek eklendi.
* Aşağıdaki Linux işletim sistemleri için destek eklenmiştir:
    - Red Hat Enterprise Linux (RHEL) 6.9
    - CentOS 6.9
    - Oracle Linux 5.11
    - Oracle Linux 6,8
* VMware için merkezi 6.5 desteği eklendi

> [!NOTE]
> * Windows için temel Agent(UA) birleştirilmiş yükleyici, Windows Server 2016'yı desteklemek için yenilendi. Yeni yükleyici **InMage_UA_8.0.1.0_Windows_GA_28Sep2017_release.exe** temel Scout GA paket ile paketlenmiştir (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Aynı yükleyici, tüm desteklenen Windows sürümü için kullanılır. 
> * Temel Windows vContinuum & ana hedef Yükleyicisi için destek Windows Server 2016 yenilenmiş meydana geldi. Yeni yükleyici **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_10Oct2017_release.exe** temel Scout GA paket ile paketlenmiştir (**InMage_Scout_Standard_8.0.1 GA-Oct17.zip**). Aynı yükleyici Windows 2016 ana hedef ve Windows 2012 R2 ana hedef dağıtmak için kullanılır.
> * Açıklanan şekilde portaldan GA paketi indirmek [kasa oluşturma](#create-a-vault).
>

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri
- Çoğaltılacak diskler listesi ile Linux VM için yeniden koruma başarısız yapılandırma sonunda boş.

### <a name="site-recovery-scout-801-update-5"></a>Site kurtarma Scout 8.0.1 güncelleştirme 5
Scout güncelleştirme 5 toplu bir güncelleştirmesidir. Bu, güncelleştirme 1'deki tüm düzeltmeleri güncelleştirme 4 ve aşağıda açıklanan yeni düzeltmeleri içerir.
- Güncelleştirme 5 ile Site kurtarma Scout güncelleştirme 4'ten düzeltmeleri özellikle ana hedef ve vContinuum için bileşenlerdir.
- Ardından kaynak sunucular, ana hedef, yapılandırma, işlem ve RX sunucuları zaten Update 4 çalıştırıyorsanız, yalnızca ana hedef sunucusunda uygulayın. 

#### <a name="new-platform-support"></a>Yeni platform desteği
* SUSE Linux Enterprise Server 11 4(SP4) hizmet paketi
* SLES 11 SP4 64 bit **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** temel Scout GA paket ile paketlenmiştir (**InMage_Scout_Standard_8.0.1 GA.zip**). Açıklanan şekilde portaldan GA paketi indirmek [kasa oluşturma](#create-a-vault).


#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri

* Güvenilirlik düzeltmeleri daha yüksek bir Windows kümesi için destek:
    * Düzeltildi-P2V MSCS bazıları küme diskleri haline kurtarma işleminden sonra ham.
    * Fixed-P2V MSCS küme kurtarma disk siparişi uyumsuzluğu nedeniyle başarısız olur.
    * Disk boyutu uyumsuzluğu hatası ile başarısız diskleri ekleme işlemi sabit MSCS küme.
    * Sabit hazırlık durumunu denetle kaynağının RDM LUN'ları eşleme ile MSCS küme boyutu doğrulaması başarısız olur.
    * Fixed-tek düğüm Küme korumayı SCSI uyumsuzluk sorunları nedeniyle başarısız olur. 
    * Hedef küme diskleri varsa Fixed-P2V Windows Küme sunucusu yeniden koruma başarısız olur. 
    
* Düzeltildi: Seçili ana hedef sunucusu vContinuum yanlış ana hedef sunucunun, yeniden çalışma kurtarma ve Kurtarma sırasında seçer. daha sonra yeniden koruma sırasında korumalı kaynak makineyle aynı ESXi sunucusunda (İleri koruma sırasında), değildir işlem başarısız olur.

> [!NOTE]
> * P2V küme düzeltmeleri, yalnızca yeni Site kurtarma Scout güncelleştirme 5 ile korunan fiziksel MSCS kümeler için geçerlidir. Eski güncelleştirmeleri korumalı P2V MSCS kümeleriyle küme düzeltmeleri yüklemek için 12 bölümünde belirtildiği yükseltme adımları [Site kurtarma Scout sürüm notları](https://aka.ms/asr-scout-release-notes).
> * Başlangıçta korumalı oldukları gibi yeniden koruma süresi aynı disk kümesi her küme düğümleri üzerinde etkin olan, yeniden koruma fiziksel bir MSCS kümesindeki yalnızca var olan hedef diskler kullanabilirsiniz. Aksi takdirde, el ile yapılacak adımlar bölümünü 12 kullanın [Site kurtarma Scout sürüm notları](https://aka.ms/asr-scout-release-notes), hedef tarafı disklerin yeniden koruma sırasında yeniden kullanım için doğru veri deposunu yoluna taşıyın. P2V modunda MSCS Küme yükseltme adımları izleyerek olmadan yeniden koruma, hedef ESXi sunucusunda yeni bir disk oluşturur. Eski diskleri deposundan el ile silmeniz gerekir.
> * Bir kaynağı SLES11 veya SLES11 (ile herhangi bir hizmet paketi) sunucusu düzgün bir şekilde yeniden başlatıldığında, el ile işaretleme **kök** disk çoğaltma çiftlerinin yeniden eşitleme. CX arabirimindeki bildirim yoktur. Yeniden eşitleme için kök disk işaretlemeyin, veri bütünlüğü sorunları görebilirsiniz.


### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site kurtarma Scout 8.0.1 güncelleştirme 4
Scout güncelleştirme 4 toplu bir güncelleştirmesidir. Bu, güncelleştirme 1'deki tüm düzeltmeleri güncelleştirme 3 ve aşağıda açıklanan yeni düzeltmeleri içerir.

#### <a name="new-platform-support"></a>Yeni platform desteği

* VCenter/vSphere için 6.0, 6.1 ve 6.2 desteği eklendi
* Bu Linux işletim sistemleri için destek eklenmiştir:
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 ve 7.2
  * 7.0, 7.1 ve 7.2 centOS
  * Red Hat Enterprise Linux (RHEL) 6,8
  * CentOS 6,8

> [!NOTE]
> RHEL/CentOS 7 64-bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** temel Scout GA paket ile paketlenmiştir **InMage_Scout_Standard_8.0.1 GA.zip**. Scout GA paket açıklandığı gibi portaldan indirme [kasa oluşturma](#create-a-vault).

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri

* Geliştirilmiş Kapat: aşağıdaki Linux işletim sistemleri ve kopyaları, istenmeyen yeniden eşitleme sorunlarını önlemek için işleme
    * Red Hat Enterprise Linux (RHEL) 6.x
    * Oracle Linux (OL) 6.x
* Linux için birleşik aracı yükleme dizinindeki tüm klasör erişim izinlerinin artık yalnızca yerel kullanıcı kısıtlanır.
* Windows, bir zaman aşımından ortak dağıtılmış tutarlılık yer işaretleri, verirken gerçekleşen bir sorun için bir düzeltme üzerinde yoğun olarak SQL Server ve paylaşım noktası kümeleri gibi dağıtılmış uygulamaların yüklenme zamanı.
* Günlük ilgili düzeltme yapılandırma sunucusu temel yükleyicisi.
* Bir indirme bağlantısı için VMware vCLI 6.0, Windows ana hedef temel yükleyici eklendi.
* Ek denetimler ve günlükleri, ağ yapılandırma değişikliklerini yük devretme ve olağanüstü durum kurtarma tatbikatları sırasında eklendi.
* Bekletme bilgileri, yapılandırma sunucusuna bildirilmesini değil neden olan sorunu için düzeltme.  
* Fiziksel kümeler için birim yeniden boyutlandırma vContinuum Sihirbazı'nda, Kaynak birimde küçültülürken başarısız olmasına neden olan sorunu için düzeltme.
* Hata ile başarısız olan bir küme koruma sorun için bir düzeltme: "Başarısız disk imzası bulmak", küme diskinin PRDM disk olduğunda.
* Bir aralık dışı özel durumun bir cxps aktarım sunucusu kilitlenme için düzeltme.
* Sunucu adı ve IP adresi sütunlar artık yeniden boyutlandırılabilir içinde **anında yükleme** vContinuum Sihirbazı sayfası.
* RX API geliştirmeleri:
  * Beş en son kullanılabilir ortak tutarlılık artık kullanılabilir (yalnızca etiketler garanti) işaret eder.
  * Tüm korumalı cihazlar için kapasite ve boş alan ayrıntıları görüntülenir.
  * Scout sürücü durumu kaynak sunucu üzerindeki kullanılabilir.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** temel paket vardır:
    * Güncelleştirilen configuration server temel Yükleyicisi (**InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe**)
    * Bir Windows ana hedef temel Yükleyici (**InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**).
    * Tüm yeni yüklemeleri için yeni yapılandırma sunucusu ve Windows ana hedef GA BITS kullanın.
> * Güncelleştirme 4, doğrudan 8.0.1 üzerinde uygulanabilir GA
> * Yapılandırma sunucusu ve RX güncelleştirmeleri geri bunlar uygulanmış sonra alınamaz.


### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site kurtarma Scout 8.0.1 güncelleştirme 3

Tüm Site Recovery güncelleştirmeleri birikmeli özelliktedir. Güncelleştirme 3, güncelleştirme 1 ile güncelleştirme 2'deki tüm düzeltmeleri içerir. Güncelleştirme 3 8.0.1 üzerinde doğrudan da uygulanabilir GA Yapılandırma sunucusu ve RX güncelleştirmeleri geri bunlar uygulanmış sonra alınamaz.

#### <a name="bug-fixes-and-enhancements"></a>Hata düzeltmeleri ve geliştirmeleri
Güncelleştirme 3, aşağıdaki sorunları giderir:

* Proxy'nin arkasında olduklarında yapılandırma sunucusu ve RX kasaya kayıtlı değil.
* Sistem Durumu raporu saat kurtarma noktası hedefi (RPO) üst sınırına değildi sayısı güncelleştirilmez.
* Yapılandırma sunucusu ile RX eşitleniyor değil, ESX donanım ayrıntıları veya ağ ayrıntılarını içeren herhangi bir UTF-8 karakter.
* Windows Server 2008 R2 etki alanı denetleyicileri, Kurtarma işleminden sonra başlamaz.
* Çevrimdışı eşitleme beklendiği gibi çalışmıyor.
* VM yük devretme sonrasında çoğaltma çifti silme uzun bir süre yapılandırma sunucusu konsolunda ilerleme durumu değil. Kullanıcılar, yeniden çalışmayı tamamla veya çalışmanıza devam edin.
* Genel tutarlılık işi tarafından anlık görüntü işlemleri için iyileştirilmiş, uygulama azaltmaya yardımcı olmak için SQL Server istemcileri gibi bağlantısını keser.
* Tutarlılık Aracı (VACP.exe) performansı İyileştirildi. Windows üzerinde anlık görüntü oluşturmak için gereken bellek kullanımı azaltıldı.
* Anında iletme parola 16 karakterden daha büyük olduğunda hizmet kilitlenmeleri yükleyin.
* vContinuum denetleyin ve kimlik bilgileri değiştirildiğinde, yeni vCenter kimlik bilgileri için iste değil.
* Linux üzerinde ana hedef önbellek Yöneticisi'ni (cachemgr) işlem sunucusundan dosyaları karşıdan yükleme değil. Bu, çoğaltma çifti azaltma içinde sonuçlanır.
* Tüm düğümlerde aynı fiziksel yük devretme kümesi (MSCS) disk sırası olmadığı durumlarda, bazı küme birimler için çoğaltma ayarlanmamış. Bu düzeltme yararlanmak için küme korunduktan gerekir.  
* RX 8.0.1 Scout için Scout 7.1 yükseltildikten sonra SMTP işlevselliği beklendiği gibi çalışmıyor.
* Daha fazla istatistikleri tamamlamak için geçen süreyi izlemek için geri alma işlemi için günlüğüne sürümüne eklenmiştir.
* Kaynak sunucuda Linux işletim sistemleri için destek eklenmiştir:
  * Red Hat Enterprise Linux (RHEL) 6 güncelleştirmesi 7
  * CentOS 6, 7 güncelleştir
* Yapılandırma sunucusu ve RX konsolları artık bit eşlem moduna girer çifti için bildirimleri göster.
* Aşağıdaki güvenlik düzeltmeleri RX eklenmiştir:
    * Yetkilendirme atlama parametresi oynama aracılığıyla: geçerli olmayan kullanıcılar için kısıtlı erişim.
    * Siteler arası istek sahteciliğini: sayfa belirteci kavramı uygulanmıştır ve her sayfa için rasgele oluşturur. Bu, yalnızca tek bir oturum açma örneği aynı kullanıcı için yoktur ve sayfa yenileme işe yaramazsa anlamına gelir. Bunun yerine, panoya yönlendirir.
    * Kötü amaçlı dosya karşıya yükleme: dosyalar için belirli uzantılara kısıtlı: z AIFF, asf, AVI, bmp, csv, doc, docx, bayra, flv, GIF, gz, gzip, jpeg, jpg, günlüğü, Orta, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, RMI, rmvb, rtf , sdc, sitd, swf, sxc, sxw, hedefi, tgz, TIF, TIFF, txt, vsd, wav, wma, wmv, xls, xlsx, xml ve zip.
    * Kalıcı siteler arası betik: Giriş doğrulamaları eklendi.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site kurtarma Scout 8.0.1 güncelleştirme 2 (03 Ara 15 güncelleştirmesi)

Güncelleştirme 2'deki düzeltmeler şunları içerir:

* **Yapılandırma sunucusu**: 31 gün ücretsiz kullanım ölçümü engelleyen sorunları, Site Recovery yapılandırma sunucusunun kayıtlı olmadığında beklendiği gibi çalışmasını özelliği.
* **Birleşik aracı**: ana hedef sunucusunda 8.0 için 8.0.1 sürümünden yükseltme sırasında yüklenen değil güncelleştirmede sonuçlandı güncelleştirme 1'deki bir sorun düzeltildi.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site kurtarma Scout 8.0.1 güncelleştirme 1
Güncelleştirme 1, aşağıdaki hata düzeltmeleri ve yeni özellikler içerir:

* 31 gün ücretsiz koruma sunucu örneği başına. Bu, işlevselliğini test etmek veya bir kavram kanıtı kümesi sağlar.
* Yük devretme ve yeniden çalışma, da dahil, sunucu üzerindeki tüm işlemler için ilk 31 gün boyunca ücretsizdir. Bir sunucu ile Site kurtarma Scout ilk korunduğunda süresini başlatır. 32. günden, her bir korumalı sunucunun müşteriye ait bir site için Site Recovery koruması için standart örnek fiyatı üzerinden ücretlendirilir.
* Şu anda ücretlendirildiğiniz korunan sunucu sayısına göre herhangi bir zamanda kullanılabilir **Pano** kasadaki.
* Destek 5.5 güncelleştirme 2 için vSphere komut satırı arabirimi (vCLI) eklendi.
* Kaynak sunucuda aşağıdaki Linux işletim sistemleri için destek eklendi:
    * RHEL 6 güncelleştirmesi 6
    * RHEL 5 güncelleştirme 11
    * CentOS 6 güncelleştirmesi 6
    * CentOS 5 güncelleştirme 11
* Aşağıdaki sorunları ele almak hata düzeltmeleri:
  * Yapılandırma sunucusu veya RX sunucusu için kasa kaydı başarısız.
  * Küme birimleri kümelenmiş olmadığında bunlar sürdürme gibi VM'ler yeniden korunduktan beklendiği görünmez.
  * Ana hedef sunucusu sanal makineleri şirket içi üretimden farklı ESXi sunucusunda barındırıldığında yeniden çalışma başarısız olur.
  * Yapılandırma dosya izinleri 8.0.1 için yükseltme sırasında değiştirilir. Bu değişiklik, koruma ve işlemleri etkiler.
  * Yeniden eşitleme eşiği çoğaltma tutarsız davranışa neden beklendiği gibi zorlanmaz.
  * RPO ayarlarını doğru yapılandırma sunucusu konsolunda görünmez. Sıkıştırılmamış veri değeri yanlış sıkıştırılmış değeri gösterir.
  * Kaldırma işlemi vContinuum Sihirbazı'nda beklendiği gibi silmez ve çoğaltma yapılandırması sunucu konsolundan silinmez.
  * ' A tıkladığınızda vContinuum Sihirbazı'nda, disk otomatik olarak işaretli olmadığından **ayrıntıları** MSCS Vm'leri koruma sırasında disk görünümünde.
  * Fiziksel-sanal (P2V) senaryosunda, gerekli HP hizmetler (örneğin, CIMnotify ve CqMgHost) VM kurtarma kılavuzda taşınabilir değildir. Ek önyükleme zamanında bu sorun oluşur.
  * Ana hedef sunucu üzerinde 26'dan fazla disk olduğunda Linux VM koruma başarısız olur.

