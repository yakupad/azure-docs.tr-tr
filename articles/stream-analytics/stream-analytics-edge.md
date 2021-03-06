---
title: Azure Stream Analytics olarak IOT kenarında (Önizleme)
description: Azure Stream Analytics kenar işleri oluşturmak ve bunları Azure IOT kenar aygıtları runnning dağıtabilirsiniz.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/16/2017
ms.openlocfilehash: 5ce0420dde5bf232fe8067a3b14814f14380602e
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34802536"
---
# <a name="azure-stream-analytics-on-iot-edge-preview"></a>Azure Stream Analytics olarak IOT kenarında (Önizleme)

> [!IMPORTANT]
> Bu işlevselliği Önizleme aşamasındadır ve üretimde kullanım için önerilmez.
 
Azure Stream Analytics (ASA) olarak IOT kenarında böylece cihaz tarafından oluşturulan verilerin tam değerini kilidini açabilir yakın gerçek zamanlı analitik Intelligence yakın IOT cihazlara dağıtmak için geliştiricilere güçlendirir. Azure Stream Analytics, düşük gecikme süresi, dayanıklılık, bant genişliği ve uyumluluk verimli kullanımı için tasarlanmıştır. Kuruluşların şimdi Denetim mantığı endüstriyel işlemleri yakın dağıtın ve bulutta yapılan büyük veri analizi tamamlar.  

Azure Stream Analytics IOT kenar üzerinde çalışan içinde [Azure IOT kenar](https://azure.microsoft.com/campaigns/iot-edge/) framework. İş içinde ASA oluşturulduktan sonra dağıtın ve ASA işleri IOT hub'ı kullanarak yönetin. Bu özellik önizlemede. Sorularınız veya geri bildirim varsa, kullanabileceğiniz [Bu anket](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) ürün ekibine başvurun. 

## <a name="scenarios"></a>Senaryolar
![Üst düzey diyagramı](media/stream-analytics-edge/ASAedge_highlevel.png)

* **Düşük gecikme süreli komut ve Denetim**: Örneğin, güvenlik sistemleri üretim işletimsel veri son derece düşük gecikme süresine sahip yanıtlaması gerekir. IOT Kenar çubuğunda ASA ile verileri yakın gerçek zamanlı ve bir makine durdurmak veya uyarıları tetiklemek için anormallikleri algıladığınızda komutları vermek algılayıcı çözümleyebilirsiniz.
*   **Bulut bağlantı sınırlı**: Uzak araştırma ekipman, bağlı tekneler veya offshore ayrıntılara, gibi görev kritik sistemler gereksinim çözümlemek ve bulut bağlantı aralıklı olduğunda bile veri tepki vermek. ASA, akış mantıksal ağ bağlantısı bağımsız olarak çalışır ve hangi buluta başka bir işleme veya depolama için gönderdiğiniz seçebilirsiniz.
* **Sınırlı bant genişliği**: veri birimi jet motoru tarafından üretilen veya bağlı araba veri filtre veya gereken önceden işlenen buluta göndermeden önce çok büyük olabilir. ASA kullanarak, filtre veya buluta gönderilmesi gereken veri toplama.
* **Uyumluluk**: Mevzuat uyumluluğu, yerel olarak anonim veya buluta gönderilmeden önce bir araya getirilir için bazı veriler gerektirebilir.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Azure Stream Analytics kenar işler
### <a name="what-is-an-edge-job"></a>Bir "kenar" işi nedir?

ASA kenar işleri çalıştırma modüllerle içinde [Azure IOT kenar çalışma zamanı](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works). İki bölümden oluşan bunlar:
1.  İş tanımı için sorumlu olan bir bulut bölümü: kullanıcılar, bulutta girişleri, çıktı, sorgu ve diğer ayarları (bozuk olayları, vb.) tanımlayın.
2.  Yerel olarak çalışan IOT kenar modül ASA. ASA karmaşık olay işleme altyapısı içerir ve iş tanımı buluttan alır. 

ASA kenar işleri aygıtlara dağıtmak için IOT hub'ı kullanır. Hakkında daha fazla bilgi [IOT kenar dağıtım görülme burada](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

![Edge işi](media/stream-analytics-edge/ASAedge_job.png)


### <a name="installation-instructions"></a>Yükleme yönergeleri
Üst düzey adımlar aşağıdaki tabloda açıklanmıştır. Daha fazla ayrıntı aşağıdaki bölümlerde verilmiştir.
|      |Adım   | Yerleştir     | Notlar   |
| ---   | ---   | ---       |  ---      |
| 1   | **Depolama kapsayıcısı oluşturma**   | Azure portalına       | Depolama kapsayıcıları, burada bunlar IOT cihazlarınızı tarafından erişilip iş tanımınızı kaydetmek için kullanılır. <br>  Var olan tüm depolama kapsayıcısını yeniden kullanabilirsiniz.     |
| 2   | **Bir ASA kenar işi oluşturma**   | Azure portalına      |  Select yeni bir proje oluşturmak **kenar** olarak **barındırma ortamı**. <br> Bu işleri oluşturulan ve yönetilen buluttan ve kendi IOT sınır cihazları üzerinde çalıştırın.     |
| 3   | **IOT kenar ortamınıza aygıtlarınızın Kurulumu**   | Cihazlar      | Yönergeler için [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) veya [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).          |
| 4   | **ASA IOT kenar aygıtlarınızın dağıtma**   | Azure portalına      |  ASA iş tanımı daha önce oluşturduğunuz depolama kapsayıcısı dışarı aktarılır.       |
İzleyebileceğiniz [Bu adım adım öğretici](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics) ilk ASA işinizde IOT kenar dağıtmak için. Aşağıdaki videoda bir IOT sınır cihazı bir Stream Analytics işini çalıştırmak için işlem anlamanıza yardımcı olması:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]

#### <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
Depolama kapsayıcısı derlenmiş ASA sorgu ve iş yapılandırmasını dışarı aktarmak için gereklidir. ASA Docker görüntüsünü belirli sorgunuzu ile yapılandırmak için kullanılır. 
1. İzleyin [bu yönergeleri](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) Azure portalından bir depolama hesabı oluşturmak için. ASA ile bu hesabı kullanmak için tüm diğer varsayılan seçenekleri kullanmaya devam edebilir.
2. Yeni oluşturulan depolama hesabında blob depolama kapsayıcısını oluşturun:
    1. Tıklayın **BLOB'lar**, ardından **+ kapsayıcı**. 
    2. Bir ad girin ve kapsayıcı olarak tutun **özel**.

#### <a name="create-an-asa-edge-job"></a>Bir ASA kenar işi oluşturma
> [!Note]
> Bu öğreticide Azure portalını kullanarak ASA iş oluşturma odaklanır. Ayrıca [ASA kenar işi oluşturmak için Visual Studio eklentisi kullanma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)

1. Azure Portal'dan yeni "Stream Analytics işi" oluşturun. [Burada yeni bir ASA işi oluşturmak için doğrudan bağlantı](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

2. Oluşturma ekranında şunları seçin **kenar** olarak **barındırma ortamı** (aşağıdaki resme bakın) ![proje oluşturma](media/stream-analytics-edge/ASAEdge_create.png)
3. İş tanımı
    1. **Giriş Stream(s) tanımlamak**. Bir veya birkaç giriş akışları işinizi tanımlayın.
    2. Başvuru verileri (isteğe bağlı) tanımlayın.
    3. **Çıktı Stream(s) tanımlamak**. Bir veya birkaç çıkış akışları işinizi tanımlayın. 
    4. **Sorgu tanımlamak**. ASA sorgu satır içi Düzenleyicisi'ni kullanarak bulutta tanımlayın. Derleyici ASA köşesi etkin sözdizimi otomatik olarak denetler. Örnek verileri karşıya yükleyerek, sorgunuzu test edebilirsiniz. 
4. Depolama kapsayıcısı bilgilerini kümesinde **IOT Edge ayarları** menüsü.
5. İsteğe bağlı ayarlar
    1. **Olay sıralama**. Portalda sipariş ilkesi yapılandırabilirsiniz. Belgeleri kullanılabilir [burada](https://msdn.microsoft.com/library/azure/mt674682.aspx?f=255&MSPPError=-2147217396).
    2. **Yerel ayar**. İnternalization biçimini ayarlayın.



> [!Note]
> Bir dağıtım oluşturduğunuzda, ASA iş tanımı için bir depolama kapsayıcısı dışa aktarır. Bu iş tanımı aynı kalır bir dağıtım süresi boyunca. Sonuç olarak, kenar üzerinde çalışan bir işi güncelleştirmek istiyorsanız, ASA işinde düzenleyin ve sonra yeni bir dağıtım IOT hub'ı oluşturmak gerekir.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>IOT kenar ortamınıza aygıtlarınızın ayarlama
Azure IOT kenar çalıştıran cihazlarda kenar işleri dağıtılabilir.
Bunun için bu adımları gerçekleştirmeniz gerekir:
- IOT hub'ı oluşturun.
- Docker ve IOT kenar sınır aygıtlarınızda yükleyin.
- Cihaz olarak ayarlamak **IOT sınır cihazları** IOT hub.

Bu adımları IOT kenar belgelerinde açıklanan [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) veya [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>IOT kenar aygıtlarınızın ASA dağıtımı
##### <a name="add-asa-to-your-deployment"></a>Dağıtımınız için ASA ekleme
- Azure portalında, IOT hub'ı açın ve gidin **IOT kenar** ve hedeflemek için bu dağıtım için istediğiniz cihaza tıklayın.
- Seçin **ayarlamak modülleri**seçeneğini belirleyip **+ Ekle** ve **Azure Stream Analytics Modülü**.
- Abonelik ve oluşturduğunuz ASA kenar işi seçin. Kaydet'i tıklatın.
![Dağıtımınızda ASA Modül Ekle](media/stream-analytics-edge/set_module.png)


> [!Note]
> Bu adım sırasında ASA (zaten mevcut değilse) depolama kapsayıcısında "EdgeJobs" adlı bir klasör oluşturur. Her dağıtım için yeni bir alt "EdgeJobs" klasöründe oluşturulur.
> İşinizi kenar aygıtlara dağıtmak için ASA iş tanımı dosyası için bir paylaşılan erişim imzası (SAS) oluşturur. SAS anahtarını cihaz çifti kullanarak IOT sınır cihazları güvenli bir şekilde aktarılır. Bu anahtar kullanım süresi sonu oluşturulduktan gününden üç yıl sayısıdır.


IOT kenar dağıtımları hakkında daha fazla bilgi için bkz: [bu sayfayı](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).


##### <a name="configure-routes"></a>Yolları yapılandırın
IOT kenar bildirimli olarak modülleri arasında ve modülleri ve IOT hub'ı arasında iletileri yönlendirmek için bir yol sağlar. Tam sözdizimini açıklanan [burada](https://docs.microsoft.com/azure/iot-edge/module-composition).
Girişleri ve çıkışları ASA işinde oluşturulan adlarını yönlendirme için uç noktalar olarak kullanılabilir.  

###### <a name="example"></a>Örnek
```
{
"routes": {                                              
    "sensorToAsa":   "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/ASA/inputs/temperature\")",
    "alertsToCloud": "FROM /messages/modules/ASA/* INTO $upstream", 
    "alertsToReset": "FROM /messages/modules/ASA/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")" 
}
}   

```
Bu örnek için aşağıdaki resimde açıklanan senaryo rotaları gösterir. Adlı bir kenar işi içeren "**ASA**", adlandırılmış bir girişi "**sıcaklık**"ve adlı bir çıktı"**uyarı**".
![Yönlendirme örneği](media/stream-analytics-edge/RoutingExample.png)

Bu örnekte aşağıdaki rotaların tanımlar:
- Her ileti **tempSensor** adlı modül gönderilen **ASA** adlı giriş **sıcaklık**,
- Tüm çıkışları **ASA** modülü bu (upstream$), cihaza bağlı IOT hub'ına gönderilir
- Tüm çıkışları **ASA** modülü gönderilen **denetim** uç noktasını **tempSensor**.


## <a name="technical-information"></a>Teknik bilgiler
### <a name="current-limitations-for-edge-jobs-compared-to-cloud-jobs"></a>Bulut işlerini karşılaştırıldığında kenar işleri için geçerli sınırlamalar
Hedef işleri kenar ve işleri bulut eşlik arasında olmalıdır. Çoğu SQL sorgu dil özellikleri zaten desteklenir.
Ancak aşağıdaki özellikleri kenar işleri henüz desteklenmez:
* Kullanıcı tanımlı işlevler (UDF) ve kullanıcı tanımlı toplamlarda (UDA).
* Azure ML işlevleri.
* Birden fazla 14 toplamalar tek bir adımda kullanma.
* Giriş/Çıkış AVRO biçimi. Şu anda yalnızca CSV ve JSON desteklenir.
* Aşağıdaki SQL işleçleri:
    * AnomalyDetection
    * Jeo-uzamsal işleçler:
        * CreatePoint
        * CreatePolygon
        * CreateLineString
        * ST_DISTANCE
        * ST_WITHIN
        * ST_OVERLAPS
        * ST_INTERSECTS
    * BÖLÜM
    * GetMetadataPropertyValue


### <a name="runtime-and-hardware-requirements"></a>Çalışma zamanı ve donanım gereksinimleri
ASA IOT kenarına çalıştırmak için çalıştırabilirsiniz aygıtları gerekir [Azure IOT kenar](https://azure.microsoft.com/campaigns/iot-edge/). 

ASA ve Azure IOT kullanmak kenar **Docker** kapsayıcıları (Windows, Linux) birden çok ana bilgisayar işletim sistemlerinde çalışan taşınabilir bir çözüm sağlar.

ASA IOT Kenar çubuğunda x86 64 veya Azure Resource Manager mimarileri üzerinde çalışan Windows ve Linux görüntü olarak kullanılabilir hale getirilir. 


### <a name="input-and-output"></a>Giriş ve çıkış
#### <a name="input-and-output-streams"></a>Giriş ve çıkış akışları
ASA kenar işleri IOT kenar cihazlarda çalışan diğer modüllerden girişleri ve çıkışları elde edebilirsiniz. Gelen ve belirli modüller bağlanmak için dağıtım sırasında yönlendirme yapılandırması ayarlayabilirsiniz. Daha fazla bilgi üzerinde açıklanan [IOT kenar modülünün birleşim belgelerine](https://docs.microsoft.com/azure/iot-edge/module-composition).

Giriş ve çıkış için CSV ve JSON biçimleri desteklenir.

Her giriş ve çıkış akışına ASA işinizi oluşturmak için karşılık gelen bir uç nokta dağıtılan modülünüzün oluşturulur. Bu uç noktalar, dağıtımınızın yollar kullanılabilir.

AT var, yalnızca akış girişi ve desteklenen akış çıktı türleri Edge Hub. Giriş destekler başvurusu dosya türü başvuru. Diğer çıkışlar bulut işi aşağı kullanılarak erişilebilir. Örneğin, Edge'de barındırılan bir Stream Analytics işi çıkış IOT Hub'ına sonra gönderebilir kenar Hub'ına çıkış gönderir. IOT Hub ve çıktı girişten Power BI veya başka bir çıktı türü ile bir ikinci barındırılan bulut Azure akış analizi işi'ni kullanabilirsiniz.



##### <a name="reference-data"></a>Başvuru verileri
Başvuru verileri (arama tablosu olarak da bilinir) statik veya yavaş doğası gereği değiştirme sınırlı bir veri kümesi ' dir. Bir arama gerçekleştirmek ya da veri akışı ile ilişkilendirmek için kullanılır. Yapmak için Azure Stream Analytics işiniz başvuru verilerinde kullanımı, genellikle kullanacağınız bir [başvuru veri birleştirme](https://msdn.microsoft.com/library/azure/dn949258.aspx) Sorgunuzdaki. Daha fazla bilgi için bkz: [ASA belgelerine başvuru verileri hakkında](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data).

Başvuru verileri IOT Kenar çubuğunda ASA kullanmak için aşağıdaki adımları izleyin: 
1. İşiniz için yeni bir giriş oluştur
2. Seçin **başvuru verileri** olarak **kaynak türü**.
3. Dosya yolu ayarlayın. Dosya yolu olmalıdır bir **mutlak** cihazdaki dosya yolu ![başvuru veri oluşturma](media/stream-analytics-edge/ReferenceData.png)
4. Etkinleştirme **paylaşılan sürücüleri** , Docker yapılandırmanızı ve emin olun sürücü dağıtımınıza başlamadan önce etkinleştirilir.

Daha fazla bilgi için bkz: [burada Docker için Windows belgelerine](https://docs.docker.com/docker-for-windows/#shared-drives).

> [!Note]
> Şu anda yalnızca yerel başvuru verileri desteklenir.




## <a name="license-and-third-party-notices"></a>Lisans ve üçüncü taraf bildirimler
* [Azure Stream Analytics IOT kenar Önizleme lisans üzerindeki](https://go.microsoft.com/fwlink/?linkid=862827). 
* [Azure akış analizi için üçüncü taraf bildirim IOT kenar preview'daki](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Sonraki adımlar

* [Azure IOT kenar hakkında daha fazla bilgi](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)
* [ASA IOT kenar öğretici hakkında](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Bu Anketi kullanılarak ekibine geri bildirim gönderin](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
* [Visual Studio Araçları'nı kullanarak Stream Analytics kenar işleri geliştirin](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
