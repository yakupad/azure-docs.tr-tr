---
title: Azure’a geçiş için şirket içi VMware VM’lerini Azure Geçişi ile bulma ve değerlendirme | Microsoft Docs
description: Azure’a geçiş için şirket içi VMware VM’lerinin Azure Geçişi hizmeti kullanılarak nasıl bulunacağı ve değerlendirileceği açıklanmaktadır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/09/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0b1070e29c8dc9f088297622d16fb816a10a55c0
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970794"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Azure’a geçiş için şirket içi VMware VM’lerini bulma ve değerlendirme

[Azure Geçişi](migrate-overview.md) hizmetleri, Azure’a geçiş için şirket içi iş yüklerini değerlendirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Geçişi’nin şirket içi VM’leri bulmak için kullanacağı bir hesap oluşturma
> * Bir Azure Geçişi projesi oluşturun.
> * Değerlendirmek için şirket içi VMware VM’lerini toplamak üzere bir şirket içi toplayıcı sanal makinesi (VM) ayarlayın.
> * VM’leri gruplandırın ve bir değerlendirme oluşturun.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Ön koşullar

- **VMware**: Geçirmeyi planladığınız sanal makineler, 5.5, 6.0 veya 6.5 sürümünü çalıştıran vCenter Server tarafından yönetilmelidir. Buna ek olarak, toplayıcı VM’yi dağıtmak için 5.0 veya daha sonraki sürüme sahip bir ESXi konağı gerekir.
- **vCenter Server hesabı**: vCenter Server’a erişmek için salt okunur bir hesabınız olması gerekir. Azure Geçişi, şirket içi VM’leri bulmak için bu hesabı kullanır.
- **İzinler**: vCenter Server’da, bir dosyayı .OVA biçiminde içeri aktararak VM oluşturma iznine sahip olmanız gerekir.
- **İstatistik ayarları**: vCenter Server için istatistik ayarları, dağıtım başlatılmadan önce düzey 3 olarak belirlenmelidir. Ayarlar düzey 3’ün altında olursa değerlendirme gerçekleştirilir, ancak depolama ve ağ için performans verileri toplanmaz. Bu durumda boyut önerileri, CPU ve belleğe ait performans verilerine ve diskin ve ağ bağdaştırıcılarının yapılandırma verilerine bağlı olarak yapılır.

## <a name="create-an-account-for-vm-discovery"></a>VM bulma işlemi için hesap oluşturma

Azure Geçişi’nin, değerlendirme amacıyla VM’leri otomatik olarak bulması için VMware sunucularına erişebilmesi gerekir. Aşağıdaki özelliklere sahip bir VMware hesabı oluşturun. Bu hesabı Azure Geçişi kurulumu sırasında belirtirsiniz.

- Kullanıcı türü: En azından salt okunur bir kullanıcı
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, rol=Salt okunur
- Ayrıntılar: Veri merkezi düzeyinde atanmış ve veri merkezindeki tüm nesnelere erişimi olan kullanıcı.
- Erişimi kısıtlamak için Alt nesneye yay ile Erişim yok rolünü alt nesnelere (vSphere konakları, veri depoları, VM’ler ve ağlar) atayın.


## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com)’da oturum açın.

## <a name="create-a-project"></a>Proje oluşturma

1. Azure portalında **Kaynak oluştur**’a tıklayın.
2. **Azure Geçişi** araması yapın ve arana sonuçlarında **Azure Geçişi** hizmetini seçin. Sonra **Oluştur**’a tıklayın.
3. Proje için bir proje adı ve Azure aboneliği belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projenin oluşturulacağı konumu belirtin ve sonra **Oluştur**’a tıklayın. Bir Azure Geçişi projesini yalnızca Orta Batı ABD veya Doğu ABD bölgesinde oluşturabilirsiniz. Ancak yine de herhangi bir hedef Azure konumu için geçişinizi planlayabilirsiniz. Proje için belirtilen konum yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır.

    ![Azure Geçişi](./media/tutorial-assessment-vmware/project-1.png)



## <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware VM’lerini bulur ve bunlara ilişkin meta verileri Azure Geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için, bir .OVA dosyası indirin ve VM’yi oluşturmak için dosyayı şirket içi vCenter sunucusuna aktarın.

1. Azure Geçişi projesinde **Kullanmaya Başlama** > **Bul ve Değerlendir** > **Makineleri Keşfet**’ye tıklayın.
2. **Makineleri keşfet** bölümünde .OVA dosyasını indirmek için **İndir**’e tıklayın.
3. **Proje kimlik bilgilerini kopyala** bölümünde proje kimliğini ve anahtarı kopyalayın. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.

    ![.ova dosyasını indir](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Dağıtmadan önce .OVA dosyasının güvenilir olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma bu ayarlara uygun olmalıdır.

  OVA sürüm 1.0.9.12 için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | d0363e5d1b377a8eb08843cf034ac28a
    SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
    SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

  OVA sürüm 1.0.9.8 için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | b5d9f0caf15ca357ac0563468c2e6251
    SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
    SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

    OVA sürüm 1.0.9.7 için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

    OVA sürüm 1.0.9.5 için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

    OVA 1.0.9.2 sürümü için

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

## <a name="create-the-collector-vm"></a>Toplayıcı VM’yi oluşturma

İndirilen dosyayı vCenter Server’a aktarın.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.

    ![OVF dağıtma](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. OVF Şablonu Dağıtma Sihirbazı > **Kaynak** bölümünde .ova dosyasının konumunu belirtin.
3. **Ad** ve **Konum** bölümünde toplayıcı VM için bir kolay ad ve VM’nin barındırılacağı envanter nesnesini belirtin.
5. **Konak/Küme** bölümünde, toplayıcı VM’nin çalıştırılacağı konağı veya kümeyi belirtin.
7. Depolama’da, toplayıcı VM için depolama hedefini belirleyin.
8. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
9. **Ağ Eşleme** bölümünde toplayıcı VM’nin bağlanacağı ağı belirtin. Meta verileri Azure’a göndermek için ağ, İnternet bağlantısına sahip olmalıdır.
10. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.

## <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola tercihlerini belirtin.
3. Masaüstünde **Toplayıcı çalıştır** kısayoluna tıklayın.
4. Toplayıcı kullanıcı arabiriminin üst çubuğundaki **Güncelleştirmeleri denetle**'ye tıklayın ve toplayıcının en son sürümde çalıştırıldığını doğrulayın. Aksi takdirde, bağlantıdan en son yükseltme paketini indirmeyi seçebilir ve toplayıcıyı güncelleştirebilirsiniz.
5. Azure Geçişi Toplayıcısı’nda **Önkoşulları ayarla** seçeneğini açın.
    - Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - Toplayıcı, VM’nin İnternet erişimine sahip olup olmadığını denetler.
    - VM, proxy üzerinden İnternet erişimine sahipse **Proxy ayarları**’na tıklayın ve proxy adresini ve dinleme bağlantı noktasını belirtin. Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin. İnternet bağlantısı gereksinimleri ve toplayıcının eriştiği URL'lerin listesi hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity).

    > [!NOTE]
    > Proxy adresinin, http://ProxyIPAddress veya http://ProxyFQDN biçiminde girilmesi gerekir. Yalnızca HTTP proxy’si desteklenir.

    - Toplayıcı, toplayıcı hizmetinin çalışıp çalışmadığını denetler. Hizmet, toplayıcı VM’ye varsayılan olarak yüklenir.
    - VMware PowerCLI’yı indirin ve yükleyin.

6. **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - vCenter sunucusunun adını (FQDN) veya IP adresini belirtin.
    - **Kullanıcı adı** ve **Parola** bölümünde, toplayıcının vCenter sunucusundaki VM’leri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin.
    - **Toplama kapsamı**’nda, VM bulma için bir kapsam seçin. Toplayıcı yalnızca belirtilen kapsam içindeki VM’leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam en fazla 1500 VM’yi içermelidir. Daha büyük bir ortamı nasıl bulabileceğiniz hakkında [daha fazla bilgi edinin](how-to-scale-assessment.md).

7. **Geçişi projesini belirtin** bölümünde portaldan kopyaladığınız Azure Geçişi proje kimliğini ve anahtarını belirtin. Bu bilgileri kopyalamadıysanız toplayıcı VM’den Azure portalını açın. Projenin **Genel Bakış** sayfasında **Makineleri Bul**’a tıklayın ve değerleri kopyalayın.  
8. **Toplama durumunu görüntüle** bölümünde bulma işlemini izleyin ve VM’lerden toplanan meta verilerin kapsam içinde olup olmadığını denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar. Azure Geçişi toplayıcı tarafından toplanan veriler hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected).

> [!NOTE]
> Toplayıcı, işletim sistemi dili ve toplayıcı arabirimi dili olarak yalnızca "İngilizce (ABD)"yi destekler.
> Değerlendirmek istediğiniz bir makinenin ayarlarını değiştirirseniz, değerlendirmeyi çalıştırmadan önce yeniden keşfetmeyi tetikleyin. Toplayıcıda, bunu yapmak için **Koleksiyonu yeniden başlat** seçeneğini kullanın. Koleksiyon tamamlandıktan sonra, güncelleştirilmiş değerlendirme sonuçlarını almak için portalda değerlendirmeye yönelik **Yeniden hesapla** seçeneğini belirleyin.



### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Bulma süresi, kaç VM bulduğunuza bağlıdır. 100 VM için, toplayıcı çalışmayı durdurduktan sonra bulma işleminin tamamlanması genellikle yaklaşık bir saat sürer.

1. Migration Planner projesinde **Yönet** > **Makineler**’e tıklayın.
2. Bulmak istediğiniz VM’lerin portalda görüntülenip görüntülenmediğini kontrol edin.


## <a name="create-and-view-an-assessment"></a>Değerlendirme oluşturma ve görüntüleme

VM’ler bulunduktan sonra bunları gruplandırın ve bir değerlendirme oluşturun.

1. Projenin **Genel Bakış** sayfasında **+Değerlendirme oluştur**’a tıklayın.
2. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.
3. Grubu oluşturun ve bir grup adı belirtin.
4. Gruba eklemek istediğiniz makineleri seçin.
5. Grubu ve değerlendirmeyi oluşturmak için **Değerlendirme Oluştur**’a tıklayın.
6. Değerlendirme oluşturulduktan sonra **Genel Bakış** > **Pano** bölümünde görüntüleyebilirsiniz.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.

### <a name="assessment-details"></a>Değerlendirme ayrıntıları

Değerlendirme, şirket içi sanal makinelerin Azure için uyumlu olup olmadığı, Azure’da sanal makineyi çalıştırmak için doğru sanal makine boyutunun ne olacağı ve tahmini aylık Azure maliyetleri hakkında bilgileri içerir.

![Değerlendirme raporu](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure için hazır olma

Değerlendirmedeki Azure için hazır olma görünümü, her bir sanal makinenin hazır olma durumunu gösterir. Sanal makinenin özelliklerine bağlı olarak her bir sanal makine şöyle işaretlenebilir:
- Azure için hazır
- Azure için koşullu olarak hazır
- Azure için hazır değil
- Hazır olma durumu bilinmiyor

Azure Geçişi, hazır olan VM’ler için Azure’da bir VM boyutu önerir. Azure Geçişi’nin yaptığı boyut önerisi, değerlendirme özelliklerinde belirtilen boyutlandırma ölçütüne bağlıdır. Boyutlandırma ölçütü performansa dayalı boyutlandırma ise, sanal makinelerin performans geçmişi (CPU ve bellek) ve diskleri (IOPS ve aktarım hızı) dikkate alınarak boyut önerisinde bulunulur. Boyutlandırma ölçütünün 'şirket içinde olduğu gibi' olması durumunda, Azure Geçişi VM'ler ve diskler için performans verilerini dikkate almaz. Azure'da VM boyutu önerisi, şirket içi VM'nin boyutuna bakılarak yapılır ve disk boyutlandırması da değerlendirme özelliklerinde belirtilen Depolama türü (varsayılan değer premium diskler) temelinde yapılır. Azure Geçişi’nde boyutlandırmanın nasıl yapıldığı hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md).

Azure için hazır olmayan veya koşullu olarak hazır olan sanal makineler için Azure Geçişi, hazır olma durumu sorunlarını açıklar ve düzeltme adımları sağlar.

Azure Geçişi’nin Azure için hazır olma durumunu belirleyemediği (verilerin kullanılabilir olmaması nedeniyle) sanal makineler, hazır olma durumu bilinmiyor olarak işaretlenir.

Azure Geçişi, Azure için hazır olma ve boyutlandırmaya ek olarak sanal makineyi geçirmek için kullanabileceğiniz araçları da önerir. Bunun için şirket içi ortamın daha kapsamlı bir keşfi gereklidir. Şirket içi makinelere aracılar yükleyerek nasıl daha kapsamlı bir keşif gerçekleştirebileceğiniz hakkında [daha fazla bilgi edinin](how-to-get-migration-tool.md). Şirket içi makinelere aracılar yüklenmemişse [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) kullanılarak lift and shift ile geçiş önerilir. Şirket içi makineye aracılar yüklenmişse Azure Geçişi, makinenin içinde çalışan işlemlere bakar ve makinenin bir veritabanı makinesi olup olmadığını belirler. Makine bir veritabanı makinesiyse geçiş aracı olarak [Azure Veritabanı Geçiş Hizmeti](https://docs.microsoft.com/azure/dms/dms-overview); makine bir veritabanı makinesi değilse geçiş aracı olarak Azure Site Recovery önerilir.

  ![Hazır olma durumu değerlendirmesi](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Aylık maliyet tahmini

Bu görünümde, Azure’da çalışan VM’lerin toplam işlem ve depolama maliyetinin yanı sıra her makineye ilişkin ayrıntılar da görüntülenir. Maliyet tahminleri, Azure Geçişi tarafından bir makine, bu makinenin diskleri ve değerlendirme özellikleri için gerçekleştirilen boyut önerileri göz önünde bulundurularak hesaplanır.

> [!NOTE]
> Azure Geçişi tarafından sağlanan maliyet tahmini, hizmet olarak Azure Altyapısı (IaaS) VM’leri biçiminde çalıştırılan şirket içi VM’lerine yöneliktir. Azure Geçişi, Hizmet olarak platform (PaaS) veya Hizmet olarak yazılım (SaaS) maliyetlerini hesaba katmaz.

İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir.

![VM maliyeti değerlendirmesi](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Güvenilirlik derecelendirmesi

Azure Geçişi’ndeki her değerlendirme 1 yıldız ile 5 yıldız (1 yıldız en düşük, 5 yıldız en yüksektir) arasında değişen bir güvenilirlik derecesiyle ilişkilendirilir. Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır. Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.

Bir değerlendirmenin güvenilirlik derecesi, boyutlandırma ölçütü 'performansa dayalı boyutlandırma' olan değerlendirmelerde daha kullanışlıdır. Performansa dayalı boyutlandırma için Azure Geçişi, CPU ile VM'nin belleği için kullanım verilerine ihtiyaç duyar. Bunlara ek olarak, VM'ye eklenen her disk için disk IOPS ve aktarım hızı değerleri de gerekir. Sanal makineye eklenmiş her bir ağ bağdaştırıcısı için benzer şekilde Azure Geçişi’nin performansa dayalı boyutlandırma gerçekleştirmek amacıyla ağ giriş/çıkışına ihtiyacı vardır. Yukarıdaki kullanım rakamlarından herhangi biri vCenter Server’da mevcut değilse Azure Geçişi’nin yaptığı boyut önerisi güvenilir olmayabilir. Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için aşağıdaki gibi güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

Aşağıdaki nedenlerle bir değerlendirme için tüm veri noktaları kullanılabilir olmayabilir:
- vCenter Server'daki istatistik ayarı 3 düzeyine ayarlanmamıştır. vCenter Server’daki istatistik ayarı 3 düzeyinden küçükse vCenter Server’dan disk ve ağ için performans verileri toplanmaz. Bu durumda, Azure Geçişi tarafından disk ve ağ için sağlanan öneri, kullanım tabanlı olmaz. Azure Geçişi, diskin IOPS/iş hacmi göz önünde bulundurulmadan diskin Azure’da premium bir disk gerektirip gerektirmediğini belirleyemeyeceğinden, bu örnekte Azure Geçişi tüm diskler için standart diskleri önerir.
- vCenter Server’daki istatistik ayarı, keşif başlatılmadan önce daha kısa süre bir için 3. düzey olarak ayarlanmıştır. Örneğin, bugün istatistik ayarı düzeyini 3 olarak değiştirdiğiniz, yarın ise (24 saat sonra) toplayıcı gerecini kullanarak keşfi başlattığınız bir senaryoyu ele alalım. Bir gün için değerlendirme oluşturuyorsanız tüm veri noktalarına sahip olursunuz ve değerlendirmenin güvenilirlik derecesi 5 yıldız olur. Ancak değerlendirme özelliklerinde performans süresini bir ay olarak değiştiriyorsanız, son bir ay için disk ve ağ performansı verileri kullanılabilir halde olmayacağından güvenilirlik derecesi düşer. Son bir ayın performans verilerini dikkate almak istiyorsanız, keşif başlatmadan önce bir ay boyunca vCenter Server istatistik ayarını 3 düzeyinde tutmanız önerilir.
- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine kapatılmıştır. Herhangi bir sanal makine belirli bir süre boyunca kapatıldıysa vCenter Server o süreye ait performans verilerine sahip olmaz.
- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu tür durumlarda yeni sanal makinelerin performans geçmişi, sürenin tamamı boyunca mevcut olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecesi 4 Yıldız’ın altında ise vCenter Server istatistik ayarları düzeyini 3 olarak değiştirmeniz, değerlendirme için göz önünde bulundurmak istediğiniz süre (1 gün/1 hafta/1 ay) boyunca bekleyip daha sonra keşif ve değerlendirme gerçekleştirmeniz önerilir. Bu yapılamazsa, performansa dayalı boyutlandırma güvenilir olmayabilir ve değerlendirme özellikleri değiştirilerek *şirket içi olarak boyutlandırmaya* geçiş yapılması önerilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bir değerlendirmeyi gereksinimleriniz temelinde nasıl özelleştirebileceğinizi [öğrenin](how-to-modify-assessment.md).
- [Makine bağımlılık eşlemesi](how-to-create-group-machine-dependencies.md) kullanarak yüksek güvenilirlikli değerlendirme grubu oluşturmayı öğrenin
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
- Geniş bir VMware ortamında bulma ve değerlendirme işlemlerinin nasıl yapılacağını [öğrenin](how-to-scale-assessment.md).
- Azure Geçişi’ne ilişkin SSS hakkında [daha fazla bilgi edinin](resources-faq.md)
