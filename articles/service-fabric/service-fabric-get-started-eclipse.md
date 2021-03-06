---
title: Eclipse için Azure Service Fabric eklentisi | Microsoft Docs
description: Eclipse için Service Fabric eklentisini kullanmaya başlayın.
services: service-fabric
documentationcenter: java
author: rapatchi
manager: timlt
editor: ''
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/06/2018
ms.author: rapatchi
ms.openlocfilehash: 2fbae584c09fd83f2233895d31c1013acd06ae3b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643007"
---
# <a name="service-fabric-plug-in-for-eclipse-java-application-development"></a>Eclipse Java uygulama geliştirmesi için Service Fabric eklentisi
Eclipse, Java geliştiricileri için en yaygın kullanılan tümleşik geliştirme ortamlarından (IDE’ler) biridir. Bu makalede, Azure Service Fabric ile çalışmak için Eclipse geliştirme ortamınızı ayarlama işlemi ele alınmaktadır. Service Fabric eklentisini yükleme, Service fabric uygulaması oluşturma ve Service Fabric uygulamanızı Eclipse’teki yerel veya uzak bir Service Fabric kümesine dağıtma hakkında bilgi edinin. 

> [!NOTE]
> Eclipse eklentisi Windows üzerinde şu anda desteklenmemektedir. 

## <a name="install-or-update-the-service-fabric-plug-in-in-eclipse"></a>Eclipse’te Service Fabric eklentisi yükleme veya güncelleştirme
Eclipse'te Service Fabric eklentisi yükleyebilirsiniz. Eklenti, Java hizmetleri oluşturup dağıtma işlemini kolaylaştırmaya yardımcı olur.

> [!IMPORTANT]
> Service Fabric eklentisi için Eclipse Neon veya üzeri bir sürüm gerekir. Eclipse sürümünüzün nasıl denetleneceği hakkında bu nottan sonraki yönergelere bakın. Eclipse’in önceki bir sürümünü yüklediyseniz, [Eclipse sitesinden](https://www.eclipse.org) daha yeni sürümleri indirebilirsiniz. Mevcut bir Eclipse yüklemesinin üzerine yüklemeniz (üzerine yazmanız) önerilmez. Yükleyiciyi çalıştırmadan önce kaldırabilir veya yeni sürümü farklı bir dizine yükleyebilirsiniz. 
> 
> Ubuntu üzerinde, paket yükleyici (`apt` veya `apt-get`) kullanmak yerine doğrudan Eclipse sitesinden yükleme yapılmasını öneririz. Böylece, Eclipse’in en güncel sürümünü elde etmeniz sağlanır. 

[Eclipse sitesinden](https://www.eclipse.org) Eclipse Neon veya sonraki bir sürümü yükleyin.  Ayrıca Buildship 2.2.1 veya sonraki bir sürümü de yükleyin (Service Fabric eklentisi, eski Buildship sürümleriyle uyumlu değildir):
-   Yüklü bileşenlerin sürümlerini denetlemek için Eclipse’te **Yardım** > **Eclipse Hakkında** > **Yükleme Ayrıntıları** seçeneğine gidin.
-   Buildship’i güncelleştirmek için bkz. [Eclipse Buildship: Gradle için Eclipse Eklentileri][buildship-update].
-   Eclipse güncelleştirmelerini denetleyip yüklemek için **Yardım** > **Güncelleştirmeleri Denetle** seçeneğine gidin.

Service Fabric eklentisini yüklemek için **Yardım** > **Yeni Yazılım Yükle** seçeneğine gidin.
1. **Birlikte çalış** kutusuna **http://dl.microsoft.com/eclipse** girin.
2. **Ekle**'ye tıklayın.
    ![Eclipse için Service Fabric eklentisi][sf-eclipse-plugin-install]
3. Service Fabric eklentisini seçip **İleri**’ye tıklayın.
4. Yükleme adımlarını tamamlayın ve ardından Microsoft Yazılım Lisans Koşulları’nı kabul edin.
  
Service Fabric eklentisi zaten yüklüyse, en yeni sürümü yükleyin. 
1. Kullanılabilir güncelleştirmeleri denetlemek için **Yardım** > **Eclipse Hakkında** > **Yükleme Ayrıntıları** seçeneğine gidin. 
2. Yüklü eklentiler listesinde Service Fabric’i seçip **Güncelleştir**’e tıklayın. Kullanılabilir güncelleştirmeler yüklenir.
3. Service Fabric eklentisini güncelleştirdikten sonra Gradle projesini de yenileyin.  **build.gradle** öğesine sağ tıklayın ve **Yenile**’yi seçin.

> [!NOTE]
> Service Fabric eklentisi yavaş yükleniyor veya güncelleştiriliyorsa, bunun nedeni bir Eclipse ayarı olabilir. Eclipse, Eclipse örneğinize kaydedilmiş siteleri güncelleştirmek üzere tüm değişikliklere ait meta verileri toplar. Bir Service Fabric eklenti güncelleştirmesini denetleme ve yükleme işlemini hızlandırmak için **Kullanılabilir Yazılım Siteleri** bölümüne gidin. Service Fabric eklenti konumunu (http://dl.microsoft.com/eclipse/azure/servicefabric)) işaret eden site dışındaki tüm sitelerin onay kutularının işaretini kaldırın.

> [!NOTE]
>Eclipse Mac bilgisayarınızda beklendiği gibi çalışmıyorsa (veya süper kullanıcı olarak çalışmanızı gerektiriyorsa), **ECLIPSE_INSTALLATION_PATH** klasörüne ve ardından **Eclipse.app/Contents/MacOS** alt klasörüne gidin. `./eclipse` öğesini çalıştırarak Eclipse’i başlatın.


## <a name="create-a-service-fabric-application-in-eclipse"></a>Eclipse’te Service Fabric uygulaması oluşturma

1.  Eclipse’te **Dosya** > **Yeni** > **Diğer** seçeneğine gidin. **Service Fabric Projesi**’ni seçip **İleri**’ye tıklayın.

    ![Service Fabric Yeni Proje sayfa 1][create-application/p1]

2.  Projeniz için bir ad girip **İleri**’ye tıklayın.

    ![Service Fabric Yeni Proje sayfa 2][create-application/p2]

3.  Şablonlar listesinde **Hizmet Şablonu**’nu seçin. Hizmet şablonu türünüzü (Aktör, Durum Bilgisi Olmayan, Kapsayıcı veya Konuk İkili) seçip **İleri**’ye tıklayın.

    ![Service Fabric Yeni Proje sayfa 3][create-application/p3]

4.  Hizmet adını ve hizmet ayrıntılarını girip **Son**’a tıklayın.

    ![Service Fabric Yeni Proje sayfa 4][create-application/p4]

5. İlk Service Fabric projenizi oluşturduktan sonra, **İlişkili Perspektifi Aç** iletişim kutusunda **Evet**’e tıklayın.

    ![Service Fabric Yeni Proje sayfa 5][create-application/p5]

6.  Yeni projeniz şöyle görünür:

    ![Service Fabric Yeni Proje sayfa 6][create-application/p6]

## <a name="build-and-deploy-a-service-fabric-application-in-eclipse"></a>Eclips’te Service Fabric uygulaması derleme ve dağıtma

1.  Yeni Service Fabric uygulamanıza sağ tıklayın ve ardından **Service Fabric**’i seçin.

    ![Service Fabric sağ tıklama menüsü][publish/RightClick]

2. Alt menüde istediğiniz seçeneği belirleyin:
    -   Uygulamayı temizlemeden derlemek için **Uygulamayı Derle**’ye tıklayın.
    -   Uygulamanın temiz bir derlemesini gerçekleştirmek için **Uygulamayı Yeniden Derle**’ye tıklayın.
    -   Derlenen yapıtları uygulamadan temizlemek için **Uygulamayı Temizle**’ye tıklayın.

3.  Bu menüden ayrıca uygulamanızı dağıtabilir, dağıtımını kaldırabilir ve yayımlayabilirsiniz:
    -   Yerel kümenize dağıtmak için **Uygulamayı Dağıt**’a tıklayın.
    -   **Uygulamayı Yayımla** iletişim kutusunda bir yayımlama profili seçin:
        -  **Local.json**
        -  **Cloud.json**

     Bu JavaScript Nesne Gösterimi (JSON) dosyaları, yerel veya bulut (Azure) kümenize bağlanmak için gereken bilgileri (bağlantı uç noktaları ve güvenlik bilgileri gibi) depolar.

  ![Service Fabric Yayımla menüsü][publish/Publish]

Service Fabric uygulamanızı dağıtmanın alternatif bir yolu, Eclipse çalıştırma yapılandırmalarının kullanılmasıdır.

  1.    **Çalıştır** > **Çalıştırma Yapılandırmaları** öğesine gidin.
  2.    **Grade Projesi** altındaki **ServiceFabricDeployer** çalıştırma yapılandırmasını seçin.
  3.    Sağ bölmedeki **Bağımsız Değişkenler** sekmesinde **publishProfile** için **yerel** veya **bulut** seçeneğini belirtin.  Varsayılan seçenek **yerel**’dir. Uzak kümeye veya bulut kümesine dağıtmak için **bulut** seçeneğini belirleyin.
  4.    Yayımlama profillerine doğru bilgilerin doldurulduğundan emin olmak için **Local.json** veya **Cloud.json** dosyasını gereken şekilde düzenleyin. Uç nokta bilgilerini ve güvenlik kimlik bilgilerini ekleyebilir ya da güncelleştirebilirsiniz.
  5.    **Çalışma Dizini** öğesinin, dağıtmak istediğiniz uygulamayı işaret ettiğinden emin olun. Uygulamayı değiştirmek için, **Çalışma Alanı** düğmesine tıklayın ve istediğiniz uygulamayı seçin.
  6.    **Uygula** ve ardından **Çalıştır** seçeneğine tıklayın.

Uygulamanız birkaç dakika içinde derlenip dağıtılır. Dağıtım durumunu Service Fabric Explorer’dan izleyebilirsiniz.  

## <a name="add-a-service-fabric-service-to-your-service-fabric-application"></a>Service Fabric uygulamanıza Service Fabric hizmeti ekleme

Var olan bir Service Fabric uygulamasına Service Fabric hizmeti eklemek için aşağıdaki adımları uygulayın:

1.  Hizmet eklemek istediğiniz projeye sağ tıklayın ve ardından **Service Fabric**’e tıklayın.

    ![Service Fabric Hizmet Ekleme sayfa 1][add-service/p1]

2.  **Service Fabric Hizmeti Ekle**’ye tıklayın ve projeye bir hizmet eklemek için adımları tamamlayın.
3.  Projenize eklemek istediğiniz hizmet şablonunu seçip **İleri**’ye tıklayın.

    ![Service Fabric Hizmet Ekleme sayfa 2][add-service/p2]

4.  Hizmet adını (ve gereken diğer ayrıntıları) girip **Hizmet Ekle** düğmesine tıklayın.  

    ![Service Fabric Hizmet Ekleme sayfa 3][add-service/p3]

5.  Hizmet eklendikten sonra projenizin genel yapısı aşağıdaki projeye benzer:

    ![Service Fabric Hizmet Ekleme sayfa 4][add-service/p4]

## <a name="edit-manifest-versions-of-your-service-fabric-java-application"></a>Service Fabric Java uygulamanızın Bildirim sürümlerini düzenleme

Bildirim sürümlerini düzenlemek için projeye sağ tıklayın, **Service Fabric**'e gidin ve açılan menüden **Bildirim Sürümlerini Düzenle...** seçeneğini belirleyin. Sihirbazda uygulama bildiriminin ve hizmet bildiriminin yanı sıra **Kod**, **Yapılandırma** ve **Veriler** adlı paketlere ilişkin sürümlere yönelik bildirim sürümlerini güncelleştirebilirsiniz.

**Uygulama ve hizmet sürümlerini otomatik olarak güncelleştir** seçeneğini işaretler ve ardından bir sürümü güncelleştirirseniz bildirim sürümleri de otomatik olarak güncelleştirilir. Örneğin, onay kutusunu işaretleyip **Kod** sürümünü 0.0.0'dan 0.0.1'e güncelleştirir ve ardından **Son**'a tıklarsanız hizmet bildirim sürümü ve uygulama bildirim sürümü otomatik olarak 0.0.1'e güncelleştirilir.

## <a name="upgrade-your-service-fabric-java-application"></a>Service Fabric Java uygulamanızı yükseltme

Bir yükseltme senaryosu için, Eclips’te Service Fabric eklentisini kullanarak **App1** projesini oluşturduğunuzu varsayalım. **fabric:/App1Application** adlı bir uygulama oluşturmak için eklentiyi kullanarak projeyi dağıttınız. Uygulama türü **App1AppicationType**, uygulama sürümü ise 1.0’dır. Şimdi de uygulamanızı kesinti olmadan yükseltmek istiyorsunuz.

İlk olarak, uygulamanızda değişiklikleri yapın ve değiştirilen hizmeti yeniden derleyin. Değiştirilen hizmetin bildirim dosyasını (ServiceManifest.xml) hizmetin güncel sürümleriyle (ve uygun olan Kod, Yapılandırma ya da Veriler ile) güncelleştirin. Uygulamanın bildirim dosyasını (ApplicationManifest.xml), uygulamanın ve değiştirilen hizmetin güncel sürüm numarasıyla da değiştirin.  

Eclipse kullanarak uygulamanızı yükseltmek için, yinelenen bir çalıştırma yapılandırma profili oluşturabilirsiniz. Ardından, uygulamanızı gereken şekilde yükseltmek için bu profili kullanın.

1.  **Çalıştır** > **Çalıştırma Yapılandırmaları** öğesine gidin. Sol bölmede, **Grade Projesi** öğesinin sol tarafında bulunan küçük oka tıklayın.
2.  **ServiceFabricDeployer**’a sağ tıklayıp **Yinelenen**’i seçin. Bu yapılandırma için **ServiceFabricUpgrader** gibi yeni bir ad girin.
3.  Sağ paneldeki **Bağımsız Değişkenler** sekmesinde **-Pconfig='deploy'** değerini **-Pconfig='upgrade'** olarak değiştirip **Uygula**’ya tıklayın.

Bu işlem, uygulamanızı yükseltmek için dilediğiniz zaman kullanabileceğiniz bir çalıştırma yapılandırma profili oluşturup kaydeder. Ayrıca, en son güncelleştirilmiş uygulama türü sürümünü uygulama bildirim dosyasından alır.

Uygulama yükseltmesi birkaç dakika sürer. Uygulama yükseltme işlemini Service Fabric Explorer’dan izleyebilirsiniz.

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a>Eski Service Fabric Java uygulamalarını Maven ile kullanılmak üzere geçirme
Service Fabric Java kitaplıklarını yakın zamanda Service Fabric Java SDK’sından Maven deposuna taşıdık. Eclipse kullanarak oluşturduğunuz yeni uygulamalar en son güncelleştirilen projeleri oluşturur (Maven ile çalışırlar), ancak daha önce Service Fabric Java SDK’sı kullanan mevcut Service Fabric durum bilgisi olmayan ya da aktör Java uygulamalarını Maven’ın Service Fabric Java bağımlılıklarını kullanacak şekilde güncelleştirebilirsiniz. Eski uygulamanızın Maven ile çalıştığından emin olmak için lütfen [burada](service-fabric-migrate-old-javaapp-to-use-maven.md) belirtilen adımları izleyin.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-eclipse/service-fabric-eclipse-plugin.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
