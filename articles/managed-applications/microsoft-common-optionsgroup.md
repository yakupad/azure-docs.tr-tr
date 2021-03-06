---
title: Azure OptionsGroup UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Common.OptionsGroup kullanıcı Arabirimi öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: e51061dc781e4ec6e822cde9cc450887ff3b1368
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061194"
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup UI öğesi
Seçim denetim kullanılabilir seçeneklerden bir satır.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Etiketi `constraints.allowedValues` öğenin görünen metindir ve değerini öğesi seçildiğinde çıktı değeri.
- Belirtilmişse, varsayılan değer mevcut bir etiketi olmalıdır `constraints.allowedValues`. Belirtilmezse, listedeki ilk öğe `constraints.allowedValues` varsayılan olarak seçilidir. Varsayılan değer **null**.
- `constraints.allowedValues` en az bir öğe olmalıdır.

## <a name="sample-output"></a>Örnek çıktı
```json
"two"
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
