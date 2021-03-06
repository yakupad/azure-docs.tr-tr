---
title: Language Understanding (LUIS) sınırları içinde Node.js ile LUIS bölge bulma | Microsoft Docs
description: Bulma uç noktası anahtarı ve uygulama ile bölgeye yayımlama program aracılığıyla LUIS kimliği.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/01/2018
ms.author: diberry
ms.openlocfilehash: 66a34b5b611934ca531e7709b13e0c6d1c794206
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222520"
---
# <a name="region-can-be-determined-from-api-call"></a>Bölge belirlenebilir API çağrısından 
LUIS uygulama kimliği ve LUIS abonelik kimliği varsa, hangi bölge için uç nokta sorgular bulabilirsiniz.

> [!NOTE] 
> Eksiksiz Node.js çözümünü kullanılabilir [ **LUIS-Samples** Github deposu](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/nodejs/).

## <a name="luis-endpoint-query-strategy"></a>LUIS uç nokta sorgu stratejisi
Her LUIS uç nokta sorgu gerektirir:

* Bir uç noktası anahtarı
* Uygulama Kimliği
* Bir bölge

LUIS uç nokta sorgu doğru uç noktası anahtarı ve uygulama kimliği, yanlış bölgeye kullanıyorsa, yanıt kodu 401 ' dir. Abonelik kotasında 401 istek katılmaz. Bu istek doğru bölgeyi bulmak için tüm bölgeler yoklamak için bir strateji açın. Doğru 2xx durum kodu döndüren yalnızca istek bölgedir. 

|Yanıt kodu|Parametreler|
|--|--|
|2xx|doğru uç noktası anahtarı<br>doğru uygulama kimliği<br>doğru ana bölge|
|401|doğru uç noktası anahtarı<br>doğru uygulama kimliği<br>_yanlış_ ana bölge|

## <a name="nodejs-code-to-find-region"></a>Bölge bulmak için Node.js kodu
Konsol uygulaması LUIS uygulama kimliği ve uç noktası anahtarı alır ve onunla ilişkili tüm bölgeler döndürür. Bir uç noktası anahtarı bölgeye göre şu anda, oluşturulan bu nedenle yalnızca tek bir bölge döndürmelidir.

NPM bağımlılıkları şunlardır:

[!code-javascript[Add the dependencies](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=5-6 "Add the dependencies")]

Sabitleri ekleyin. Değişken değerleri değiştirin `subscriptionKey` ve `appId` kendi değerlerinizle.  

[!code-javascript[Add constants](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=8-25 "Add constants")]

Ekleme `searchRegions` bölgeyi bulmak için işlevi. Tüm yanlış bölgeler, yakalanan ve göz ardı 401, döndürür.

[!code-javascript[Find region](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=27-37 "Find region")]

Çağrı `searchRegions` işlev ve tek bölge döndürür:

[!code-javascript[Call the function](~/samples-luis/documentation-samples/find-region/nodejs/index.js?range=39-43 "Call the function")]

Uygulamayı çalıştırdığınızda, terminal uygulama kimliği için bölgenizi görebilirsiniz.

![Konsol uygulamasının LUIS bölge gösteren ekran görüntüsü](./media/find-region-nodejs/console.png)


## <a name="next-steps"></a>Sonraki adımlar

LUIS hakkında daha fazla bilgi [bölgeleri](luis-reference-regions.md).
