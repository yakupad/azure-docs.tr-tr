---
title: 'Öğretici: C# kullanarak bir Language Understanding (LUIS) uygulamasını çağırma | Microsoft Docs'
description: Bu öğreticide C# kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 0416d19d27810a2ab8eeb20e16b2f921fc7826ee
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263498"
---
# <a name="tutorial-call-a-luis-endpoint-using-c"></a>Öğretici: C# kullanarak bir LUIS uç noktasını çağırma

Amaç ve varlıkları döndürmek için bir LUIS uç noktasına konuşma iletin.

<!-- green checkmark -->
> [!div class="checklist"]
> * LUIS aboneliği oluşturma ve anahtar değerini daha sonra kullanmak üzere kopyalama
> * Tarayıcıdan genel örnek IoT uygulamasına iletilen LUIS uç nokta sonuçlarını görüntüleme
> * LUIS uç noktasına HTTPS çağrısı yapan bir Visual Studio C# konsol uygulaması oluşturma

<!-- link to free account --> Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="create-luis-subscription-key"></a>LUIS abonelik anahtarı oluşturma
1. Öncelikle Azure portalında bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturmanız gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

2. https://portal.azure.com adresinden Azure portalında oturum açın. 

3. Anahtar almak için [Azure'da Abonelik Anahtarı Oluşturma](./luis-how-to-azure-subscription.md) sayfasındaki adımları inceleyin.

4. [LUIS](luis-reference-regions.md) web sitesine geri dönün. Azure hesabınızı kullanarak oturum açın. 

    [![](media/luis-get-started-cs-get-intent/app-list.png "Uygulama listesinin ekran görüntüsü")](media/luis-get-started-cs-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>LUIS uygulaması tarafından döndürülen verileri anlama

Bir LUIS uygulaması tarafından döndürülen verileri anlamak için örnek LUIS uygulamasının URL'sini bir tarayıcı penceresine yapıştırabilirsiniz. Örnek uygulama, kullanıcının lambaları açma veya kapatma isteğini belirleyen bir IoT uygulamasıdır.

1. Örnek uygulamanın uç noktası şu biçimdedir: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` URL'yi kopyalayın ve `subscription-key` alanının değerini kendi abonelik anahtarınızla değiştirin.
2. URL'yi bir tarayıcı penceresine yapıştırıp Enter tuşuna basın. Tarayıcıda LUIS uygulamasının `HomeAutomation.TurnOn` amacını ve `HomeAutomation.Room` varlığını `bedroom` değeriyle algıladığını belirten bir JSON sonucu görüntülenir.

    ![JSON sonucu, TurnOn amacını algılar](./media/luis-get-started-cs-get-intent/turn-on-bedroom.png)
3. URL'deki `q=` parametresinin değerini `turn off the living room light` olarak değiştirip Enter tuşuna basın. Sonuç şimdi LUIS uygulamasının `HomeAutomation.TurnOff` amacını ve `HomeAutomation.Room` varlığını `living room` değeriyle algıladığını belirtir. 

    ![JSON sonucu, TurnOff amacını algılar](./media/luis-get-started-cs-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-c"></a>C# ile Uç Nokta API'sini kullanarak bir LUIS sonucundan faydalanma 

C# kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz. 

1. Visual Studio’da yeni bir konsol uygulaması oluşturun. Aşağıdaki kodu kopyalayıp bir *.cs dosyasına kaydedin:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/endpoint-api-samples/csharp/Program.cs)]
1. `subscriptionKey` değişkeninin değerini LUIS abonelik anahtarınızla değiştirin.

3. Visual Studio projesinde **System.Web** başvurusu ekleyin.

4. Konsol uygulamasını çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.

![Konsol penceresi, LUIS uygulamasından gelen JSON sonucunu görüntüler](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticide oluşturulan iki kaynak, LUIS abonelik anahtarı ve C# projesidir. LUIS abonelik anahtarını Azure portalından kaldırın. Visual Studio projesini kapatın ve dizini dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-cs-add-utterance.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website