---
title: Standart Kodlayıcı Azure Media Services bit hızı otomatik olarak oluşturulan Merdiveni kullanarak videolarınızı kodlamak için kullanın. | Microsoft Docs
description: Bu konu giriş çözünürlük ve bit hızı dayalı bir otomatik olarak oluşturulan bit hızı Merdiveni ile video standart Kodlayıcı Media Services ile bir giriş kodlamak için nasıl kullanılacağını gösterir. Giriş çözünürlük ve bit hızı hiçbir zaman aşacak. Örneğin, giriş olması durumunda 3 MB/sn, çıktı adresindeki 720p 720 p en iyi kalır ve 3 MB/sn düşük hızlarında başlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 6e447c04f4a94f2fb534ecb0605595a90816431e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638305"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Bir otomatik olarak oluşturulan bit hızı Merdiveni ile kodlayın

## <a name="overview"></a>Genel Bakış

Bu makalede standart Kodlayıcı Media Services giriş çözünürlük ve bit hızı dayalı bir otomatik olarak oluşturulan bit hızı Merdiveni (bit hızı çözümleme çiftleri) bir giriş video kodlayın için nasıl kullanılacağı açıklanmaktadır. Bu ayarı yerleşik Kodlayıcı veya hazır, hiçbir giriş çözünürlük ve bit hızı aşacak. Örneğin, giriş 3 MB/sn hızında 720 p ise, çıktı 720 p en iyi kalır ve 3 MB/sn düşük hızlarında başlar.

### <a name="encoding-for-streaming"></a>Akış için kodlama

Kullandığınızda **AdaptiveStreaming** içinde hazır **dönüştürme**, akış HLS ve tire gibi protokolleri aracılığıyla teslim için uygun bir çıktı alın. Bu hazır kullanırken, hizmetin akıllıca oluşturmak için kaç tane video katmanları belirler ve hangi bit hızı ve çözümleme. Çıkış içerik nerede AAC kodlanmış ses ve video H.264 kodlu değil, araya eklemeli MP4 dosyalarını içerir.

Bu hazır nasıl kullanıldığı bir örnek görmek için bkz: [bir dosya akışı](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Çıktı

Bu bölüm ile kodlama sonucunda Media Services Kodlayıcı tarafından üretilen çıkış video katmanları üç örnekleri gösterir **AdaptiveStreaming** hazır. Tüm durumlarda çıktı stereo sesli 128 Kb/sn ile kodlanmış bir salt ses MP4 dosyası içerir.

### <a name="example-1"></a>Örnek 1
Yükseklik "1080" ve "29.970" kare hızı kaynağıyla 6 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
Yükseklik "720" ve "23.970" kare hızı kaynağıyla 5 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
Yükseklik "360" ve "29.970" kare hızı kaynağıyla 3 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bit hızı (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
