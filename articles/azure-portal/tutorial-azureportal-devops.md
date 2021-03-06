---
title: 'Öğretici: Azure portal ile DevOps | Microsoft Docs'
description: Azure Portal'daki çeşitli DevOps iş akışları hakkında bilgi edinin.
services: azure-portal
documentationcenter: ''
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: 2764d8a17d50f1589d413c2343badfded6adb66e
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216351"
---
# <a name="tutorial-devops-with-the-azure-portal"></a>Öğretici: Azure portal ile DevOps
Azure platformu esnek DevOps iş akışları ile doludur. Bu öğreticide, geliştirme, test, dağıtma, sorun giderme, izleme ve çalışan uygulamaları yönetmek için Azure portalında özelliklerini kullanmayı öğrenin. Bu öğretici aşağıdakilere odaklanır:

1. Bir web uygulaması oluşturma ve sürekli dağıtımı etkinleştirme
2. Uygulama geliştirme ve test etme
3. Bir uygulamayı izleme ve sorunlarını giderme
4. Genel uygulama yönetimi görevleri

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a>Bir web uygulaması oluşturma ve sürekli dağıtımı etkinleştirme
[Azure Uygulama Hizmeti](https://azure.microsoft.com/services/app-service/) ile bu öğreticinin geri kalanında kullanacağınız bir web uygulaması oluşturun. Başlangıçta sürekli dağıtım kaynak kod deponuzdan çalışan Azure ortamınıza etkinleştirmeniz.

1. Azure portalında oturum açın
2. **Uygulama Hizmetleri** &gt; **Simge ekle** seçeneğini belirleyin ve bir ad girin, aboneliğinizi seçin ve hizmetin kapsayıcısı olarak görev yapacak yeni bir kaynak grubu oluşturun.
   
   Kaynak grupları, çözümün faturalama, dağıtım ve tek bir grup olarak tüm izleme gibi çeşitli yönlerini yönetmek izin [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
   
   ![image1][image1]
3. Birkaç dakika sonra uygulama hizmetiniz oluşturulur. Portalda hizmete ilişkin çeşitli menü seçeneklerini keşfetmek için birkaç dakikanızı ayırın.
   
   ![image2][image2]    
4. URL’ye tıklayın. Havuzlar ve depolara yönelik çeşitli mevcut seçeneklere dikkat edin. Ayrıca .NET, Java ve Ruby gibi seçtiğiniz dilleri ve çerçeveleri kullanabilirsiniz.
   
   ![image3][image3]    
5. Azure portalı sürekli dağıtımı yalnızca birkaç basit adımdan oluşan kolay bir işlem haline getirir. Azure portalında oluşturduğunuz app service için ait simgeden ayarları seçin.
   
   ![image4][image4]
   
   Sağ tarafta açılan dikey pencereden yayımlama bölümüne gidin.
   
   ![image5][image5]
6. Ardından, uygulamanın sürekli dağıtımını etkinleştirmek için bazı ayarları yapılandırın. Dağıtım Kaynağı’na ve ardından Kaynak Seç’e tıklayın. Depo kaynakları için sahip olduğunuz çeşitli seçeneklere dikkat edin.
   
   ![image6][image6]
7. Bu örnek için GitHub'ı seçin. İsteğe bağlı olarak, tercih ettiğiniz depoyu seçin ve yetkilendirme kimlik bilgilerini ayarlayın.
   
   ![image7][image7]
8. Deponuzda yetkilendirme yaptıktan sonra dağıtmak istediğiniz bir projeyi ve dalı seçebilirsiniz. Aşağıda birkaç hayali örnek listelenmiştir.
   
   ![image8][image8]
9. Projenizi ve dalınızı seçtikten sonra Tamam'a tıklayın. Bir dağıtımın bildirimlerini görmeye başlamanız gerekir.
   
   ![image9][image9]
10. Kaynak denetim deposunu Azure ile tümleştirmek üzere oluşturulan web kancasını görmek için GitHub’a geri gidin. Azure portalı yalnızca birkaç basit adımda GitHub ile tümleştirme sağlar.
    
    ![image10][image10]
11. Sürekli dağıtımı göstermek için depoya bir miktar içeriği hızlıca ekleyin. Basit bir örnek için GitHub deposuna örnek bir metin dosyası ekleyin. App Service ile .NET, Ruby, Python veya başka bir uygulama türü kullanabilirsiniz. Tercih ettiğiniz depoya bir metin dosyası, ASP.NET MVC, Java ya da Ruby uygulaması eklemekten çekinmeyin.
    
    ![image11][image11]
12. Değişiklikleri deponuza uyguladıktan sonra portal bildirimleri alanında yeni bir dağıtımın başladığını görürsünüz. Değişiklikleri deponuza uyguladıktan sonra hızlıca görmüyorsanız Eşitle’ye tıklayın.
    
    ![image12][image12]
13. Bu noktada uygulama hizmeti için sayfayı yüklemeyi denerseniz bir 403 hatası alabilirsiniz. Bu örnekte bunun nedeni index.htm veya default.html dosyası gibi sayfa için tipik bir varsayılan belge ayarı olmamasıdır. Hızlı bir şekilde bu Azure portalında aracınızla çözebilirsiniz.  Azure portalında, Ayarlar'ı seçin &gt; uygulama ayarları.
    
     ![image13][image13]
14. Uygulama ayarları için bir dikey pencere açılır. “SamplePage.html” sayfasının adını girin ve Kaydet’e tıklayın. Birkaç dakika boyunca diğer ayarları keşfedin.
    
    ![image14][image14]
15. Beklenen değişiklikleri gördüğünüzden emin olmak için tarayıcı URL’nizi isteğe bağlı olarak yenileyin. Bu örnekte, artık sayfayı dolduran bazı basit metinler vardır. Depoda yapılan her ek değişiklik yeni bir otomatik dağıtımla sonuçlanır.
    
    ![image15][image15]
    
    Azure portal ile sürekli dağıtımı etkinleştirme kolay bir deneyimdir. Ayrıca daha karmaşık yayın işlem hatları oluşturabilir ve otomatik derleme ile yayın yönetimi sistemlerinden yararlanma dahil olmak üzere var olan kaynak denetimi ve Azure’a dağıtılacak sürekli tümleştirme sistemleri ile diğer birçok tekniği kullanabilirsiniz.

## <a name="develop-and-test-an-app"></a>Uygulama geliştirme ve test etme
Ardından, kod temelinde bazı değişiklikler yapın ve bu değişiklikleri hızlıca dağıtın. Bazı performans Web uygulaması için testleri ayarlayacaksınız.

1. Azure Portal'da Gezinti bölmesinden uygulama Hizmetleri'ni seçin ve App service'inizi bulun.
   
   ![image16][image16]
2. Araçlar'a tıklayın
   
   ![image17][image17]
3. Araçlar altındaki geliştirme kategorisine dikkat edin. Bize Azure Portal'dan çıkmadan uygulamalarla çalışmaya sağlayan birkaç faydalı araç burada vardır. Konsol'a tıklayın.
   
   ![image18][image18]
4. Konsol penceresinde uygulamanız için dinamik komutlar verebilirsiniz. Dir komutunu yazın ve enter tuşuna basın. Yükseltilmiş ayrıcalıklar gerektiren komutlar çalışmaz.
   
   ![image19][image19]
5. Geliştirme kategorisine geri dönün ve Visual Studio Online’ı seçin. Note: Visual Studio Online artık Visual Studio Team Services olarak adlandırılmaktadır.
   
   ![image20][image20]
6. Uygulamanız için tarayıcı içi düzenleme deneyimini açın.
   
   ![image21][image21]
7. Uygulamanız için bir web uzantısı yüklenir. Uzantılar Azure’daki uygulamalara hızlı ve kolay bir şekilde işlevsellik katar. Aşağıdaki ekran görüntüsünde başka uzantı türleri de mevcuttur.
   
   ![image22][image22]
8. Visual Studio Online uzantısı yüklendikten sonra Git'e tıklayın.
   
   ![image23][image23]
9. Geliştirme IDE’sini doğrudan tarayıcıda gördüğünüz bir tarayıcı sekmesi açılır. Aşağıdaki deneyim Chrome’dadır.
   
   ![image24][image24]
10. Dosya düzenleme, dosya ve klasör ekleme ile canlı siteden içerik indirme gibi birkaç etkinlik gerçekleştirebilirsiniz. SamplePage.html dosyasında hızlı bir düzenleme yapın.
    
    ![image25][image25]
11. Birkaç dakika içinde değişiklikler otomatik olarak kaydedilir. Sayfaya geri dönerseniz değişiklikleri görebilirsiniz. Bunlar gibi canlı düzenlemeler üretim ortamları için büyük olasılıkla uygun değildir. Ancak, araçlar geliştirme ve test ortamları için hızlı değişiklikler yapmayı çok kolay hale getirir.
    
    ![image26][image26]
    
    ![image27][image27]
12. Araçlar dikey penceresine geri gidin ve Geliştirme kategorisi altında Performans Testi’ne tıklayın.
    
    ![image28][image28]
13. Bir Team Services hesabı ayarlamanız gerekir. Daha fazla ayrıntı için buraya bakın: [Team Services Hesabı Oluşturma](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
14. Performans testi oluşturmak için Yeni’ye tıklayın.
    
    ![image29][image29]
    
    Çeşitli değerleri yapılandırın ve iletişim kutusunun altındaki Testi Çalıştır’a tıklayarak bir performans testi başlatın.
    
    ![image30][image30]
    
    ![image31][image31]
15. Test çalışmaya başladıktan sonra durumunu izleyebilirsiniz.
    
    ![image32][image32]
    
    Test tamamlandıktan sonra sonuca tıklandığında daha ayrıntılı bilgi gösterilir.
    
    ![image33][image33]
16. Bu örnekte küçük bir test çalışması oluşturdunuz; bu nedenle analiz etmek için sınırlı veri olabilir, ancak çeşitli ölçümleri görebilir ve testinizi bu görünümden yeniden çalıştırabilirsiniz. Oluşturma Azure portal yapar, kolay bir işlem yürütme ve web performans çözümleme sınar. Aşağıdaki ekran görüntüleri performans verilerini göstermektedir.
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a>Bir uygulamayı izleme ve sorunlarını giderme
Azure, çalışan uygulamaları izleme ve sorunlarını gidermeye yönelik çok sayıda özellik sağlar.

1. Azure portalında Web uygulaması için Araçlar'ı seçin.
   
   ![image37][image37]
2. Sorun Giderme kategorisi altında çalışan bir uygulamayla ilgili olası sorunları gidermek üzere araç kullanma seçeneklerinin çeşitliliğine dikkat edin. Canlı HTTP trafiğini izleme, kendi kendini onarmayı etkinleştirme, günlükleri görüntüleme ve daha fazla işlemi yapabilirsiniz.
   
   ![image38][image38]
3. Bazı HTTP kodlarına ilişkin hızlı bir bakış için Site Ölçümleri’ni seçin.
   
   ![image39][image39]
4. Hizmet Olarak Tanılama’yı seçin. Uygulamanızın türünü ve ardından Çalıştır'ı seçin.
   
   ![image40][image40]
   
   Bir koleksiyon başlar.
   
   ![image41][image41]
5. Olası sorunları tanılamak için uygun günlüğünü seçebilirsiniz. HTTP Günlükleri gibi tüm kullanılabilir veri seçeneklerini görmek için günlüğe kaydetmeyi etkinleştirmeniz gerekir.
   
   ![image42][image42]
   
   Olası sorunları çözmeye yardımcı olmak üzere, Bellek Dökümü dosyasına tıklayarak bir DebugDiag analiz raporu indirip analiz edebilirsiniz.
   
   ![image43][image43]
6. Daha fazla veri görüntülemek için ek günlük kaydını etkinleştirmeniz gerekir. Azure portalında Web uygulamasına gidin ve ayarları'nı seçin.
   
   ![image44][image44]
7. Özellikler kategorisine inin ve Tanılama günlükleri’ni seçin.
   
      ![image45][image45]
8. Günlük kaydına ilişkin çeşitli seçeneklere dikkat edin. Web sunucusu günlüğünü açın ve Kaydet'e tıklayın.
   
   ![image46][image46]
9. Uygulamanın araçlar alanına geri dönün ve Hizmet olarak tanılama’yı seçip Çalıştır’a tıklayarak veri koleksiyonunu yeniden çalıştırın.
   
   ![image47][image47]
10. HTTP günlüğü ayarı etkin olduğunda bundan böyle HTTP Günlükleri için toplanan verileri görebilirsiniz.
    
    ![image48][image48]
11. HTML dosya günlüğüne tıklayarak, daha fazla araştırma için zengin bir tarayıcı tabanlı rapor oluşturabilirsiniz.
    
    ![image49][image49]
12. Azure Portalı'ndaki Araçlar bölümüne uygulama için geri dönün. Araçlar bölümüne gidin ve İşlem Gezgini'ni seçin.
    
    ![image50][image50]
13. İşlem Gezgini'ni seçerek, çalışan işlemlere ilişkin ayrıntıları görüntüleyebilirsiniz. Aşağıda işlemlerin ayrıntılarına inebilir ve hatta Azure portalından tüm işlemleri sonlandırma kullanabilirsiniz.
    
    ![image51][image51]
    
    ![image52][image52]
14. Sol taraftaki Ayarları dikey penceresine geri dönün. Yeni destek isteği’ne tıklayın.
    
    ![image53][image53]
15. Sağdaki dikey pencereden sorunlara ilişkin ayrıntılı bilgileri doldurabilir, iletişim bilgilerini girebilir ve hatta tanılama verilerini karşıya yükleyebilirsiniz. Azure portal, Microsoft desteği ile sorunsuzca çalışmayı sağlar.
    
    ![image54][image54]
    
    ![image55][image55]
    
    Güçlü ve bilindik araç deneyimleri izlemenize yardımcı olmak ve çalışan uygulamalarınızı sorunlarını gidermek için Azure portalı sağlar. Ayrıca işlemleri geri dönüştürme, çeşitli veri koleksiyonlarını etkinleştirme ve devre dışı bırakma ve hatta Microsoft profesyonel desteği ile tümleştirme gibi görevleri gerçekleştirerek hızlıca önlem alabilirsiniz.

## <a name="general-application-management"></a>Genel Uygulama Yönetimi
Uygulamaları yönetirken çoğunlukla yedekleme stratejilerini yapılandırma, kimlik sağlayıcılarını uygulama ve yönetme ile Rol tabanlı erişim denetimini yapılandırma gibi çok çeşitli etkinlikler gerçekleştirmeniz gerekir. Diğer DevOps deneyimlerinde olduğu gibi Azure platformu da bu görevleri doğrudan portalda tümleştirir.

1. Web Uygulaması veri kaybından koruduğunuzdan emin olmak için yedeklemeleri yapılandırmanız gerekir. Web uygulamanızın Ayarlar alanına gidin.
   
   ![image56][image56]
2. Sağdaki dikey pencerede Özellikler kategorisine inin.
   
    ![image57][image57]
3. Yedeklemeler’i seçin; sağ tarafta bir dikey pencere açılır.
   
   ![image58][image58]
4. Yapılandır'a tıklayın ve sağ taraftaki dikey pencereden bir depolama hesabı seçin.
   
   ![image59][image59]
5. Bundan sonra yedeklemelerinizin tutulacağı bir depolama kapsayıcısı oluşturup seçin. Dikey pencerenin alt kısmındaki Oluştur’a tıklayın. Ardından kapsayıcıyı seçin.
   
   ![image60][image60]
6. Kapsayıcıyı seçtikten sonra zamanlamaları yapılandırın, yapabilir veritabanlarınız için yedeklemeler ayarlayabilir. Bu senaryo için Kaydet simgesine tıklayın.
   
    ![image61][image61]
7. Kaydettikten sonra Yedeklemeler için soldaki dikey pencereye geri dönün. Uygulamayı yedeklemek için Şimdi Yedekle’ye tıklayın.
   
    ![image62][image62]
8. Birkaç dakika içinde bir yedeğin oluşturulduğunu görürsünüz. Aşağıdaki ekran görüntüsünde Şimdi Geri Yükle seçeneğine dikkat edin.
   
    ![image63][image63]
9. Şimdi Geri Yükle’ye tıklayın ve sağ taraftaki dikey pencerede bulunan seçenekleri inceleyin. Uygun bir yedekleme seçebilir ve gerektiğinde daha önceki bir duruma kolayca geri yükleyebilirsiniz. Azure portalı, uygulama için basit bir olağanüstü durum kurtarma stratejisi etkinleştirmemize yardımcı olmuştur.
   
    ![image64][image64]
10. Soldaki Ayarlar dikey penceresine geri dönün ve Özellikler altında Kimlik Doğrulama/Yetkilendirme’yi seçin.
    
     ![image65][image65]
11. Sağdaki dikey pencereden App Service Kimlik Doğrulaması’nı seçin. Popüler sağlayıcılar ile yapılandırabileceğiniz çeşitli seçeneklere dikkat edin.
    
     ![image66][image66]
12. Tercih ettiğiniz sağlayıcıyı seçin ve kapsam seçeneklerine dikkat edin. Bir Uygulama Kimliği ve Uygulama Gizli Anahtarı belirtebilir ve uygulama için Facebook kimlik doğrulamasını kolayca etkinleştirebilirsiniz. Azure portal, uygulamalar için hazır bir çözüm olarak kimlik doğrulaması sağlar.
    
     ![image67][image67]
13. Ayarlar dikey penceresine geri dönün ve Kaynak Yönetimi kategorisi altında Kullanıcılar’ı seçin.
    
     ![image68][image68]
14. Sağdaki dikey pencerede rol ve kullanıcı eklemeye yönelik çeşitli seçeneklere dikkat edin. Azure portalında RBAC (rol tabanlı erişim denetimi) uygulama için kolayca denetlemenize olanak tanır.
    
     ![image69][image69]

## <a name="summary"></a>Özet
Bu öğretici bir web uygulaması için sürekli dağıtımı hızlıca etkinleştirerek, çeşitli geliştirme ve test etkinlikleri gerçekleştirerek, canlı bir uygulamayı izleyip sorunlarını gidererek ve son olarak olağanüstü durum kurtarma, kimlik ve rol tabanlı erişim denetimi gibi anahtar stratejileri yöneterek Azure platformunun bazı olumlu yönlerini göstermiştir. Azure platformu bu DevOps iş akışları için tümleştirilmiş bir deneyim sağlar ve elinizdeki metnin bağlamını koruyarak verimli bir şekilde çalışabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Resource Manager, Azure platformunda DevOps’un etkinleştirilmesi için önemlidir.  Daha fazla bilgi edinmek için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).
* Azure Uygulama Hizmeti dağıtımı hakkında daha fazla bilgi için [Uygulamanızı Azure Uygulama Hizmeti’e dağıtma](../app-service/app-service-deploy-local-git.md) sayfasını ziyaret edin

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
