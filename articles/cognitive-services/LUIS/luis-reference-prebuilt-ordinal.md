---
title: LUIS önceden oluşturulmuş varlıklar sıralı başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makale, Language Understanding (LUIS) önceden oluşturulmuş sıralı varlık bilgileri içerir.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 216114ec521e2065cb13cd39b4086f50ec81ba56
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236763"
---
# <a name="ordinal-entity"></a>Sıralı varlık
Sıra numarası olduğu bir nesne kümesi içinde sayısal bir gösterimi: `first`, `second`, `third`. Bu varlık zaten eğitildi çünkü uygulama hedefleri için sıralı içeren örnek Konuşma ekleme gerekmez. Sıralı varlık içerisinde desteklendiği [çok kültür](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Sıra türü
Sıra yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) Github deposu

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Önceden oluşturulmuş sıralı varlık için çözümleme
Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.ordinal** varlık.

```JSON
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md), ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıklar. 