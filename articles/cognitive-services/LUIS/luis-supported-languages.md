---
title: Azure'da LUIS uygulamaları kullanmaya yerelleştirmeyi destekleme | Microsoft Docs
description: LUIS destekleyen diller hakkında bilgi edinin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2017
ms.author: diberry
ms.openlocfilehash: d2c479445aabe05013470724c623978402abeb9d
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248426"
---
# <a name="culture-specific-understanding-in-luis-apps"></a>LUIS uygulamalarında kültüre özgü anlama

Bir LUIS uygulaması kültüre özgü olan ve ayarlandıktan sonra değiştirilemez. 

## <a name="multi-language-luis-apps"></a>Çok dilli LUIS uygulamaları
Çok dilli LUIS istemci uygulama bir sohbet Robotu gibi gerekiyorsa, birkaç seçeneğiniz vardır. LUIS, tüm diller destekliyorsa, her dil için bir LUIS uygulaması geliştirin. Her LUIS uygulamanın benzersiz uygulama kimliği ve uç nokta günlük vardır. Language Understanding LUIS desteklemez, bir dil için kullanabileceğiniz sağlamak gerekiyorsa [Microsoft Translator API'si](../Translator/translator-info-overview.md) utterance desteklenen bir dile çevirmek için utterance LUIS uç noktasına gönderme ve alma Sonuçta elde edilen puanları.

## <a name="languages-supported"></a>Desteklenen diller
LUIS, konuşma şu dillerde anlar:


| Dil |Yerel ayar  |  Önceden oluşturulmuş etki alanı | Önceden oluşturulmuş varlık | Tümcecik önerileri | **[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) | 
|--|--|:--:|:--:|:--:|:--:|
| Amerikan İngilizcesi |`en-US` | ✔ | ✔  |✔|✔|
| Kanada Fransızcası |`fr-CA` |-|   -   |-|✔|
| *[Çince](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Felemenkçe |`nl-NL` |-|  -   |-|✔|
| Fransızca (Fransa) |`fr-FR` |-| ✔ |✔ |✔|
| Almanca  |`de-DE` |-| ✔ |✔ |✔|
| İtalyanca |`it-IT` |-| ✔ |✔|✔|
| *[Japonca](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Yalnızca anahtar ifade|
| Kore dili |`ko-KR` |-|   -   |-|Yalnızca anahtar ifade|
| Portekizce (Brezilya) |`pt-BR` |-| ✔ |✔ |tüm alt kültürler|
| İspanyolca (İspanya) |`es-ES` |-| ✔ |✔|✔|
| İspanyolca (Meksika)|`es-MX` |-|  -   |✔|✔|


Dil desteği değişir için [önceden oluşturulmuş varlıklarla](luis-reference-prebuilt-entities.md) ve [önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md). 

### <a name="chinese-support-notes"></a>* Çince desteği notları

 - İçinde `zh-cn` kültür LUIS, Basitleştirilmiş Çince karakter yerine geleneksel karakter kümesi kümesini bekliyor.
 - Amacı, varlıkları, özellikler ve normal ifadeler adlarını Çince veya Latin karakter olabilir.
 - Bkz: [önceden oluşturulmuş etki alanları başvuru ](luis-reference-prebuilt-domains.md) üzerinde önceden oluşturulmuş etki alanları desteklenmektedir bilgi `zh-cn` kültür.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>* Japonca desteği notları

 - LUIS söz dizimi analizi sağlamaz ve Keigo resmi olmayan Japonca arasındaki farkı anlamak değil çünkü formality eğitim örnekler uygulamalarınız için farklı düzeylerde birleştirmek gerekir. 
     - でございます です ile aynı değil. 
     - です だ ile aynı değil. 

### <a name="text-analytics-support-notes"></a>** Metin analizi, notları destekler.
Metin analizi, anahtar cümlesi içeren önceden oluşturulmuş varlık ve yaklaşım analizi. Yalnızca Portekizce subcultures için desteklenir: `pt-PT` ve `pt-BR`. Tüm kültürler birincil kültür düzeyinde desteklenir. Metin analizi hakkında daha fazla bilgi [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). 

### <a name="speech-api-supported-languages"></a>Konuşma API'si desteklenen diller
Konuşma bkz [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) konuşma dikte modu diller için.

### <a name="bing-spell-check-supported-languages"></a>Desteklenen Bing yazım denetimi dilleri
Bing yazım denetimi bkz [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) için desteklenen diller ve durum listesi.

## <a name="rare-or-foreign-words-in-an-application"></a>Bir uygulamada nadir veya yabancı kelimeler
İçinde `en-us` kültür LUIS öğrenir argo kullanımlar da dahil olmak üzere, en Türkçe kelimeler ayırt etmek. İçinde `zh-cn` kültür LUIS öğrenir çoğu Çince karakter ayırt etmek. Nadir bir sözcük kullanırsanız `en-us` veya karakter olarak `zh-cn`, ve LUIS gibi görünüyor, word veya karakter ayırt edemez, sözcük ekleyin veya için karakter gördüğünüz bir [tümcecik listesi özelliği](luis-how-to-add-features.md). Örneğin, bir ifade listesi özelliğini sözcükleri--yabancı kelimeler--kültürünü dışında eklenmesi gerekir. Bu ifade listesi, olmayan-birbirinin yerine, nadir bir sözcükler kümesini LUIS tanımayı öğrenin bir sınıf oluşturur, ancak bunlar eş anlamlı sözcükler değildir belirtmek için veya birbirinin yerine birbirleri ile işaretlenmelidir.

### <a name="hybrid-languages"></a>Karma dilleri
Karma dil İngilizce ve Çince gibi iki kültürün sözcük birleştirin. Uygulama tek bir kültürü temel aldığından, bu dillerden LUIS desteklenmez.

## <a name="tokenization"></a>Simgeleştirme
Makine öğrenimi için LUIS bir utterance keser [belirteçleri](luis-glossary.md#token) kültüre göre. 

|Dil|  her alanı ya da özel karakter | karakter düzeyi|Bileşik sözcüklerin|[parçalanmış varlık döndürdü](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Çince||✔||✔|
|Felemenkçe|||✔|✔|
|İngilizce (en-us)|✔ ||||
|Fransızca (fr-FR)|✔||||
|Fransızca (fr-CA)|✔||||
|Almanca |||✔|✔|
|İtalyanca|✔||||
|Japonca||||✔|
|Kore dili||✔||✔|
|Portekizce (Brezilya)|✔||||
|İspanyolca (es-ES)|✔||||
|İspanyolca (es-MX)|✔||||

 