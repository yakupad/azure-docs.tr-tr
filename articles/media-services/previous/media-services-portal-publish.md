---
title: Azure portalında içerik yayımlama | Microsoft Docs
description: Bu öğreticide, Azure portalında içeriğinizi yayımlama adımları açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: d93bfc548134f730f4fad49a37593c861d6b6cbb
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790310"
---
# <a name="publish-content-in-the-azure-portal"></a>Azure portalında içerik yayımlama
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-publish.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [REST](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a>Genel Bakış
> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Kullanıcınıza içeriğinizin akışını sağlamak veya indirmek için kullanılabilecek bir URL sağlamak üzere, önce bulucu oluşturarak varlığınızı “yayımlamanız” gerekir. Bulucular varlık dosyaları erişim sağlar. Azure Media Services iki tür bulucuyu destekler: 

* **Akış (OnDemandOrigin) bulucuları**. Akış bulucuları, uyarlamalı akış için kullanılır. Uyarlamalı akış örnekleri (DASH, MPEG-DASH olarak da bilinir) HTTP üzerinden Apple HTTP canlı akışı (HLS), Microsoft kesintisiz akış ve dinamik Uyarlamalı akış içerir. Akış bulucusu oluşturmak için varlığınız bir .ism dosyası içermelidir. Örneğin, http://amstest.streaming.mediaservices.windows.net/61b3da1d-96c7-489e-bd21-c5f8a7494b03/scott.ism/manifest.
* **Aşamalı (paylaşılan erişim imzası) bulucuları**. Aşamalı bulucular, videoları aşamalı indirme aracılığıyla sunmak için kullanılır.

HLS akış URL’si oluşturmak için, URL’ye *(format=m3u8-aapl)* ekleyin.

    {streaming endpoint name-media services account name}/{locator ID}/{file name}.ism/Manifest(format=m3u8-aapl)

Kesintisiz Akış varlıklarını oynatacak akış URL’sini oluşturmak için aşağıdaki URL biçimini kullanın:

    {streaming endpoint name-media services account name}/{locator ID}/{file name}.ism/Manifest

MPEG DASH akış URL’si oluşturmak için, URL’ye *(format=mpd-time-csf)* ekleyin.

    {streaming endpoint name-media services account name}/{locator ID}/{file name}.ism/Manifest(format=mpd-time-csf)

Paylaşılan erişim imzası URL'si aşağıdaki biçime sahiptir:

    {blob container name}/{asset name}/{file name}/{shared access signature}

Daha fazla bilgi için bkz: [içerik genel bakış sunan](media-services-deliver-content-overview.md).

> [!NOTE]
> Azure portalında Mart 2015 öncesinde oluşturulmuş olan bulucuların iki yıllık sona erme tarihi vardır.  
> 
> 

Bulucunun sona erme tarihi güncelleştirmek için kullanım kullanabilirsiniz bir [REST API](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) veya [.NET API](http://go.microsoft.com/fwlink/?LinkID=533259). 

> [!NOTE]
> Paylaşılan erişim imzası bulucunun sona erme tarihini güncelleştirdiğinizde URL değişir.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Bir varlık yayımlamak için portal kullanmak üzere
1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** > **Varlıklar**’ı seçin. Yayımlamak istediğiniz varlığı seçin.
3. **Yayımla** düğmesini seçin.
4. Bulucu türünü seçin.
5. **Add (Ekle)** seçeneğini belirleyin.
   
    ![Videoyu yayımlama](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL **Yayımlanan URL’ler** listesine eklenir.

## <a name="play-content-in-the-portal"></a>Portalda içerik oynatma
Videonuzu Azure portalında bir içerik oynatıcıda test edebilirsiniz.

Videoyu seçin ve ardından **Oynat** düğmesini seçin.

![Videoyu Azure portalında oynatma](./media/media-services-portal-vod-get-started/media-services-play.png)

Bazı dikkate alınması gereken noktalar vardır:

* Videonun yayımlandığından emin olun.
* Azure portal medya oynatıcı varsayılan akış uç noktasından oynatılır. Varsayılan olmayan bir akış uç noktasından oynatmak istiyorsanız, URL'yi seçip kopyalayın ve başka bir oynatıcıya yapıştırın. Örneğin [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) üzerinde videonuzu test edebilirsiniz.
* Akış akış uç çalıştırması gerekir.  

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

