---
title: Azure portal ile Mantıksal Uygulamayı bir API olarak içeri aktarma | Microsoft Docs
description: Bu öğreticide, Mantıksal Uygulamayı bir API olarak içeri aktarmak için API Management’ın (APIM) nasıl kullanılacağı gösterilir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: 4b5f884fe6e1f1fdc12d7993418f7a10614a4cbe
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38602964"
---
# <a name="import-a-logic-app-as-an-api"></a>Mantıksal Uygulamayı API olarak içeri aktarma

Bu makalede Mantıksal Uygulamanın bir API olarak nasıl içeri aktarılacağı gösterilir. Makale, APIM API’sinin nasıl test edileceğini de göstermektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Mantıksal Uygulamayı API olarak içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ Aboneliğinizde bir Mantıksal Uygulama olduğundan emin olun. Daha fazla bilgi için [İlk Mantıksal Uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Arka uç API’sini içeri aktarma ve yayımlama

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Yeni API ekleyin** listesinden **Mantıksal Uygulama**’yı seçin.

    ![Mantıksal uygulama](./media/import-logic-app-as-api/logic-app-api.png)
3. Aboneliğinizdeki Mantıksal Uygulamaların listesini görmek için **Gözat**’a basın.
4. Uygulamayı seçin. APIM, seçili uygulamayla ilişkili Swagger’ı bulur, getirir ve içeri aktarır. 
5. API URL'si soneki ekleyin. Sonek, bu belirli API’yi bu APIM örneğinde tanımlayan bir addır. Son ekin bu APIM örneğinde benzersiz olması gerekir.
6. API’yi bir ürünle ilişkilendirerek yayımlayın. Bu durumda, "*Sınırsız*" ürünü kullanılır.  API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız, bir ürüne ekleyin. Bunu API oluşturması sırasında yapabilir ya da daha sonra ayarlayabilirsiniz.

    Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    * **Başlangıç**
    * **Sınırsız**   
7. **Oluştur**’u seçin.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Azure portalında yeni APIM API’sini test etme

İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan Azure portalından çağrılabilir.  

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **Test** sekmesine basın.
3. Bir işlem seçin.

    Sayfa, sorgu parametrelerinin ve üst bilgilerin alanlarını görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur. 
1. **Gönder**’e basın.

    Arka uç, **200 OK** ve bazı verilerle yanıt verir.

## <a name="call-operation"> </a>Geliştirici portalından işlem çağırma

API’leri test etmek için **Geliştirici portalından** da işlemler çağrılabilir. 

1. "Arka uç API’sini içeri aktarma ve yayımlama" adımında oluşturduğunuz API’yi seçin.
2. **Geliştirici portalı** düğmesine basın.

    "Geliştirici portalı" sitesi açılır.
3. Oluşturduğunuz **API**’yi seçin.
4. Test etmek istediğiniz işleme tıklayın.
5. **Deneyin**’e basın.
6. **Gönder**’e basın.
    
    Bir işlem çağrıldıktan sonra, geliştirici portalı **Yanıt durumu**, **Yanıt üst ilgileri** ve tüm **Yanıt içeriğini** gösterir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
> Her Mantıksal Uygulama, **el ile çağırma** işlemine sahiptir. API’nizin birden çok mantıksal uygulamadan oluşmasını istiyorsanız, çakışma olmaması için işlevi yeniden adlandırmanız gerekir.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)