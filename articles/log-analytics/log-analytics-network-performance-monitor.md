---
title: Ağ Performansı İzleyicisi çözüm azure'da | Microsoft Docs
description: Ağ Performansı İzleyicisi azure'da algılamak ve ağ performans sorunlarını bulmak için neredeyse gerçek zamanlı, ağlarınız performansını izlemenize yardımcı olur.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: na
ms.openlocfilehash: 28f5ac882051250903fc2f45def95eb65d1b1ce3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38635547"
---
# <a name="network-performance-monitor-solution-in-azure"></a>Ağ Performansı İzleyicisi çözüm azure'da

![Ağ Performansı İzleyicisi simgesi](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Ağ Performansı İzleyicisi, izleme, ağ altyapınızın çeşitli noktalarında arası ağ performansını izlemenize yardımcı olan bir çözümü karma bulut tabanlı bir ağdır. Ayrıca, hizmet ve uygulama uç noktaları ve izleme ağ bağlantısının izlemenize yardımcı olur performansı Azure ExpressRoute. 

Ağ Performansı İzleyicisi trafiği blackholing, yönlendirme hataları ve sorunları geleneksel ağ izleme yöntemleri algılayabilir gibi ağ sorunları algılar. Çözüm, uyarılar oluşturur ve bir ağ bağlantısı için bir eşiği ihlal olduğunda size bildirir. Ayrıca, ağ performansı sorunlarını zamanında algılanması sağlar ve belirli ağ kesimine veya cihaza sorunun kaynağını yerelletirilmesi. 

Ağ Performansı İzleyicisi üç geniş özellikleri sunar: 

* [Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md): Bulut dağıtımları ve şirket içi konumlara arasında ağ bağlantısı izleyebilirsiniz birden çok veri merkezleri ve şube ofislerinde ve görev açısından kritik çok katmanlı uygulamalar veya mikro hizmetler. Kullanıcılar şikayet etmeden önce Performans İzleyicisi ile ağ sorunlarını algılayabilir.

* [Hizmet Bağlantı İzleyicisi](log-analytics-network-performance-monitor-service-endpoint.md): çok önem verdiğiniz, yolda hangi altyapıya belirlemek ve burada ağ performans sorunlarını ortaya tanımlamak hizmetlerini kullanıcılarınızdan gelen bağlantıları izleyebilir. Kullanıcılarınızın önce kesintilerden haberdar bilmeniz ve sorunların tam konumu, ağ yola bakın. 

    Bu özellik HTTP, HTTPS, TCP ve ICMP, neredeyse gerçek zamanlı veya geçmişe dönük olarak kullanılabilirlik ve yanıt süresi hizmetinizin izlemek için temel testleri gerçekleştirmenize yardımcı olur. Ağ paket kaybı ve gecikme katkısını da izleyebilirsiniz. Bir ağ topolojisi haritası ile ağ yavaşlamaları ayırabilirsiniz. Ağ yol boyunca her atlama gecikme verileriyle hizmetine düğümden oluşan sorun noktaları belirleyebilirsiniz. Yerleşik testler ile Office 365 ve Dynamics CRM tüm önceden yapılandırma olmadan ağ bağlantısı izleyebilirsiniz. Bu özellik sayesinde, Web siteleri, SaaS uygulamaları, PaaS uygulamalarının ve SQL veritabanları gibi tüm TCP özellikli uç noktası için ağ bağlantısı izleyebilirsiniz.

* [ExpressRoute İzleyicisi](log-analytics-network-performance-monitor-expressroute.md): uçtan uca bağlantıyı ve performansı arasında şubeleriniz ve Azure, Azure ExpressRoute üzerinden izleyin.  

Tarafından desteklenen çeşitli özellikler hakkında daha fazla bilgi [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) çevrimiçi kullanılabilir.
 
## <a name="supported-regions"></a>Desteklenen bölgeler
NPM aşağıdaki bölgelerden birinde bulunan bir çalışma alanından dünyanın herhangi bir parçası olan ve ağlar arasında bağlantı izleyebilirsiniz:
* Batı Avrupa
* Batı Orta ABD
* Doğu ABD
* Güneydoğu Asya
* Güney Doğu Avustralya
* UK Güney
* ABD Devleti Virginia

ExpressRoute İzleyicisi kullanılabilir için desteklenen bölgeler listesi [belgeleri](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117#regions).


## <a name="set-up-and-configure"></a>Ayarlama ve yapılandırma

### <a name="install-and-configure-agents"></a>Yükleme ve yapılandırma aracıları 

Temel işlemler, aracıları yüklemek için kullanmak [Azure Log analytics'e bağlanma Windows bilgisayarları](log-analytics-windows-agents.md) ve [Log Analytics için Operations Manager'ı bağlama](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Aracılarının nasıl yükleneceği 

* **Performans İzleyicisi**: en az bir düğümde yüklemek Operations Management Suite Aracısı bağlı diğer alt ağlara ağ bağlantısı izlemek istediğiniz her alt ağ.

    Bir ağ bağlantısı izlemek için bu bağlantının her iki bitiş noktasında aracılarını yükleyin. Ağınızın topolojisi hakkında emin değilseniz aracıları sunucuları arasındaki ağ performansını izlemek istediğiniz kritik iş yükleri ile yükleyin. Örneğin, bir web sunucusu ve SQL çalıştıran bir sunucu arasındaki ağ bağlantısını izlemek istiyorsanız, her iki sunucuda bir aracı yükleyin. Konak, konak kendileri arasında ağ bağlantısı (Bağlantılar) aracıları izleyin. 

* **Hizmet Bağlantı İzleyicisi**: ağ hizmet uç noktası bağlantısını izlemek istediğiniz her bir düğümde bir Operations Management Suite aracısını yükleyin. O1, O2 ve O3 office sitelerden Office 365'e ağ bağlantılarını izlemek istiyorsanız bir örnektir. En az bir düğüm her O1, O2 ve O3 Operations Management Suite aracısını yükleyin. 

* **ExpressRoute İzleyicisi**: Azure sanal ağınızda en az bir Operations Management Suite aracısını yükleyin. Ayrıca ExpressRoute özel eşlemesi aracılığıyla bağlanan, şirket içi alt ağ içinde en az bir aracıyı yükleyin.  

### <a name="configure-operations-management-suite-agents-for-monitoring"></a>Operations Management Suite Aracısı izleme için yapılandırma 

Ağ Performansı İzleyicisi yapay işlemler, kaynak ve hedef aracılar arası ağ performansını izlemek için kullanır. Performans İzleyicisi ve hizmet bağlantı İzleyicisi özelliklerinde izleme için protokol olarak TCP ve ICMP arasında seçim yapabilirsiniz. Yalnızca TCP ExpressRoute İzleyicisi için İzleyici protokolü olarak kullanılabilir. Güvenlik Duvarı'nın seçtiğiniz protokole ilişkin izlemek için kullanılan Operations Management Suite Aracısı arasındaki iletişim izin verdiğinden emin olun. 

* **TCP Protokolü**: izleme için protokol olarak TCP seçerseniz, aracıların birbirleriyle bağlanabildiğinden emin olmak için Ağ Performansı İzleyicisi'ni ve ExpressRoute izleme için kullanılan aracıları güvenlik duvarı bağlantı noktasını açın. Bağlantı noktası açmak için çalıştırın [EnableRules.ps1](https://aka.ms/npmpowershellscript) PowerShell betiğini yönetici ayrıcalıklarıyla bir PowerShell penceresinde herhangi bir parametre olmadan.

    Betik, çözüm için gerekli kayıt defteri anahtarları oluşturur. Ayrıca, aracıların birbirleriyle TCP bağlantıları oluşturmak izin vermek için Windows Güvenlik duvarı kuralları oluşturur. Betiği tarafından oluşturulan kayıt defteri anahtarlarını oturum hata ayıklama günlükleri ve günlük dosyasının yolunu belirtin. Betik Ayrıca iletişim için kullanılan Aracısı TCP bağlantı noktasını tanımlar. Bu anahtarları için değerleri otomatik olarak komut dosyası tarafından ayarlanır. Bu anahtarları el ile değiştirmeyin. Varsayılan olarak açık bağlantı noktası 8084 ' dir. Komut parametresi portNumber sağlayarak özel bir bağlantı noktası kullanabilirsiniz. Aynı bağlantı noktasını, tüm bilgisayarlarda betiğin çalıştırıldığı kullanın. 

    >[!NOTE]
    > Betik yalnızca Windows Güvenlik Duvarı yerel olarak yapılandırır. Bir ağ güvenlik duvarınız varsa, Ağ Performansı İzleyicisi tarafından kullanılan TCP bağlantı noktasının giden trafiğe izin verdiğinden emin olun.

    >[!NOTE]
    > Çalıştırmanız gerekmez [EnableRules.ps1](https://aka.ms/npmpowershellscript ) hizmet uç noktası İzleyicisi için PowerShell Betiği.

    

* **ICMP Protokolü**: izleme için protokol olarak ICMP'yi seçerseniz, ICMP güvenilir bir şekilde kullanmak aşağıdaki güvenlik duvarı kurallarını etkinleştirin:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>Çözümü yapılandırma 

1. Çalışma alanınızdan Ağ Performansı İzleyicisi çözüm ekleme [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). Açıklanan işlemi ayrıca kullanabileceğiniz [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](log-analytics-add-solutions.md). 
2. Log Analytics çalışma alanınızın açın ve seçin **genel bakış** Döşe. 
3. Seçin **Ağ Performansı İzleyicisi** döşeme iletinin *çözüm ek yapılandırma gerektirir*.

   ![Ağ Performansı İzleyicisi kutucuğu](media/log-analytics-network-performance-monitor/npm-config.png)

4. Üzerinde **Kurulum** sayfasında Operations Management Suite aracıları yüklemek ve izleme için aracıları yapılandırmak için bu seçeneği görürsünüz **ortak ayarları** görünümü. Operations Management Suite Aracısı yükleyip, daha önce anlatıldığı olarak, seçin **Kurulum** özelliğini yapılandırmak için kullanmak istediğiniz görünümü. 

   **Performans İzleyicisi**: Sentetik işlemler için kullanılacak protokolü seçin **varsayılan** Performans İzleyicisi Kuralı ' nı seçip **Kaydet ve devam et**. Bu protokol seçimi yalnızca sistem tarafından oluşturulan varsayılan kural bulunur. Performans İzleyicisi kuralı açıkça oluşturulan her durumda Protokolü seçmeniz gerekebilir. Her zaman taşıyabilirsiniz **varsayılan** kural ayarları **Performans İzleyicisi** sekme (göründüğü 0 gün yapılandırmanızı tamamladıktan sonra) ve daha sonra değişiklik protokolü. İzleyici özelliği rPerfomance istemiyorsanız varsayılan kuralından devre dışı bırakabilirsiniz **varsayılan** kural ayarları **Performans İzleyicisi** sekmesi.

   ![Performans izleme görünümü](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
   **Hizmet Bağlantı İzleyicisi**: Office 365 ve Dynamics 365, aracılardan gelen ağ bağlantılarını izlemek için yerleşik önceden yapılandırılmış testler yeteneği sağlar. Bunları yanındaki onay kutularını seçerek izlemek istediğiniz Office 365 ve Dynamics 365 hizmetleri seçin. İzlemek istediğiniz aracılarını seçmek için Seç **aracı Ekle**. Daha sonra ayarlamak için bu özelliği veya istediğiniz kullanmak istemiyorsanız, yoksa hiçbir şey seçip **Kaydet ve devam et**.

   ![Hizmet uç noktası İzleyicisi görüntüle](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

   **ExpressRoute İzleyicisi**: seçin **Şimdi Bul** Azure aboneliğindeki sanal ağlara bağlı özel eşlemeler bu Log Analytics çalışma alanıyla bağlantılı tüm ExpressRoute bulunacak. 

   >[!NOTE] 
   > Çözüm, şu anda yalnızca ExpressRoute özel eşlemesi bulur. 

   >[!NOTE] 
   > Yalnızca bu Log Analytics çalışma alanıyla bağlantılı aboneliği ile ilişkili sanal ağlara bağlı özel eşlemeler bulunur. ExpressRoute, bu çalışma alanına bağlı abonelik dışında sanal ağlara bağlıysa, bu Aboneliklerde bir Log Analytics çalışma alanı oluşturun. Bu eşlemeler izlemek için Ağ Performansı İzleyicisi'ni kullanın.

   ![ExpressRoute İzleyicisi görüntüle](media/log-analytics-network-performance-monitor/npm-express-route.png)

   Bulma tamamlandıktan sonra bulunan özel eşlemeler bir tabloda listelenmiştir. 

   ![Ağ Performansı İzleyicisi Yapılandırması sayfası](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
Bu eşlemeler için izleme, başlangıçta devre dışı durumda olan. İzlemek istediğiniz her eşleme seçin ve sağ taraftaki Ayrıntıları görünümünde izlemesini bunlar için yapılandırın. Seçin **Kaydet** yapılandırmayı kaydetmek için. Daha fazla bilgi için "Yapılandırma ExpressRoute izleme" makalesine bakın. 

Kurulum tamamlandıktan sonra veriler doldurmak için bir saat 30 dakika sürer. Çözüm, ağ üzerinden verileri toplar, ancak iletiyi görmek *çözüm ek yapılandırma gerektirir* Ağ Performansı İzleyicisi **genel bakış** kutucuk. Sonra veriler toplanır ve dizinli **genel bakış** kutucuk değişir ve bir Özet olarak, ağ durumunu size bildirir. Ardından, hangi Operations Management Suite aracısı yüklü olan düğümlerin yanı sıra ortamınızdaki keşfedilen alt ağlar izlenmesini düzenleyebilirsiniz.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Alt ağları ve düğümler için izleme ayarlarını Düzenle 

En az bir aracısının yüklü olduğu tüm alt ağlar listelenir **alt ağlar** yapılandırma sayfasında sekmesi. 


Etkinleştirmek veya belirli alt ağlar izleme devre dışı bırakmak için:

1. Seçin veya yanındaki onay kutusunu temizleyin **alt ağ kimliği**. Ardından emin olun **kullanımı için izleme** olduğundan seçilen veya temizlenen, uygun şekilde. Seçin veya birden çok alt ağa temizleyin. Alt ağlar devre dışı bırakıldığında, izlenen olmayan ve diğer aracıları ping işlemi durdurmak için aracıları güncelleştirilir. 
2. Belirli bir alt ağ içinde izlemek istediğiniz düğümleri seçin. Listeden bir alt ağ seçin ve gerekli düğümleri izlenmeyen ve izlenen düğümleri içeren listeleri arasında taşıyın. Alt ağ için bir özel açıklama ekleyebilirsiniz.
3. Seçin **Kaydet** yapılandırmayı kaydetmek için. 

#### <a name="choose-nodes-to-monitor"></a>İzlemek için düğümleri seçin

Bunlara bir aracı yüklü tüm düğümleri listelenir **düğümleri** sekmesi. 

1. Seçin veya izlemek veya izlemeyi durdurmak istediğiniz düğümleri temizleyin. 
2. Seçin **kullanımı için izleme**, veya, uygun şekilde temizleyin. 
3. **Kaydet**’i seçin. 


İstediğiniz özellikleri yapılandırın:

- [Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute İzleyicisi](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Veri koleksiyonu ayrıntıları
Kayıp ve gecikme bilgilerini toplamak için protokol olarak TCP seçtiğinizde TCP SYN SYNACK ACK el sıkışması paketleri Ağ Performansı İzleyicisi'ni kullanır. Protokol olarak ICMP'yi seçtiğinizde Ağ Performansı İzleyicisi ICMP YANKI ICMP YANKI YANITI kullanır. İzleme yönlendirmesi, topoloji bilgilerini almak için de kullanılır.

Veri toplama yöntemleri ve Ağ Performansı İzleyicisi için verileri nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Doğrudan aracı | System Center Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager aracısı veri yönetim grubu gönderilir. | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |TCP el sıkışmaları/ICMP YANKI iletilerinin 5 saniyede veri 3 dakikada gönderilen |
 

 
Çözüm yapay işlemler, ağ durumunu değerlendirmek için kullanır. Operations Management Suite aracıları, ağ exchange TCP veya ICMP Yankı çeşitli noktalarında birbiriyle yüklü. Aracıları TCP veya ICMP Yankı kullanıp izlemek için seçtiğiniz protokolde bağlıdır. İşlem sırasında aracılar varsa gidiş dönüş süresi ve paket kaybı öğrenin. Düzenli olarak, her bir aracı ayrıca diğer aracılara test edilmelidir ağdaki tüm çeşitli yollarını bulmak için bir izleme yolu yapar. Bu verileri kullanarak, aracıları ağ gecikme süresi ve paket kaybı rakamları çıkarabilir. Testler, her beş saniyede bir yinelenir. Log Analytics hizmetine ıntune'ye yüklenmeden önce veriler yaklaşık üç dakika Aracılar tarafından toplanır.



>[!NOTE]
> Aracılar birbiriyle sıklıkla iletişim kurduğu olsa da, testleri yürütürken önemli ölçüde ağ trafiği üretme. Aracıları kayıp ve gecikme süresi belirlemek için yalnızca TCP SYN SYNACK ACK el sıkışması paketlerde kullanır. Hiçbir veri paketi değiştirilir. Bu işlem sırasında aracılar birbiriyle yalnızca gerektiğinde iletişim kurar. Aracı iletişim topolojisinin ağ trafiğini azaltmak için optimize edilmiştir.

## <a name="use-the-solution"></a>Çözüm kullanın 

### <a name="network-performance-monitor-overview-tile"></a>Ağ Performansı İzleyicisi'ne genel bakış kutucuğu 

Ağ Performansı İzleyicisi çözümü etkinleştirdikten sonra çözüm kutucuğunu **genel bakış** sayfası, ağ durumu hızlı bir genel bakış sağlar. 

 ![Ağ Performansı İzleyicisi'ne genel bakış kutucuğu](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Ağ performansı izleme Panosu 

* **İlk ağ durumu olayları**: olayları etkin olan bu yana bu sayfayı bir listesini en son sistem durumu olayları ve Uyarıları sistem ve zaman sağlar. İzleme kuralı için seçilen ölçüm (kaybı, gecikme süresi, yanıt süresi veya bant genişliği kullanımı) değeri eşiği aştığında bir sistem durumu olayı veya uyarısı oluşturulur. 

* **ExpressRoute İzleyicisi**: Bu sayfa için çeşitli ExpressRoute eşleme bağlantıları sistem durumu özetlerini, çözümü sağlar. izleyiciler. **Topolojisi** kutucuk ağınızda ağ yollarını izlenen ExpressRoute bağlantı hatları aracılığıyla sayısını gösterir. Gitmek için bu kutucuğu seçin **topolojisi** görünümü.

* **Hizmet Bağlantı İzleyicisi**: Bu sayfa, oluşturduğunuz farklı testler için durum özetleri sağlar. **Topolojisi** kutucuğu izlenen uç noktaların sayısını gösterir. Gitmek için bu kutucuğu seçin **topolojisi** görünümü.

* **Performans İzleyicisi**: Bu sayfa için sistem durumu özetlerini sağlar **ağ** bağlantıları ve **alt ağ** çözüm izleyen bağlantılar. **Topolojisi** kutucuğu, ağınızdaki izlenen ağ yollarının sayısını gösterir. Gitmek için bu kutucuğu seçin **topolojisi** görünümü. 

* **Genel sorgular**: Bu sayfa ham ağ izleme verileri doğrudan fetch arama sorguları bir dizi içerir. Bu sorgular, özelleştirilmiş raporlama için kendi sorgularınızı oluşturmak için başlangıç noktası olarak kullanabilirsiniz. 

   ![Ağ performansı izleme Panosu](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>Derinlik için detaya gitme 

Çözüm panosunda detaya gitmek için çeşitli bağlantılar seçebilirsiniz ilgilendiğiniz herhangi bir alana derin aşağı. Örneğin, bir uyarı veya Panoda görünmesini bir sağlıksız ağ bağlantısı gördüğünüzde, daha fazla araştırmak için seçin. Bir sayfa belirli ağ bağlantı için tüm alt ağ bağlantıları listeler. Kaybı, gecikme süresi ve her alt ağ bağlantısı sistem durumunu görebilirsiniz. Hangi alt ağ bağlantısı sorunlarına neden oluyor kullanıma hızlıca bulabilirsiniz. Seçin **düğüm bağlantılarını görüntüle** sağlıksız alt ağ bağlantısı için tüm düğüm bağlantıları görmek için. Ardından, tek tek düğümler için bağlantılara bakın ve sağlıksız düğüm bağlantıları bulun. 

Seçin **topolojisini görüntüleme** kaynak ve hedef düğümleri arasındaki tüm yolların atlama atlama düzeyinde topolojisini görüntülemek için. Sağlıksız yollar kırmızı renkte görünür. Belirli bir ağ kısmını sorunu hızlıca tanımlayabilirsiniz böylece atlamalar tarafından katkıda bulunulan gecikme süresini görüntüleyebilirsiniz.

 

### <a name="network-state-recorder-control"></a>Ağ durumu Kaydedici denetimi

Her görünüm, zaman içinde belirli bir noktada, ağ durumu anlık görüntüsünü görüntüler. Varsayılan olarak, en son durumu gösterilir. Sayfanın en üstündeki çubuğunda noktası durumu görüntülendiği süresini gösterir. Bir önceki bir anlık görüntü, ağ durumunu görüntülemek için seçin **eylemleri**. Ayrıca etkinleştirebilir veya son durumunu görüntülerken herhangi bir sayfa için Otomatik yenilemeyi devre dışı bırakın. 

 ![Ağ durumu Kaydedici](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Eğilim grafikleri 

Detaya her düzeyinde uygulanabilir ölçüm eğilimi görebilirsiniz. Kaybı, gecikme süresi, yanıt süresi veya bant genişliği kullanımı olabilir. Eğilim için zaman aralığını değiştirmek için grafiğin üstünde zamanı denetimi kullanın. 

Eğilim grafikleri, geçmiş perspektif performans ölçümü performansını gösterir. Bazı ağ sorunları, doğası gereği geçici ve yalnızca geçerli durumda ağ bakarak catch hale getirilmesi zordur. Sorunları, hızlı bir şekilde yüzeyine ve herkesin, yalnızca zaman sonraki bir noktada yeniden fark etmeden kaybolur. Bu geçici bir sorun da uygulama yöneticileri için zor olabilir. Sorunlar genellikle uygulama yanıt süresi, beklenmeyen bir artış olarak görünmesi bile, tüm uygulama bileşenleri sorunsuz çalışır gibigörünür. 

Bu tür sorunları bir eğilim Grafiği bakarak kolayca algılayabilir. Sorun, ağ gecikmesi veya paket kaybı ani bir depo görünür. Sorunu araştırmaya sorunun oluştuğu zaman ağ anlık görüntü ve o noktadan topolojisini görüntülemek için ağ durumu Kaydedici denetimi kullanın.

 
![Eğilim grafikleri](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topoloji Haritası 

Ağ Performansı İzleyicisi, kaynak ve hedef uç noktada bir etkileşimli bir topoloji Haritası arasında rotaları atlama atlama düzeyinde topolojisini gösterir. Bir topoloji Haritası görüntülemek için seçin **topolojisi** çözüm Panoda kutucuk. Belirleyebilirsiniz **topolojisini görüntüleme** detaya gitme sayfalarında bağlantı. 

Kaynak ve hedef arasında ne kaç yollar, bir topoloji Haritası görüntüler yolları veri paketlerinin yararlanın. Her ağ atlama tarafından katkıda bulunulan gecikme süresini de görülebilir. Toplam yol gecikme süresi (karşılık gelen izleme kuralında ayarlanır) eşiğin üstünde olduğu tüm yolları kırmızı renkte gösterilir. 

Bir düğüm veya üzerine gelindiğinde kullanılacak üzerine topolojisi haritada seçtiğiniz FQDN ve IP adresi gibi düğüm özelliklerini görürsünüz. Bir atlama IP adresini görmek için seçin. Sorunlu ağ atlama katkıda bulunduğu gecikme tercihinize göre belirleyebilirsiniz. Belirli yollara filtre uygulamak için daraltılabilir Eylem Bölmesi'nde filtreleri kullanın. Ağ topolojileri basitleştirmek için eylem bölmesinde kaydırıcıyı kullanarak ara atlama gizleyin. Yakınlaştırma veya fare tekerleğini kullanarak topoloji Haritası dışında yakınlaştırma. 

Haritada gösterilen topolojisi Katman 3 topolojisi ve Katman 2 cihazları ve bağlantıları içermiyor. 

 
![Topoloji Haritası](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Log Analytics arama 

Ağ Performansı İzleyicisi Panosu aracılığıyla kullanıma sunulan grafik ve detaya gitme sayfaları tüm verileri yerel olarak kullanılabilir ayrıca [Log Analytics arama](log-analytics-log-search-new.md). Depoda etkileşimli veri analizi gerçekleştirmek ve farklı kaynaklardaki verileri ilişkilendirin. Ayrıca özel uyarılar ve görünümler oluşturma ve verileri, Excel, Power BI veya paylaşılabilir bağlantı verin.  **Ortak sorgular** panosunda yer alan kendi sorgular ve raporlar oluşturmak için başlangıç noktası olarak kullanabileceğiniz bazı yararlı sorgular sahiptir. 

## <a name="alerts"></a>Uyarılar

Ağ Performansı İzleyicisi uyarı yeteneklerini kullanır [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts).

Tüm uyarı kullanılarak yönetilir, yani [Eylem grupları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#overview).  

OMS aracılığıyla bir uyarı oluşturan bir NPM kullanıcı varsa: 
1. Azure Portalı'na yönlendireceği bir bağlantı görürsünüz. Portala erişmek için tıklayın.
2. Ağ Performansı İzleyicisi çözüm kutucuğa tıklayın. 
3. Gezinme yapılandırmak için.  
4. Bir uyarı oluşturmak ve izleyin testi seçin adımları aşağıda belirtilen.

Azure Portalı aracılığıyla bir uyarı oluşturan bir NPM kullanıcı varsa:  
1. Doğrudan e-postanızı girin seçebilirsiniz veya Eylem grupları aracılığıyla uyarılar oluşturmak tercih edebilirsiniz.
2. Doğrudan e-postanızı girin seçerseniz, bir eylem grubu adı ile **Npm e-posta** oluşturulur ve e-posta kimliği bu eylem grubuna eklenir.
3. Eylem grupları kullanmayı seçerseniz, daha önce oluşturulan eylem grubu seçmeniz gerekecektir. Bir eylem grubu oluşturmak nasıl öğrenebilirsiniz [burada.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#create-an-action-group-by-using-the-azure-portal) 
4. Uyarı başarıyla oluşturulduktan sonra Uyarıları Yönet bağlantısına Uyarıları yönetmek için kullanabilirsiniz. 

##<a name="pricing"></a>Fiyatlandırma

Fiyatlandırma hakkında daha fazla bilgi edinilebilir [çevrimiçi](log-analytics-network-performance-monitor-pricing-faq.md).

## <a name="provide-feedback"></a>Geri bildirimde bulunma 

* **UserVoice:** fikirlerinizi bize üzerinde çalışmak istediğiniz ağ performansı İzleyicisi özellikleri için gönderebilir. Ziyaret [UserVoice sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

* **Bizim kohort katılın:** her zaman yeni müşterilere sunduğumuz kohort katılın etmeyle ilgilenen duyuyoruz. Bunu bir parçası olarak yeni özellikler ve Ağ Performansı İzleyicisi'ni geliştirmemize yardımcı olmak için bir fırsat erken erişim elde edersiniz. Katılımının ilgileniyorsanız, bu doldurun [kısa bir ankete](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Sonraki adımlar 
Daha fazla bilgi edinin [Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md), [Hizmet Bağlantı İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md), ve [ExpressRoute İzleyicisi](log-analytics-network-performance-monitor-expressroute.md). 
