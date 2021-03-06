---
title: Nasıl desenlerini tahmin doğruluğunu artırmak öğrenin | Microsoft Docs
titleSuffix: Azure
description: Hedefi tahmin puanları artırmak ve varlıkları bulmak için desenler tasarlama konularını öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry
ms.openlocfilehash: c08419e3fb5b25284121a0eac30c38c8ba7570f1
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225226"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenlerini tahmin doğruluğunu artırmak
Desenler, çeşitli konuşma çok benzer olduğunda doğruluğunu artırmak için tasarlanmıştır. LUIS, utterance için bir desen sağlayarak tahmine yüksek güvenilirliğe olabilir. 

## <a name="patterns-solve-low-intent-confidence"></a>Düşük hedefi güvenle desenleri çözün
Kuruluş şeması ile ilgili olarak çalışan bir rapor bir insan kaynakları uygulamasında göz önünde bulundurun. Bir çalışanın adı ve ilişki LUIS çalışanlar ilgili döndürür. Tom, çalışanın sahip bir yönetici adı Alice ve adlı Astları ekibi dikkate alın: Michael Rebecca ve Carl.

![Kuruluş Şeması görüntüsü](./media/luis-concept-patterns/org-chart.png)

|Konuşmalar|Tahmin hedefi|Intent puanı|
|--|--|--|
|Tom'ın alt kimdir?|GetOrgChart|.30|
|Tom alt kimdir?|GetOrgChart|.30|

LUIS, bir uygulamanın farklı uzunluktaki cümle, farklı sözcük sırasını ve hatta farklı sözcük (eş anlamlılar "alt", "manage", "rapor") ile 10 ile 20 konuşma arasında varsa, düşük güvenilirlik puanı döndürebilir. LUIS sözcük sırasını önemini anlamanıza yardımcı olması için bir desen oluşturun. 

Desen aşağıdaki durumlarda çözer: 

* Intent puanı düşük olduğunda
* Ne zaman doğru amaç en çok puan değil, ancak üst puanına çok yakın. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler hedefi garantisi değildir.
Desenlerini tahmin teknolojilerinin bir karışımını kullanın. Bir desende bir amaç için bir şablon utterance ayarı hedefi tahmin garantisi değil ancak güçlü bir sinyaldir. 

## <a name="patterns-do-not-improve-entity-detection"></a>Desenler varlık algılama geliştirmek değil
Varlıklar desenleri gerektirir, ancak bir düzeni varlık algılamak yardımcı olmaz. Bir desen, yalnızca hedefleri ve roller ile tahmini için tasarlanmıştır.  

## <a name="patterns-use-entity-roles"></a>Varlık rolleri desenleri kullanın
İki veya daha fazla varlık desenindeki bağlamsal ilişkiliyse, varlık desenleri kullanın [rolleri](luis-concept-roles.md) varlıklarla ilgili bağlamsal bilgi ayıklamak için. Bu hiyerarşik varlık alt öğelere eşdeğerdir, ancak **yalnızca** desenleri kullanılabilir. 

## <a name="prediction-scores-with-and-without-patterns"></a>Desenler olmadan ve tahmin puanları
Yeterli örnek konuşma göz önünde bulundurulduğunda, LUIS tahmin olasılık desensiz imkanımız olacaktır. Desen çok konuşma sağlamaya gerek kalmadan güvenilirlik puanı artırın.  

## <a name="pattern-matching"></a>Desen eşleştirme
Bir düzeni desen içinde varlıkları ilk algılama ve ardından bir kelimelerin rest ve sözcük sırasını deseninin doğrulama göre eşleştirilir. Varlıklar, deseni eşleştirmek için bir desen için gereklidir. 

## <a name="pattern-syntax"></a>Desen sözdizimi
Desen, bir şablon için bir utterance sözdizimidir. Şablon, sözcük ve sözcükler yanı sıra eşleştirmek istediğiniz varlıkları ve yok saymak için istediğiniz noktalama içermelidir. Bu **değil** normal bir ifade. 

Varlıkları desenleri süslü ayraç tarafından çevrilmiş `{}`. Desenler, varlıkları ve varlıklar rolleriyle içerebilir. Pattern.Any yalnızca desenlerinde kullanılan bir varlıktır. Söz dizimi aşağıdaki bölümlerde açıklanmıştır.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Bir varlık için bir desen şablonu eklemek için söz dizimi
Surround gibi küme ayracı ile varlık adı deseni şablona bir varlık eklemek için `Who does {Employee} manage?`. 

```
Who does {Employee} manage?
```

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Bir varlık ve rol için bir desen şablonu eklemek için söz dizimi
Bir varlık rolü olarak gösterilir `{entity:role}` ile bir iki nokta üst üste ve ardından rol adı tarafından izlenen varlık adı. Desen şablona bir role sahip bir varlık eklemek için varlık adı ve küme ayracı ile rol adı gibi çevreleyen `Book a ticket from {Location:Origin} to {Location:Destination}`. 

```
Book a ticket from {Location:Origin} to {Location:Destination}
```

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Bir pattern.any deseni şablonuna eklemek için söz dizimi
Pattern.any varlık varlığın değişen uzunluktaki desen eklemenizi sağlar. Düzen şablonu ve ardından sürece pattern.any herhangi bir uzunlukta olabilir. 

Eklemek için bir **Pattern.any** deseni şablon varlığa saran ve küme ayraçlarının Pattern.any varlıkla gibi `How much does {Booktitle} cost and what format is it available in?`.  

```
How much does {Booktitle} cost and what format is it available in?
```

|Deseninde bir kitap adları|
|--|
|Ne kadardır **kitabın çalabilir** maliyet ve hangi biçimde, kullanılabilir?|
|Ne kadardır **isteyin** maliyet ve hangi biçimde, kullanılabilir?|
|Ne kadardır **gece zamanlı Mandal, merak olay** maliyet ve hangi biçimde, kullanılabilir?| 

Bu kitap başlık örneklerde kitap başlığın bağlamsal sözcükleri LUIS için kafa karıştırıcı değildir. LUIS içindeki bir desenle olduğundan ve bir Pattern.any varlığı ile işaretlenmiş kitap başlığı sona ereceği bilir.

### <a name="explicit-lists"></a>Açık listeler
Desen bir Pattern.any içeriyorsa ve desen sözdizimi için olasılığını sağlar yanlış varlık ayıklama tabanlı utterance üzerinde oluşturun bir [açık listesi](https://aka.ms/ExplicitList) özel durumuna izin yazma API aracılığıyla. 

Örneğin, hem isteğe bağlı söz dizimi içeren bir düzeni olduğunu varsayalım `[]`ve varlık sözdizimi `{}`, birleştirilmiş bir şekilde hatalı verileri ayıklamak için.

Desen '[Bul] e-postadan hakkında {subject} [{person}]'. Aşağıdaki konuşma içinde **konu** ve **kişi** varlık doğru ve hatalı bir şekilde ayıklanır:

|Konuşma|Varlık|Doğru ayıklama|
|--|--|:--:|
|Chris köpekler hakkında e-posta|Konu köpekler =<br>kişi Chris =|✔|
|ADAM La Mancha gelen e-posta|Konu man =<br>kişi La Mancha =|X|

Önceki tabloda, utterance `email about the man from La Mancha`, konu olmalıdır `the man from La Mancha` (bir kitap başlığı) ancak konu isteğe bağlı sözcüğünü içerdiğinden `from`, başlığı yanlış tahmin edildiğinde. 

Bu özel durumun desen için düzeltmek için ekleme `the man from la mancha` {subject} varlık kullanan bir açık listesi eşleşme olarak [açık bir listesi için API geliştirme](https://aka.ms/ExplicitList).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>İsteğe bağlı bir metin şablonu utterance olarak işaretlemek için söz dizimi
İsteğe bağlı normal ifade köşeli parantez sözdizimini kullanarak utterance metni işaretleme `[]`. İsteğe bağlı bir metin, köşeli ayraç yalnızca iki ayraçlar en fazla iç içe yerleştirebilirsiniz.

```
[find] email about {subject} [from {person}]
```

Noktalama işaretleri gibi `.`, `!`, ve `?` köşeli ayraç kullanarak göz ardı edilebilir. Bu işaretler yoksaymak için her işareti ayrı bir desende olması gerekir. İsteğe bağlı söz dizimi, çeşitli öğeleri listesinde bir öğe yok sayılıyor desteklemiyor.

## <a name="patterns-only"></a>Yalnızca desenleri
LUIS, bir uygulamanın tüm örnek konuşma olmadan amaca sağlar. Bu kullanım desenleri kullanılırsa izin verilir. Desenler, en az bir varlık her desende gerektirir. Bu örnek konuşma gerektirdiğinden deseni-yalnızca uygulama için makine öğrenilen varlıklar deseni içermemelidir. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğreticide desenlerini uygulama hakkında bilgi edinin](luis-tutorial-pattern.md)