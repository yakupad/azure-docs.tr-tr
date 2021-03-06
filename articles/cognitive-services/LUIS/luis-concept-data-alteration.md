---
title: LUIS - Azure veri değişikliğinin kavramları anlama | Microsoft Docs
description: Language Understanding (LUIS) Öngörüler önce verileri nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/26/2018
ms.author: diberry
ms.openlocfilehash: d8421114bb5a7416ad2523fe9b0353f03f672619
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223992"
---
# <a name="data-alterations"></a>Veri değişiklikleri
LUIS, öncesinde veya sırasında tahmin utterance işlemek için yöntemler sağlar. 

## <a name="correct-spelling-errors-in-utterance"></a>Utterance yazarken yazım hataları
LUIS kullanan [Bing yazım denetimi API'si V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) utterance yazım hatalarını düzeltmek için. LUIS, hizmetle ilişkili anahtar gerekir. Anahtar oluşturun, sonra anahtarın bir sorgu dizesi parametresi olarak ekleyin [uç nokta](https://aka.ms/luis-endpoint-apis). 

Yazım hatalarını da düzeltebilirsiniz **Test** tarafından panelinde [anahtarı girme](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). Anahtar, bir Test paneli tarayıcı oturumu değişken olarak tutulur. Anahtar Test panele düzeltildi yazım istediğiniz her bir tarayıcı oturumunda ekleyin. 

Test paneli ve uç nokta sayısı doğru anahtar kullanımını [anahtar kullanımı](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) kota. LUIS, metin uzunluğu sınırlarını Bing yazım denetimi uygular. 

Uç nokta için yazım düzeltmeleri çalışmak iki params gerektirir:

|param|Değer|
|--|--|
|`spellCheck`|boole|
|`bing-spell-check-subscription-key`|[Bing yazım denetimi API'si V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) uç noktası anahtarı|

Zaman [Bing yazım denetimi API'si V7](https://azure.microsoft.com/services/cognitive-services/spell-check/) bir hata, özgün utterance ve düzeltilmiş utterance döndürülür tahminlerinin yanı sıra uç noktasından algılar.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Önceden oluşturulmuş datetimeV2 varlık saat dilimini değiştirme
Bir LUIS uygulaması önceden oluşturulmuş datetimeV2 varlık kullandığında, bir datetime değeri tahmin yanıtta döndürülebilir. Saat dilimi isteğin döndürmek için doğru datetime belirlemek için kullanılır. İstek bir bot veya alma için LUIS önce başka bir merkezi uygulamasından geliyorsa LUIS kullanır saat dilimi düzeltin. 

### <a name="endpoint-querystring-parameter"></a>Uç nokta querystring parametresi
Kullanıcının saat dilimine ekleyerek saat dilimi düzeltilene [uç nokta](https://aka.ms/luis-endpoint-apis) kullanarak `timezoneOffset` param. Değerini `timezoneOffset` saati değiştirmek için dakikalar içinde pozitif veya negatif sayı olmalıdır.  

|param|Değer|
|--|--|
|`timezoneOffset`|dakikalar içinde pozitif veya negatif sayı|

### <a name="daylight-savings-example"></a>Gün ışığından tasarruf örneği
Gün ışığından yararlanma saatine ayarlamak için döndürülen önceden oluşturulmuş datetimeV2 gerekiyorsa kullanmalısınız `timezoneOffset` querystring parametresi ile bir değer dakika cinsinden +/- [uç nokta](https://aka.ms/luis-endpoint-apis) sorgu.

60 dakika ekleyin: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appId}?q=Turn ışıkları Aç? **timezoneOffset = 60**& ayrıntılı {boolean} = & Yazım denetimi {boolean} = & hazırlama {boolean} = & bing-yazım-onay-subscription-key = {string} gün & lüğü {boolean} =

60 dakika kaldırın: 

https://{Region}.api.cognitive.microsoft.com/luis/v2.0/Apps/{appId}?q=Turn ışıkları Aç? **timezoneOffset = 60**& ayrıntılı {boolean} = & Yazım denetimi {boolean} = & hazırlama {boolean} = & bing-yazım-onay-subscription-key = {string} gün & lüğü {boolean} =

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C# kodu timezoneOffset doğru değerini belirler.
Aşağıdaki C# kod [Timezoneınfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo?view=netframework-4.7.1) sınıfın [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid?view=netframework-4.7.1#examples) doğru belirlemek için yöntemi `timezoneOffset` sistem saatini temel alan:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğretici ile doğru yazım hatalarını](luis-tutorial-bing-spellcheck.md)