---
title: Tanılama ve Azure zaman serisi Öngörüler sorunlarını çözmek | Microsoft Docs
description: Bu makalede, Azure zaman serisi Öngörüler ortamınızda karşılaşabileceğiniz sık karşılaşılan sorunları tanılamak ve sorun giderme açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: venkatja
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 04/09/2018
ms.openlocfilehash: dbd32d57206b611a37b5349e5971d2efe272ee1a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292883"
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Zaman serisi Öngörüler ortamınızdaki sorunları tanılamada ve

## <a name="problem-1-no-data-is-shown"></a>Sorun 1: Hiçbir veri gösterilir
Neden görmemiş verilerinizi çeşitli ortak nedenleri vardır [Azure zaman serisi Öngörüler Gezgini](https://insights.timeseries.azure.com):

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Olası neden A: olay kaynağı veri JSON biçiminde değil
Azure zaman serisi Öngörüler yalnızca JSON verilerini destekler. JSON örnekler için bkz: [desteklenen JSON şekiller](time-series-insights-send-events.md#supported-json-shapes).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Olası neden B: olay kaynağı anahtarı gerekli izni eksik
* Bir IOT Hub için sahip anahtarı sağlamanız gerekir. **service bağlanma** izni.

   ![IOT Hub hizmeti connect izni](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Önceki görüntüde ilkelerinin ya da gösterildiği gibi **iothubowner** ve **hizmet** çalışır, her ikisi de olduğundan **service bağlanma** izni.
   
* Bir event hub için sahip anahtarı sağlamanız gerekir. **dinleme** izni.

   ![Olay hub'ı dinleme izni](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Önceki görüntüde ilkelerinin ya da gösterildiği gibi **okuma** ve **yönetmek** çalışır, her ikisi de olduğundan **dinleme** izni.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>Olası neden tüketici grubu zaman serisi Insights'a özel değildir sağlanan C:
IOT hub'ı kaydı veya bir olay hub'ı sırasında verileri okumak için kullanılması gereken tüketici grubu belirtin. Bu bir tüketici grubundaki gerekir **değil** paylaştırılır. Tüketici grubu paylaşılıyorsa, temel olay hub'ı otomatik olarak okuyucular birini rastgele bağlantısını keser. Okunacak zaman serisi Öngörüler için benzersiz bir tüketici grubundaki sağlar.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Sorun 2: Bazı veriler gösterilir, ancak bazı eksik
Veri kısmen görebilirsiniz, ancak veri geciken, dikkate alınması gereken birkaç olasılık vardır:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>Olası neden A: ortamınızı azaltıldı
Bu, bir olay kaynağı veri ile oluşturulduktan sonra ortamları sağlandığında yaygın görülen bir sorundur.  Azure IOT hub'ları ve olay hub'ları veri yedi gün için depolama yapar.  TSI her zaman en eski olayın (FIFO) içinde olay kaynağı başlar.  Bu nedenle bir S1, tek birim TSI ortamına bağlandığında, bir olay kaynağı beş milyon olay varsa TSI yaklaşık bir milyon olay günde okuyun.  Bu, TSI ilk bakışta beş gün gecikme yaşıyor gibi sorgulamanıza aramak için görünebilir.  Gerçekte ise ortamı kısıtlanıyor aramasıdır.  Eski olayları, olay kaynağı varsa, iki yoldan biriyle yaklaşır:

- Yardımcı olmak için olay kaynağınızın bekletme sınırları içinde TSI göstermeyi istemediğiniz eski olayları elden Değiştir
- Eski olayların verimliliğini artırmak için daha büyük bir ortam boyutu (bakımından birimlerinin sayısı) sağlayın.  Bu aynı S1 ortam beş birimlerine bir gün artan, yukarıdaki örneği kullanarak, ortam şimdi gün içinde nım gerekir.  Kararlı durum olay üretim 1 M veya daha az olaylar/gün olursa, onu yakalanan sonra olayına geri tek bir birim kapasitesi azaltabilir.  

Azaltma sınırını ortamı SKU türüne ve kapasite göre uygulanır. Tüm olay kaynakları ortamında bu kapasite paylaşır. IOT hub'ını veya olay hub'ı için olay kaynağı veri zorlanan sınırları ötesinde itmesi olursa, azaltma ve bir gecikme bakın.

Aşağıdaki diyagramda, bir SKU S1 ve 3 kapasiteye sahip bir zaman serisi Öngörüler ortamı gösterilmektedir. Gün başına giriş 3 milyon olayları dağıtabilirsiniz.

![Ortam SKU geçerli kapasitesi](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Örneğin, bu ortam iletiler event hub'ındaki alma olduğunu varsayın. Aşağıdaki diyagramda gösterildiği giriş oranı inceleyin:

![Bir olay hub'ına yönelik örnek giriş oranı](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Aşağıdaki çizimde gösterildiği gibi günlük giriş ~ 67,000 iletileri hızıdır. Bu oran 46 iletileri dakikada kabaca çevirir. Her olay hub'ı ileti tek bir zaman serisi Öngörüler olay düzleştirilmiş varsa, bu ortam hiçbir azaltma görür. Ardından her olay hub'ı ileti 100 zaman serisi Öngörüler olaylarına düzleştirilmiş, 4,600 olayları dakikada alınan. Dakikada 3 kapasiteye sahip bir S1 SKU ortam için giriş yalnızca 2,100 olayları (1 milyon olay günde 3 birimlerde dakikada 700 olayları = dakikada 2,100 olayları =). Bu nedenle, kısıtlama nedeniyle bir gecikme görürsünüz. 

Mantığı düzleştirme nasıl çalıştığını üst düzey anlamak için bkz [desteklenen JSON şekiller](time-series-insights-send-events.md#supported-json-shapes).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Aşırı azaltma için önerilen çözüm adımları
Gecikme düzeltmek için ortamınıza SKU kapasitesini artırın. Daha fazla bilgi için bkz: [zaman serisi Öngörüler ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Olası neden B: ilk alım geçmiş verilerin yavaş giriş neden oluyor
Varolan bir olay kaynağına bağlanıyorsanız, IOT hub'ını veya olay hub'ınızı zaten verilerin içinde olduğunu olasıdır. Ortam olay kaynağının ileti saklama döneminin başından veri çekme başlatır.

Bu davranış varsayılan davranıştır ve değiştirilemiyor. Azaltma gerçekleştirmesine ve geçmiş verileri alma Yakala için biraz zaman alabilir.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Önerilen çözüm adımları büyük ilk alımı
Gecikme düzeltmek için aşağıdaki adımları uygulayın:
1. (Bu durumda 10) değeri izin verilen en yüksek SKU kapasitesini artırın. Kapasitesi artırılır sonra çok daha hızlı yakalama giriş işlemi başlatır. Ne kadar hızlı, kullanılabilirlik grafikte aracılığıyla yakalama görselleştirebilirsiniz [zaman serisi Öngörüler explorer](https://insights.timeseries.azure.com). Artan kapasite için sizden ücret kesilir.
2. Gecikme yakalanan sonra SKU kapasitesi normal giriş hızınızı azaltın.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Sorun 3: My olay kaynağının *zaman damgası özelliği adı* ayarı çalışmıyor
Ad ve değer aşağıdaki kurallara uygun emin olun:
* Zaman damgası özelliği adı _büyük küçük harfe duyarlı_.
* Olay kaynağınız bir JSON dizesi olarak geldiği zaman damgası özelliği değeri biçiminde olması _yyyy-MM-ddTHH. FFFFFFFK_. Bu tür bir dize örneğidir "2008-04-12T12:53Z".

Emin olmak için en kolay yolu, *zaman damgası özelliği adı* yakalanır ve düzgün çalışmasını TSI Gezgini'ni kullanma.  TSI Gezgini içinde bir süre sonra sağladığınız seçin grafiğiyle *zaman damgası özelliği adı*.  Seçime sağ tıklayın ve seçin *olayları keşfedin* seçeneği.  İlk sütun üst bilgisi olmalıdır, *zaman damgası özelliği adı* ve olması gereken bir *($ts)* Word'ün yanındaki *zaman damgası*, yerine:
- *(abc)* , hangi TSI gösterir veri değerleri dize olarak okuma
- *Takvim simgesini*, veri değeri olarak okuma TSI belirtmek *datetime*
- *#*, hangi TSI gösterir veri değerleri tamsayı olarak okuma


## <a name="next-steps"></a>Sonraki adımlar
- Ek Yardım için bir konuşma başlatmak [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- Aynı zamanda [Azure Destek](https://azure.microsoft.com/support/options/) yardımlı destek seçenekleri için.
