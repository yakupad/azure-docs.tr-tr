---
title: Azure Time Series Insights nedir? | Microsoft Docs
description: Zaman serisi verilerinin analizi ve IoT çözümleri için yeni bir hizmet olan Azure Time Series Insights'a giriş.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: overview
ms.date: 01/26/2018
ms.openlocfilehash: e3c7881406d4d3736c185c39a85a488ecf8aab77
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317775"
---
# <a name="what-is-azure-time-series-insights"></a>Azure Time Series Insights nedir?

Time Series Insights, IoT cihazları tarafından oluşturulan veriler gibi büyük miktarlardaki zaman serisi verilerini depolamak, görselleştirmek ve sorgulamak için tasarlanmıştır.  Bulutta zaman serisi verilerini depolamak, yönetmek, sorgulamak veya görselleştirmek istiyorsanız Time Series Insights aradığınız çözüm olabilir.  

![Time Series Insights akış çizelgesi](media/overview/time-series-insights-flowchart.png)

Time Series Insights dört temel işe sahiptir:

- İlk olarak Azure IoT Hub ve Azure Event Hubs gibi bulut ağ geçitleriyle tümleşiktir. Bu olay kaynaklarına kolayca bağlanır ve JSON kodu içindeki veri içeren iletilerle yapıları ayrıştırarak temiz satırlara ve sütunlara uygular. Meta verileri telemetri verileriyle birleştirerek verilerinizi sütunlu bir depoda dizinler.
- Time Series Insights ikinci olarak verilerinizin depolanmasını yönetir. Verilerin her zaman kolayca erişilebilir durumda olmasını sağlamak için verilerinizi 400 güne kadar bellek içinde ve SSD'lerde depolar. İstek üzerine milyarlarca olayı etkileşimli olarak sorgulayabilirsiniz.
- Time Series Insights üçüncü olarak TSI gezgini aracılığıyla hazır görselleştirme sağlar.  
- Time Series Insights dördüncü olarak hem TSI gezgininde hem de zaman serisi verilerinizi özel uygulamalara eklemek için kolay tümleştirme seçenekleri sunan API'leri kullanarak bir sorgu hizmeti sunar.  

Şirket içi kullanım veya dışarıdaki müşterilerin kullanımı için bir uygulama derliyorsanız Time Series Insights zaman serisi verilerini dizinlemek, depolamak ve toplamak için arka uç olarak kullanılabilir. Bunu temel alan özel bir görselleştirme ve kullanıcı deneyimi oluşturabilirsiniz.  Time Series Insights bu senaryoyu etkinleştirmek için Sorgu API'lerini sunar.  

Verilerinizin zaman serisi olup olmadığından emin değilseniz dikkate almanız gereken noktalar burada verilmiştir.  Zaman serisi verileri bir varlığın veya işlemin zaman içindeki değişimini gösterir.  Zaman damgasına sahip olması ve en anlamlı eksenin zaman olmasıyla diğer verilerden ayrılır.  Zaman serisi verileri genellikle zaman sıralamasında gelir ve genelde veritabanınıza güncelleştirme yerine ekleme olarak işlenir.  Time Series Insights her yeni olayı bir satır olarak yakalayıp depoladığından zaman içindeki değişiklik ölçülür ve bu sayede geriye doğru bakarak gelecekteki değişiklikleri tahmin etmenizi sağlar.  Büyük hacimlerdeki verilerin depolanması, dizinlenmesi, sorgulanması, analiz edilmesi ve görselleştirilmesi zor olabilir.  

## <a name="primary-scenarios"></a>Birincil senaryolar

- Zaman serisi verilerini ölçeklenebilir bir şekilde depolama.  
  - Time Series Insights'ın temelinde zaman serisi verilerine göre tasarlanmış bir veritabanı bulunur.  Time Series Insights ölçeklenebilir ve tamamen yönetilebilir olduğundan olay depolama ve yönetme süreçlerini üstlenir.

- Neredeyse gerçek zamanlı veri keşfi.  
  - Time Series Insights, bir ortama akan tüm verileri görselleştiren bir gezgin sağlar.  Bir olay kaynağına bağlandıktan hemen sonra olay verilerini Time Series Insights içinde görüntüleyebilir, keşfedebilir ve sorgulayabilirsiniz.  Veriler cihazın beklenen verileri iletip iletmediğini doğrulamanın yanı sıra bir IoT varlığını sistem durumu, üretkenlik ve genel verimlilik açılarından izleme konusunda kullanışlıdır.  

- Kök neden analizi ve anormallik algılama.
  - Time Series Insights, çok adımlı kök neden analizi gerçekleştirmek ve kaydetmek için desenler ve perspektif görünümleri gibi araçlara sahiptir.  Time Series Insights ayrıca Azure Stream Analytics gibi uyarı hizmetleriyle birlikte çalışarak uyarıların ve algılanan anormalliklerin Time Series Insights gezgininin içinde neredeyse gerçek zamanlı olarak görüntülenmesini sağlar.  

- Çoklu varlık/site karşılaştırması için ayrı konumlardan gelen zaman serisi veri akışının genel görünümü.
  - Bir Time Series Insights ortamına birden fazla olay kaynağı bağlayabilirsiniz.  Bu da birden fazla ve birbirinden ayrı konumlardaki veri akışlarının neredeyse gerçek zamanlı olarak birlikte görüntülenebileceği anlamına gelir.  Kullanıcılar bu görünürlükten faydalanarak iş liderleriyle veri paylaşabilir ve sorunların çözülmesi, en iyi yöntemlerin uygulanması ve öğrenilenlerin paylaşılması konusunda yardımcı olabilecek alan uzmanlarıyla daha iyi bir işbirliği yapılmasını sağlayabilir.

- Time Series Insights'ı temel alan bir müşteri uygulaması derleme. 
  - Time Series Insights, REST Sorgu API'leri sunarak zaman serisi verilerini kullanan uygulamalar derlemenizi sağlar.

## <a name="capabilities"></a>Özellikler

- **Hemen kullanmaya başlayın:** Azure Time Series Insights verilerin önceden hazırlanmasını gerektirmez. Azure IoT Hub'ınızdaki veya Event Hub'ındaki milyonlarca olaya dakikalar içinde bağlanın. Bağlandıktan sonra, IoT çözümlerinizi hızla doğrulamak için sensör verilerini görselleştirin ve verilerinizle etkileşim kurmaya hemen başlayın. Kod yazmadan verilerinizle etkileşim kurabilirsiniz.
Yeni dil öğrenmek gerekmez; Zaman Serisi Görüşleri ileri düzey kullanıcılara ayrıntılı, serbest metin kullanılan bir sorgu yüzeyi ve üzerine gelip tıklamayla ulaşılan açıklamalar sağlar.
- **Neredeyse gerçek zamanlı görüşler:** Zaman Serisi Görüşleri her gün, bir dakikalık bir gecikme süresiyle milyonlarca sensör olayını alabilir. Time Series Insights, eğilimleri ve anormallikleri saptamanıza, kök-neden analizleri yürütmenize ve masraflı sistem kapatma sürelerini önlemenize yardımcı olarak sensör verileriniz üzerinde içgörüler kazanmanıza katkıda bulunur. Gerçek zamanlı verilerle geçmiş verileri arasında çapraz bağıntıya olanak tanıdığından, Zaman Serisi Görüşleri verilerindeki gizli eğilimleri ortaya çıkarmanıza yardımcı olur.
- **Özel çözümler oluşturma:** Azure Time Series Insights verilerini mevcut uygulamalarınıza ekleyin ya da Time Series Insights REST API’leriyle yeni özel çözümler oluşturun. Diğer kişilerin de içgörülerinizi incelemesini sağlamak için paylaşabileceğiniz kişiselleştirilmiş görünümler oluşturun.
- **Ölçeklenebilirlik:** Zaman Serisi Görüşleri, ölçekli olarak IoT'yi destekleyecek şekilde tasarlanmıştır. Günde 1 milyon ile 100 milyon arası olay alabilir ve varsayılan saklama süresi 31 gündür. Geçmiş verilerine ek olarak canlı veri akışlarını da neredeyse gerçek zamanlı olarak görselleştirebilir ve analiz edebilirsiniz. Kurumsal ölçekle başa çıkmak için ilerletme, giriş ve saklama hızları artırılacaktır.

## <a name="getting-started"></a>Başlarken
5 dakikadan daha kısa bir sürede kullanmaya başlayabilirsiniz. 

1.  Başlamak için Azure portalında Time Series Insights ortamı sağlayın. 
2.  Azure IoT Hub veya Event Hub gibi bir etkinlik kaynağına bağlanın.  
3.  Başvuru verilerini yükleyin (bu ek hizmet değildir).
4.  Time Series Insights gezgininde verilerinizi dakikalar halinde görün.

## <a name="time-series-insights-explorer"></a>Time Series Insights gezgini
Bu diyagramda ![Time Series Insights gezgininde](media/time-series-insights-explorer/explorer4.png) görüntülenen zaman serisi içgörü verileri örneği gösterilmektedir

## <a name="next-steps"></a>Sonraki adımlar
 - [Bir tanıtım ortamında Time Series Insights gezginini kullanmayı keşfedin](./time-series-quickstart.md)
 - [Kendi Time Series Insights ortamınızı planlayın](time-series-insights-environment-planning.md)

