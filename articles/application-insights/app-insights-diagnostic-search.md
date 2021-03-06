---
title: Azure Application Insights arama'yı kullanarak | Microsoft Docs
description: Web uygulamanız tarafından gönderilen arama ve filtre ham telemetri.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 2a437555-8043-45ec-937a-225c9bf0066b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: mbullwin
ms.openlocfilehash: 1a343e238662393995404b8e4c705cf799866855
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39137384"
---
# <a name="using-search-in-application-insights"></a>Uygulama anlayışları'nda arama kullanma
Arama özelliği, [Application Insights](app-insights-overview.md) bulmak ve sayfa görüntülemeleri, özel durumlar gibi tek bir telemetri öğelerini keşfedin veya web istekleri için kullanın. Ve günlük izlemeleri ve kodlanmış olayları görüntüleyebilir.

(Verileriniz üzerinde daha karmaşık sorgular için [Analytics](app-insights-analytics-tour.md).)

## <a name="where-do-you-see-search"></a>Burada arama görüyor musunuz?

### <a name="in-the-azure-portal"></a>Azure portalında

Tanılama araması uygulamanızı Application Insights'a genel bakış dikey penceresinden açıkça açabilirsiniz:

![Tanılama Aramayı Aç](./media/app-insights-diagnostic-search/001.png)

![Tanılama araması grafiklerin ekran görüntüsü](./media/app-insights-diagnostic-search/002.png)

Tanılama araması ana gövdesi bir listedir telemetri öğelerinin - sunucu istekleri, görünümler, kodlanmış özel olaylar ve benzeri sayfa. Listenin en üstünde zaman içinde olay sayısını gösteren Özet bir grafiktir.

Yeni olaylar almak için Yenile'ye tıklayın.

### <a name="in-visual-studio"></a>Visual Studio’da

Visual Studio'da da Application Insights arama penceresi yok. Ayıkladığınız uygulama tarafından üretilen telemetri olayları görüntülemek için en kullanışlıdır. Ancak, Azure portalında yayımlanmış uygulamanızdan toplanan olayları da gösterebilirsiniz.

Arama penceresi Visual Studio'da açın:

![Visual Studio Application Insights arama açın](./media/app-insights-diagnostic-search/32.png)

Arama penceresi, web Portalı'na benzer özelliklere sahiptir:

![Visual Studio Application Insights arama penceresi](./media/app-insights-diagnostic-search/34.png)

İşlemi İzle sekmesi, bir istek veya sayfa görünümü açtığınızda kullanılabilir. Bir'işlemi ' olduğu için tek bir istek veya sayfa görüntüleme ile ilişkili bir olay dizisi. Örneğin, bağımlılık çağrıları, özel durumlar, izleme günlükleri ve özel olaylar tek bir işlemin parçası olabilir. İşlemi İzle sekmesi zamanlama ve istek veya sayfa görüntüleme ile ilgili olarak bu olayların süresi grafik şeklinde gösterir. 

## <a name="inspect-individual-items"></a>Bireysel öğeleri inceleyin

Anahtar alanları görmek için herhangi bir telemetri öğesinin ve ilgili öğeleri seçin.

![Bir bireysel bağımlılığı isteğinin ekran görüntüsü](./media/app-insights-diagnostic-search/003.png)

Bu uçtan uca işlem Ayrıntıları görünümü başlatılır:

![Uçtan uca işlem ayrıntıları görünümünün ekran görüntüsü.](./media/app-insights-diagnostic-search/004.png)

## <a name="filter-event-types"></a>Olay türlerini Filtrele
Filtre dikey penceresini açın ve görmek istediğiniz olay türlerini seçin. (Daha sonra dikey pencere açılır filtreler geri yüklemek istiyorsanız Sıfırla'ya tıklayın.)

![Filtreyi seçin ve telemetri türlerini seçin](./media/app-insights-diagnostic-search/02-filter-req.png)

Olay türleri şunlardır:

* **İzleme** - [tanılama günlükleri](app-insights-asp-net-trace-logs.md) TrackTrace, log4Net, NLog ve System.Diagnostic.Trace çağrılar dahil olmak üzere.
* **İstek** -sayfaları, betikleri, görüntüler, Stil dosyaları ve verileri dahil olmak üzere sunucu uygulamanız tarafından alınan HTTP istekleri. Bu olaylar, istek ve yanıt genel bakış grafiklerinde oluşturmak için kullanılır.
* **Sayfa görünümü** - [Telemetri web istemcisi tarafından gönderilen](app-insights-javascript.md)sayfa görünümü raporları oluşturmak için kullanılır. 
* **Özel olay** - için TrackEvent() çağrıları eklediyseniz [kullanım izleme](app-insights-api-custom-events-metrics.md), burada bulabilirsiniz.
* **Özel durum** - yakalanmayan [sunucu özel durumları](app-insights-asp-net-exceptions.md)hem de TrackException() kullanarak oturum açın.
* **Bağımlılık** - [sunucu uygulamanızı çağrılarından](app-insights-asp-net-dependencies.md) REST API'leri veya veritabanları ve AJAX gibi diğer hizmetlere çağırır, [istemci kodu](app-insights-javascript.md).
* **Kullanılabilirlik** -sonuçlarını [kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md).

## <a name="filter-on-property-values"></a>Özellik değerlerine göre filtreleme
Olayları değerlerine özelliklerine göre filtreleyebilirsiniz. Kullanılabilir özellikler, seçtiğiniz olay türlerine bağlıdır. 

Örneğin, istekleri belirli bir yanıtı koduyla çıkış seçin. 

![Bir özelliği'ni genişletin ve bir değer seçin](./media/app-insights-diagnostic-search/03-response500.png)

Belirli bir özellik yok değerlerini seçerek tüm değerleri seçmeyi aynı etkiye sahiptir. Bu, söz konusu özellik üzerinde filtreleme devre dışı geçer.

### <a name="narrow-your-search"></a>Aramanızı daraltın
Filtre değerleri sağındaki sayıları kaç örnekleri var. geçerli filtrelenmiş kümededir Göster dikkat edin. 

Bu örnekte, 'Rpt/çalışanlar' Sonuçları '500' hataların çoğu istek açıktır:

![Bir özelliği'ni genişletin ve bir değer seçin](./media/app-insights-diagnostic-search/04-failingReq.png)

## <a name="find-events-with-the-same-property"></a>Aynı özellik ile olayları bulun
Aynı özellik değerine sahip tüm öğeleri bulun:

![Bir özelliğe sağ tıklayın](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-the-data"></a>Veri arama

> [!NOTE]
> Daha karmaşık sorgular yazmak için açık [ **Analytics** ](app-insights-analytics-tour.md) arama dikey pencerenin üst.
> 

Herhangi bir özellik değeri terimlerini arayabilirsiniz. Bu yazdığınız, özellikle yararlı olacaktır [özel olaylar](app-insights-api-custom-events-metrics.md) özellik değerlerine sahip. 

Aralık, daha kısa bir aralık içinde aramalar olarak daha hızlı bir zaman ayarlamak isteyebilirsiniz. 

![Tanılama Aramayı Aç](./media/app-insights-diagnostic-search/appinsights-311search.png)

Arama için tam sözcük, alt dizeler değil. Özel karakterleri için tırnak işaretleri kullanın.

| dize | olan *değil* tarafından bulunamadı | ancak bunlar Bul |
| --- | --- | --- |
| HomeController.About |giriş sayfası<br/>Denetleyici<br/>Çıkış | homecontroller<br/>hakkında<br/>"homecontroller.about"|
|Amerika Birleşik Devletleri|UNI<br/>ted|Birleşik<br/>durumları<br/>VE Birleşik Devletleri<br/>"ABD"

Kullanabileceğiniz arama ifadeleri şunlardır:

| Örnek sorgu | Etki |
| --- | --- |
| `apple` |Alanları "apple" sözcüğünü içeren bir zaman aralığında tüm etkinlikler bulun |
| `apple AND banana` |Her iki sözcükleri içeren etkinlikler bulun. Capital "ve" değil kullan "ve". |
| `apple OR banana`<br/>`apple banana` |Her iki word içeren etkinlikler bulun. "Veya", yok "veya".<br/>Kısa biçim. |
| `apple NOT banana` |Bir sözcük, ancak diğer içeren etkinlikler bulun. |

## <a name="sampling"></a>Örnekleme
Uygulamanız çok sayıda telemetri oluşturuyorsa (ve ASP.NET SDK sürüm 2.0.0-Beta3 veya daha sonra), Uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Ancak aynı istekle ilgili olayları seçtikten veya bir grup olarak seçili, böylece ilgili olaylar arasında gezinebilirsiniz. 

[Örnekleme hakkında bilgi edinin](app-insights-sampling.md).

## <a name="create-work-item"></a>İş öğesi oluştur
Tüm telemetri öğesinin Ayrıntıları ile GitHub veya Visual Studio Team Services içinde bir hata oluşturabilirsiniz. 

![Yeni iş öğesini, alanları düzenleyin ve sonra Tamam'a tıklayın.](./media/app-insights-diagnostic-search/42.png)

Project ve Team Services hesabı bağlantısını yapılandırmak için bunu ilk kez istenir.

![Team Services sunucunuza ve proje adı URL'sini girin ve Yetkilendir'e tıklayın](./media/app-insights-diagnostic-search/41.png)

(Ayrıca bağlantı iş öğeleri dikey penceresinde yapılandırabilirsiniz.)

## <a name="send-more-telemetry-to-application-insights"></a>Daha fazla telemetri Application Insights'a gönderme
Application Insights SDK'sı tarafından gönderilen kullanıma hazır telemetriyi ek olarak, şunları yapabilirsiniz:

* İçinde sık kullandığınız günlük çerçeveden günlük izlemelerini yakalama [.NET](app-insights-asp-net-trace-logs.md) veya [Java](app-insights-java-trace-logs.md). Bunun anlamı, günlük izlemelerini aramak ve sayfa görüntülemeleri, özel durumlar ve diğer olaylarla ilişkilendirin. 
* [Kod yazma](app-insights-api-custom-events-metrics.md) özel olaylar, sayfa görüntülemeleri ve özel durumlar göndermek için. 

[Günlükleri ve özel telemetri, Application Insights'a gönderme öğrenin](app-insights-asp-net-trace-logs.md).

## <a name="questions"></a>SORU- CEVAP
### <a name="limits"></a>Ne kadar veri tutulur?

Bkz: [sınırları özeti](app-insights-pricing.md#limits-summary).

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Gönderme verisi sunucu isteklerim nasıl görebilirim?
Gönderme verisi otomatik olarak oturum yok, ancak kullanabileceğiniz [TrackTrace ya da günlük aramaları](app-insights-asp-net-trace-logs.md). Gönderme verisi ileti parametreyi yerleştirin. İletide, özelliklerde filtre aynı şekilde filtre uygulanamıyor, ancak boyut sınırını uzundur.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="add"></a>Sonraki adımlar
* [Karmaşık sorgular Analytics'te yazma](app-insights-analytics-tour.md)
* [Günlükleri ve özel telemetri, Application Insights'a gönderme](app-insights-asp-net-trace-logs.md)
* [Kullanılabilirlik ve yanıt hızını testleri ayarlama](app-insights-monitor-web-app-availability.md)
* [Sorun giderme](app-insights-troubleshoot-faq.md)
