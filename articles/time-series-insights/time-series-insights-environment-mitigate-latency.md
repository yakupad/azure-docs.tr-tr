---
title: İzleme ve Azure zaman serisi öngörü azaltma azaltmak nasıl | Microsoft Docs
description: Bu makalede, izleme, tanılama ve gecikme süresi ve azaltma Azure zaman serisi öngörü neden performans sorunlarını azaltmak açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: jasonh
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/27/2017
ms.openlocfilehash: 35860838d03d61e1145d35fd2516c1688c3bb64f
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130589"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>İzleme ve Azure zaman serisi Öngörüler gecikmesini azaltmak için azaltma azaltmak
Gelen veri miktarına, ortam yapılandırma aşarsa, gecikme veya Azure zaman serisi öngörü azaltma karşılaşabilirsiniz.

Gecikme süresi ve analiz etmek istediğiniz veri miktarı, ortamınızı düzgün şekilde yapılandırarak azaltma önleyebilirsiniz.

Gecikme süresi ve ne zaman azaltma deneyimi büyük olasılıkla:

- (Zaman serisi Öngörüler Yakala gerekir), ayrılan giriş hızı aşabilir eski verileri içeren bir olay kaynağı ekleyin.
- Daha fazla olay kaynakları (ortamınızı 's kapasite aşabilir) ek olaylar depodan sonuçta bir ortam ekleyin.
- Büyük miktarda geçmiş olayları (zaman serisi Öngörüler Yakala gerekir) bir gecikme sonuçta bir olay kaynağı iletin.
- Başvuru verileri büyük olay boyutu elde telemetriyle katılın.  Bir azaltma açısından ingressed veri paketi paket boyutu 32 KB'ın 32 olayları olarak kabul edilir, her 1 KB boyuta sahip. İzin verilen en fazla olay boyutu 32 KB'tır; veri paketlerinin 32 KB'den büyük kesilir.


## <a name="monitor-latency-and-throttling-with-alerts"></a>İzleyici gecikme süresi ve uyarılarla azaltma

Uyarılar tanılamak ve ortamınızın neden gecikmesi sorunları azaltmaya yardımcı olmak için yardımcı olabilir. 

1. Azure portalında tıklatın **ölçümleri**. 

   ![Ölçümler](media/environment-mitigate-latency/add-metrics.png)

2. Tıklatın **ölçüm uyarı Ekle**.  

    ![Ölçüm uyarısı ekleme](media/environment-mitigate-latency/add-metric-alert.png)

Buradan, aşağıdaki ölçümleri kullanarak uyarıları yapılandırabilirsiniz:

|Ölçüm  |Açıklama  |
|---------|---------|
|**Giriş alınan bayt miktarı**     | Olay kaynaklardan okuma ham bayt sayısı. Ham sayısı genellikle özellik adı ve değeri içerir.  |  
|**Giriş geçersiz ileti alındı**     | Geçersiz ileti sayısı, tüm Azure Event Hubs ya da Azure IOT Hub olay kaynağından okuyun.      |
|**Giriş alınan iletileri**   | İleti sayısı tüm Event Hubs ya da IOT hub'ları olay kaynağından okuyun.        |
|**Bayt giriş depolanan**     | Toplam boyut depolanan olayların ve sorgu için kullanılabilir. Boyutu yalnızca özellik değeri hesaplanır.        |
|**Giriş olayları depolanan**     |   Sayısı düzleştirilmiş depolanan ve sorgu için kullanılabilir.      |
|**Giriş alınan ileti zaman gecikmesini**    |  İleti sıraya alınan olay olduğu zaman arasında saniye cinsinden kaynak ve giriş işlendiği zaman fark.      |
|**İleti sayısı öteleme giriş alındı**    |  Son sıraya alınan ileti sıra numarası arasındaki farkı olay kaynağı giriş işlenen ileti bölüm ve sıra sayısı.      |


![Gecikme süresi](media/environment-mitigate-latency/latency.png)

Kısıtlanan değilse, için bir değer görür *giriş alınan ileti zaman gecikmesini*, size TSI arkasında kaç saniye gerçek saati ileti mı olduğunu bildiren isabetler (appx dizin oluşturma süresi hariç. olay kaynağı 30-60 saniye).  *Giriş alınan ileti sayısı öteleme* de kaç iletiler, arkasında belirlemenize olanak sağlayan bir değer olmalıdır.  Yakalanan için en kolay yolu, ortamınızın 's kapasite fark üstesinden olanak tanıyan bir boyut artırmaktır.  

Örneğin, bir tek birim S1 ortamınız varsa ve beş milyon ileti öteleme olup olmadığını, yakalanan için altı birimler için günde bir geçici ortamınıza boyutunu arttırabilir.  Hatta catch daha fazla sınırlandıramazsınız yukarı daha hızlı artırabilir.  Yakalama süresi, özellikle bu olayları içinde zaten bir olay kaynağına bağlandığınızda başlangıçta bir ortam sağlamada veya karşıya yükleme çok fazla geçmiş veri toplu sık karşılaştıkları değil.

Ayarlamak için başka bir tekniktir bir **giriş depolanan olayları** uyarı > bir eşiğinin biraz toplam ortamı kapasitenizi 2 saat boyunca =.  Bu uyarı, bir yüksek gecikme olasılığını gösterir kapasitede sürekli olup olmadığını anlamanıza yardımcı olabilir.  

Örneğin, sağlanan üç S1 birimler (veya dakika giriş kapasite başına 2100 olayları) varsa, ayarlayabileceğiniz bir **giriş depolanan olayları** için uyarı > 1900 olayları = için 2 saat. Sürekli olarak bu Eşiği aşan ve bu nedenle, uyarıyı tetikleyen varsa, büyük olasılıkla altında-sağlanır.  

Ayrıca, kısıtlanan şüpheleniyorsanız karşılaştırabileceğiniz, **giriş alınan iletilerin** , olay ile kaynak iletileri egressed.  Olay Hub'ınızı içine giriş büyükse, **giriş alınan iletilerin**, zaman serisinin Öngörülerinizi olasılıkla kısıtlanan.

## <a name="improving-performance"></a>Performansı iyileştirme 
Azaltma veya gecikme yaşıyor azaltmak için düzeltmek için en iyi yolu ortamınızı 's kapasite artırmaktır. 

Gecikme süresi ve analiz etmek istediğiniz veri miktarı, ortamınızı düzgün şekilde yapılandırarak azaltma önleyebilirsiniz. Ortamınız için kapasite ekleme hakkında daha fazla bilgi için bkz: [ortamınızın ölçeğini](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Sonraki adımlar
- Ek sorun giderme adımları için [Tanıla ve zaman serisi Öngörüler ortamınızdaki sorunları](time-series-insights-diagnose-and-solve-problems.md).
- Ek Yardım için bir konuşma başlatmak [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [yığın taşması](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). De başvurabilirsiniz [Azure Destek](https://azure.microsoft.com/support/options/) yardımlı destek seçenekleri için.
