---
title: Azure geçişi, Toplayıcı gerecini | Microsoft Docs
description: Toplayıcı gerecini ve nasıl yapılandırılacağına ilişkin genel bir bakış sağlar.
author: ruturaj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: ruturajd
services: azure-migrate
ms.openlocfilehash: c99d0f74dbb8cc28cabebae60fe10645f4bdb3b6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308468"
---
# <a name="collector-appliance"></a>Toplayıcı Gereci

[Azure geçişi](migrate-overview.md) Azure'a geçiş için şirket içi iş yüklerini değerlendirir. Bu makalede, Toplayıcı gerecini kullanma hakkında bilgi sağlar.



## <a name="overview"></a>Genel Bakış

Azure geçişi toplayıcısı şirket içi vCenter ortamınızı bulmak için kullanılan basit bir gereçtir. Bu gerecin şirket içi VMware makinelerini bulur ve bunlar hakkındaki meta veriler Azure geçişi hizmetine gönderir.

Toplayıcı gerecini Azure geçişi projeden indirebileceğiniz bir OVF ' dir. 4 çekirdek, 8 GB RAM ve 80 GB'lık bir disk ile VMware sanal makine örneğini oluşturduğunda. Windows Server 2012 R2 (64-bit) gerecinin işletim sistemidir.

Toplayıcı adımları izleyerek oluşturabilirsiniz burada - [Toplayıcı VM'yi oluşturma işlemini](tutorial-assessment-vmware.md#create-the-collector-vm).

## <a name="collector-communication-diagram"></a>Toplayıcı iletişim diyagramı

![Toplayıcı iletişim diyagramı](./media/tutorial-assessment-vmware/portdiagram.PNG)


| Bileşen      | Şununla iletişim kurmak için   | Gereken bağlantı noktası                            | Neden                                   |
| -------------- | --------------------- | ---------------------------------------- | ---------------------------------------- |
| Toplayıcı      | Azure Geçişi hizmeti | TCP 443                                  | Toplayıcı SSL bağlantı noktası 443 üzerinden hizmetiyle iletişim kurabilir |
| Toplayıcı      | vCenter Server        | Varsayılan 443                             | Toplayıcı vCenter sunucusu ile iletişim kurabildiğini olmalıdır. 443 üzerinden vCenter varsayılan olarak bağlanır. VCenter farklı bir bağlantı noktasında dinliyorsa, bağlantı noktası Toplayıcı üzerinde giden bağlantı noktası olarak kullanılabilir olması gerekir |
| Toplayıcı      | RDP|   | TCP 3389 | Toplayıcı makineye yönelik RDP kullanabilmek için |





## <a name="collector-pre-requisites"></a>Toplayıcı ön koşullar

Azure geçişi hizmetine bağlanın ve bulunan verileri karşıya yükleme emin olmak için birkaç önkoşul denetimlerini geçmek Toplayıcı gerekir. Bu makalede her Önkoşullar görünür ve neden gerekli olduğunu anlamalısınız.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Toplayıcı gerecini bulunan makineler bilgileri göndermek için internet'e bağlı olması gerekir. Makine aşağıdaki iki yöntemden biri bir internet'e bağlanabilir.

1. Doğrudan internet bağlantınız için toplayıcıyı yapılandırabilirsiniz.
2. Proxy sunucu üzerinden bağlanmak için toplayıcı yapılandırabilirsiniz.
    * Proxy sunucusu kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola bağlantı ayarlarında belirtebilirsiniz.
    * Proxy sunucusunun IP adresini/FQDN'yi biçiminde olmalıdır http://IPaddress veya http://FQDN. Yalnızca http proxy'si desteklenir.

> [!NOTE]
> HTTPS tabanlı Ara sunucuları toplayıcı tarafından desteklenmez.

#### <a name="whitelisting-urls-for-internet-connection"></a>İnternet bağlantısı için URL'leri beyaz listeye ekleme

Önkoşul denetimi Toplayıcı sağlanan ayarları aracılığıyla İnternet'e bağlanabiliyorsa başarılı olur. Bağlantı denetimi URL'lerin bir listesini aşağıdaki tabloda verilen bağlanarak doğrulanır. Herhangi bir URL tabanlı güvenlik duvarı proxy'si kullanıyorsanız giden bağlantıyı denetlemek için bu URL'ler gerekli izin verilenler listesine emin olun:

**URL** | **Amacı**  
--- | ---
*.portal.azure.com | Azure hizmeti bağlantısını denetleyin ve zaman eşitleme doğrulamak için gereken sorunlar.

Ayrıca, onay ayrıca aşağıdaki URL'lere bağlantı doğrulamaya çalışır ancak onay erişilemiyor, başarısız olmaz. Aşağıdaki URL'ler için beyaz liste yapılandırmak isteğe bağlıdır, ancak Önkoşul denetimi azaltmak için el ile adımları uygulamanız gerekir.

**URL** | **Amacı**  | **Beyaz liste olmadığında ne olur**
--- | --- | ---
*.oneget.org:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yüklemesi başarısız olur. Modül el ile yükleyin.
*.windows.net:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yüklemesi başarısız olur. Modül el ile yükleyin.
*.windowsazure.com:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yüklemesi başarısız olur. Modül el ile yükleyin.
*. powershellgallery.com:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yüklemesi başarısız olur. Modül el ile yükleyin.
*.msecnd.net:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yüklemesi başarısız olur. Modül el ile yükleyin.
*.visualstudio.com:443 | Gerekli powershell indirmek için vCenter Powerclı modülü temel. | Powerclı yüklemesi başarısız olur. Modül el ile yükleyin.

### <a name="time-is-in-sync-with-the-internet-server"></a>Internet sunucusuyla eşitlenmiş saat

Toplayıcı hizmet isteklerine kimlik doğrulaması sağlamak için internet saat sunucusuyla eşitlenmiş olması gerekir. Böylece zaman doğrulanmış portal.azure.com url Toplayıcısından erişilebilir olmalıdır. Makine eşitleme dışı ise, saatin geçerli zamanı, aşağıdaki şekilde eşleşecek şekilde Toplayıcı VM üzerinde değişiklik gerekir:

1. VM üzerinde bir yönetici komut istemi açın.
1. Saat dilimi denetlemek için w32tm /tz çalıştırın.
1. Zaman eşitlenecek w32tm/resync çalıştırın.

### <a name="collector-service-should-be-running"></a>Toplayıcı hizmetinin çalışıyor olması gerekir

Azure geçişi Toplayıcısı hizmeti makinede çalıştırılması. Makinenin önyükleme işlemi sırasında bu hizmeti otomatik olarak başlatılır. Hizmet çalışmıyorsa başlatabilirsiniz *Azure geçişi toplayıcısı* Denetim Masası aracılığıyla hizmet. VCenter sunucusuna bağlanmak için makine meta verileri ve performans verilerini toplama ve hizmete göndermek için toplayıcı hizmetinin sorumludur.

### <a name="vmware-powercli-65"></a>VMware Powerclı 6.5

VMware powerclı'yı powershell modülü Toplayıcı makine ayrıntılarını ve performans verileri için sorgu ve vCenter sunucusu ile iletişim kurabilmesi için yüklenmesi gerekir. Powershell modülü otomatik olarak indirilir ve önkoşul denetimi bir parçası olarak yüklenir. Otomatik Yükleme gerektirir ya da sağlayan başarısız birkaç URL'leri beyaz beyaz listeye ekleme tarafından erişim veya modülünü el ile yükleme.

Modül aşağıdaki adımları kullanarak el ile yükleyin:

1. Powerclı Toplayıcısında internet bağlantısı olmadan yüklemek için verilen adımları izleyin. [bu bağlantıyı](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html) .
2. İnternet erişimi olan başka bir bilgisayardakiler de PowerShell modülü yükledikten sonra dosyaları VMware.* bu makineden Toplayıcı makineye kopyalayın.
3. Önkoşul denetimleri yeniden başlatın ve Powerclı yüklü olduğunu onaylayın.

## <a name="connecting-to-vcenter-server"></a>VCenter Server'a bağlanma

Toplayıcı, vCenter Server'a bağlanın ve sanal makineler, bunların meta verileri ve performans sayaçlarıyla sorgulayabilir. Bu veriler, proje tarafından bir değerlendirme hesaplamak için kullanılır.

1. VCenter Server'a bağlanmak için aşağıdaki tabloda verilen izinlerine sahip salt okunur bir hesap bulmayı çalıştırmak için kullanılabilir.

    |Görev  |Gerekli rol/hesap  |İzinler  |
    |---------|---------|---------|
    |Toplayıcı Gereci tabanlı bulma    | En az bir salt okunur kullanıcı gerekir        |Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only         |

2. Bulma için yalnızca belirtilen vCenter hesabı tarafından erişilebilen veri merkezlerinden erişilebilir.
3. VCenter vCenter sunucusuna bağlanmak için FQDN/IP adresi belirtmeniz gerekir. Varsayılan olarak, bu bağlantı noktası 443 üzerinden bağlanır. VCenter farklı bir bağlantı noktası üzerinde dinleyecek şekilde yapılandırdıysanız, sunucu adresi IPAddress:Port_Number veya FQDN:Port_Number biçiminde bir parçası olarak belirtebilirsiniz.
4. Dağıtımı başlatmadan önce vCenter Server için istatistik ayarları düzeyini 3 olarak ayarlanması gerekir. Düzey 3'ten daha düşükse bulma tamamlanır, ancak depolama ve ağ için performans verileri toplanmaz. Değerlendirme boyut önerileri, CPU ve bellek için performans verilerini ve yapılandırma verilerini disk ve ağ bağdaştırıcıları için yalnızca bu durumda hesaplanır. [Daha fazla bilgi edinin](./concepts-collector.md) hangi verilerin toplandığını ve değerlendirme nasıl etkiler.
5. Toplayıcı, bir ağ görebilmesi için vCenter sunucusuna sahip olmalıdır.

> [!NOTE]
> Yalnızca vCenter Server sürümleri 5.5, 6.0 ve 6.5 resmi olarak desteklenir.

> [!IMPORTANT]
> Böylece tüm sayaçları düzgün toplanan istatistikleri düzeyi için en yüksek genel düzeyde (3) ayarlamanızı öneririz. Daha düşük bir düzeyde ayarlamak vCenter varsa, yalnızca birkaç sayaçları 0 olarak ayarlamak geri kalanı ile tamamen toplanabilir. Değerlendirmeyi, ardından eksik veri gösterebilir.

### <a name="selecting-the-scope-for-discovery"></a>Bulma kapsamını seçme

VCenter bağlandıktan sonra bulmak için bir kapsamı seçebilirsiniz. Kapsam seçilmesi, tüm sanal makineler belirtilen vCenter stok yolundan bulur.

1. Kapsam, bir veri merkezi, bir klasör veya ESXi konağı olabilir.
2. Aynı anda yalnızca bir kapsamı seçebilirsiniz. Daha fazla sanal makine seçmek için bir bulma tamamlamak ve yeni bir kapsam ile keşif işlemini yeniden başlatın.
3. Yalnızca olan kapsamı seçebilirsiniz *1500'den küçük sanal makineler*.

## <a name="specify-migration-project"></a>Geçiş projesi belirtin

Şirket içi vCenter bağlı ve kapsam belirtilen sonra Keşif ve değerlendirme için kullanılması gereken geçiş proje ayrıntılarını artık belirtebilirsiniz. Proje kimliği ve anahtarını belirtin ve bağlanın.

## <a name="start-discovery-and-view-collection-progress"></a>Bulma ve görüntüleme toplama ilerleme durumunu Başlat

Bulma işlemi başladıktan sonra vCenter sanal makineler bulunur ve meta verileri ve performans verilerini sunucuya gönderilir. İlerleme durumunu, aşağıdaki kimlikler bildirir:

1. Toplayıcı kimliği: Toplayıcı makinenize verilen benzersiz bir kimliği. Bu kimliği, belirli bir makine için farklı bulmaların arasında değiştirmez. Bu kimliği hataları durumunda Microsoft Support sorunu bildirirken kullanabilirsiniz.
2. Oturum kimliği: Çalışan iş koleksiyonu için benzersiz bir kimliği. Bulma iş tamamlandığında Portalı'nda aynı oturum Kimliğine başvurabilirsiniz. Bu kimliği her toplama işine değiştirir. Hataları olması durumunda, bu kimliği Microsoft Support bildirebilirsiniz.

### <a name="what-data-is-collected"></a>Verilerin ne toplanır?

Seçili sanal makinelerle ilgili olarak aşağıdaki statik meta veri toplama işini bulur.

1. VM görünen adı (temel, vCenter)
2. Sanal makinenin envanteri yolu (konak/klasör vcenter)
3. IP adresi
4. MAC adresi
5. İşletim sistemi
5. Çekirdek, disk, NIC sayısı
6. Bellek boyutu, Disk boyutları
7. Ve VM, disk ve aşağıdaki tabloda listelendiği gibi ağ performans sayaçları.

Aşağıdaki tabloda toplanır ve aynı zamanda belirli bir sayaç alınamadı, etkilenen değerlendirme sonuçları listeler performans sayaçları listeler.

|Sayaç                                  |Düzey    |Cihaz başına düzeyi  |Etki değerlendirmesi                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|mem.Usage.average                        | 1       |NA                |Önerilen VM boyutu ve maliyet                    |
|virtualDisk.read.average                 | 2       |2                 |Disk boyutu, depolama maliyetini ve VM boyutu         |
|virtualDisk.write.average                | 2       |2                 |Disk boyutu, depolama maliyetini ve VM boyutu         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Disk boyutu, depolama maliyetini ve VM boyutu         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Disk boyutu, depolama maliyetini ve VM boyutu         |
|NET.Received.average                     | 2       |3                 |VM boyutu ve ağ maliyeti                        |
|NET.transmitted.average                  | 2       |3                 |VM boyutu ve ağ maliyeti                        |

> [!WARNING]
> Daha yüksek bir istatistik düzeyini yeni ayarladıysanız, bir gün için performans sayaçları oluşturma sürer. Bu nedenle, bir gün sonra bulma çalıştırmanızı öneririz.

### <a name="time-required-to-complete-the-collection"></a>Koleksiyon tamamlamak için gereken süre

Toplayıcı yalnızca makine verileri bulur ve projeye gönderir. Proje bulunan verileri portalda görüntülenir ve değerlendirme oluşturmaya başlamadan önce ek zaman alabilir.

Seçilen kapsamda sanal makinelerin sayısına bağlı olarak, en fazla 15 dakika projeye statik meta veri göndermek için sürer. Statik meta verileri portalda kullanılabilir duruma geldikten sonra portalda makineler listesini görmek ve grupları oluşturmaya başlayın. Koleksiyon işi tamamlar ve proje işlenen veri kadar bir değerlendirme oluşturulamaz. Bir kez toplama işi Toplayıcısında tamamlandı, uygulamanın en fazla sürebilir bir saat için portalda kullanılabilir olması performans verilerini seçilen kapsam sanal makinelerin sayısına dayalı olarak.

## <a name="locking-down-the-collector-appliance"></a>Toplayıcı gerecini kilitleme
Toplayıcı gerecini üzerinde sürekli Windows güncelleştirmeleri çalıştırmanızı öneririz. Bir Toplayıcı 60 gün boyunca güncelleştirilmezse, Toplayıcı makinesi otomatik olarak kapatılıyor başlar. Bir bulma çalışıyorsa, 60 gün süresi olsa bile makine kapalı, açılır değil. POST bulma işi tamamlar, makine kapatılır. Toplayıcı 45 günden fazla bir süre için kullanıyorsanız, her zaman çalışan Windows update tarafından güncelleştirme zamanı makine tutma öneririz.

Ayrıca gerecinize güvenliğini sağlamak için aşağıdaki adımları öneririz
1. Paylaşım değil veya yönetici parolaları yetkisiz kişilerle misplace.
2. Gereç kullanımda olmadığında kapatın.
3. Güvenli bir ağ Gereci yerleştirin.
4. Geçiş işi tamamlandıktan sonra gereç örneğini silin. Diskler üzerinde önbelleğe vCenter kimlik bilgilerini olabilir (Vmdk) dosyalarını yedekleme disk da silmek emin olun.

## <a name="how-to-upgrade-collector"></a>Toplayıcı yükseltme

OVA yine indirmeden Toplayıcı en son sürüme yükseltebilirsiniz.

1. En son sürümünü indirin [yükseltme paketi](https://aka.ms/migrate/col/upgrade_9_13) (sürüm 1.0.9.13).
2. İndirilen düzeltme güvenli olmasını sağlamak için yönetici komut penceresi açın ve karma ZIP dosyası oluşturmak için aşağıdaki komutu çalıştırın. Oluşturulan karma karşı belirli bir sürüm belirtilen karma değeri ile eşleşmesi gerekir:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (örnek kullanım C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.7.zip SHA256)
3. Azure geçişi toplayıcısı sanal makineye (Toplayıcı gerecini) zip dosyasını kopyalayın.
4. Zip dosyasını sağ tıklatın ve tümünü Ayıkla'ı seçin.
5. Setup.ps1 üzerinde sağ tıklayın ve PowerShell ile Çalıştır'ı seçin ve güncelleştirmeyi yüklemek için ekrandaki yönergeleri izleyin.

### <a name="list-of-updates"></a>Güncelleştirmelerin listesi

#### <a name="upgrade-to-version-10913"></a>1.0.9.13 sürümüne yükseltin

Karma değerleri yükseltme [paketini 1.0.9.13](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e

#### <a name="upgrade-to-version-10911"></a>1.0.9.11 sürümüne yükseltin

Karma değerleri yükseltme [paketini 1.0.9.11](https://aka.ms/migrate/col/upgrade_9_11)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

#### <a name="upgrade-to-version-1097"></a>Sürüm 1.0.9.7 yükseltme

Karma değerleri yükseltme [paketini 1.0.9.7](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi VMware Vm'leri için bir değerlendirmenin ayarlayın](tutorial-assessment-vmware.md)
