---
title: Desteklenen ülkeler/bölgeler ve diller için Bing görsel arama API'si | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Hangi ülkeler/bölgeler ve diller Bing görsel arama API'si tarafından desteklendiğini öğrenin.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 068459f4959bf3f8daea8fe949ec324879ee260c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001237"
---
# <a name="bing-visual-search-countriesregions-and-languages"></a>Bing görsel arama ülkeler/bölgeler ve diller

Bing görsel arama API'sine birden fazla üç düzine ülkeler/bölgeler, çoğu birden fazla dili destekler. Her istek, kullanıcının ülke/bölge ve tercih ettiğiniz dilde içermelidir. Kullanıcının Pazar bilerek uygun sonuçları döndürmek Bing yardımcı olur. Bir ülke/bölge ve dil belirtmezseniz, Bing, kullanıcının ülke/bölge ve dil belirlemek için bir en iyi hale getirir. Sonuçları Bing bağlantılar içeriyor olabileceğinden, kullanıcının Bing bağlantıları tıklarsa ülke/bölge ve dil bilerek tercih edilen bir yerelleştirilmiş Bing kullanıcı deneyimini sağlayabilir.

Ülke/Bölge ve dil belirtmek için ayarlayın `mkt` (Pazar) sorgu parametresine bir koddan **pazarlara** aşağıdaki tabloda. Market, bir ülke/bölge ve dil belirtir. Kullanıcının tercih ettiği görmek, metni farklı bir dilde görüntülemek, ayarlamak `setLang` sorgu parametresi için uygun dil kodu.

Alternatif olarak, ülke/bölge kullanarak belirtebilirsiniz `cc` sorgu parametresi. Bir ülke/bölge belirtirseniz, kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` HTTP üstbilgisi. Desteklenen diller, ülke/bölgeye göre değişiklik gösterir; pazarlara tablodaki her bir ülkede verilir.



> [!NOTE]
> Aşağıdaki market kısıtlamalar uygulanır:
> 
> - Görüntü tanıma ek açıklamalar yalnızca İngilizce dilinde kullanılabilir. 
> - Tarif, alışveriş ve sayfalar dahil ınsights yalnızca en-US pazarında kullanılabilir. 


## <a name="countries"></a>Ülkeler

|Ülke/bölge|Kod|
|-------|----|
|Arjantin|AR|
|Avustralya|Otomatik Olarak Güncelleştir|
|Avusturya|AT|
|Belçika|OLABİLİR|
|Brezilya|BR|
|Kanada|CA|
|Şili|CL|
|Danimarka|DK|
|Finlandiya|FI|
|Fransa|GS|
|Almanya|GİZLE|
|Hong Kong|HK|
|Hindistan|GİRİŞ|
|Endonezya|Kimlik|
|İtalya|BT|
|Japonya|JP|
|Kore|KR|
|Malezya|BİLGİSAYARIM|
|Meksika|MX|
|Hollanda|NL|
|Yeni Zelanda|NZ|
|Norveç|HAYIR|
|Çin|CN =|
|Polonya|PL|
|Portekiz|PT|
|Filipinler|PH|
|Rusya|RU|
|Suudi Arabistan|SA|
|Güney Afrika|ZA|
|İspanya|ES|
|İsveç|SE|
|İsviçre|CH|
|Tayvan|TW|
|Türkiye|TR|
|Birleşik Krallık|GB|
|Amerika Birleşik Devletleri|ABD|


## <a name="markets"></a>Pazarlar

|Ülke/bölge|Dil|Pazar kod|
|-------|--------|-----------|
|Arjantin|İspanyolca |ES AR|
|Avustralya|Türkçe|tr-AU|
|Avusturya|Almanca |de-AT|
|Belçika|Felemenkçe|NL-olabilir|
|Belçika|Fransızca |FR-olabilir|
|Brezilya|Portekizce|pt-BR|
|Kanada|Türkçe|CA tr|
|Kanada|Fransızca |fr-CA|
|Şili|İspanyolca |ES-CL|
|Danimarka|Danca|v-DK|
|Finlandiya|Fince|FI-FI|
|Fransa|Fransızca |fr-FR|
|Almanya|Almanca |de-DE|
|Hong Kong|Geleneksel Çince|zh-HK|
|Hindistan|Türkçe|tr-giriş|
|Endonezya|Türkçe|tr kimliği|
|İtalya|İtalyanca|it-IT|
|Japonya|Japonca|ja-JP|
|Kore|Kore dili|ko-KR|
|Malezya|Türkçe|MY tr|
|Meksika|İspanyolca |es-MX|
|Hollanda|Felemenkçe|NL-NL|
|Yeni Zelanda|Türkçe|tr NZ|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|Türkçe|tr PH|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA|
|Güney Afrika|Türkçe|tr-ZA|
|İspanya|İspanyolca |es-ES|
|İsveç|İsveç dili|sv-SE|
|İsviçre|Fransızca |FR-CH|
|İsviçre|Almanca |de-CH|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|İspanyolca |ES-ABD|
