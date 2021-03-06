---
title: Azure portal ile İşlev Uygulaması'nı bir API olarak içeri aktarma | Microsoft Docs
description: Bu öğreticide, API Management’ı (APIM) kullanarak İşlev Uygulaması'nı bir API olarak içeri aktarma işlemi gösterilir.
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
ms.date: 07/15/2018
ms.author: apimpm
ms.openlocfilehash: 0ee83446bb08e66c7f325bdd5585b8cc0484a74e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090937"
---
# <a name="import-a-function-app-as-an-api"></a>İşlev Uygulaması'nı API olarak içeri aktarma

Bu makalede İşlev Uygulaması'nın bir API olarak nasıl içeri aktarılacağı gösterilir. Makale, APIM API’sinin nasıl test edileceğini de göstermektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * İşlev Uygulaması'nı API olarak içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ Aboneliğinizde bir Azure İşlev Uygulaması bulunduğundan emin olun. Daha fazla bilgi için bkz. [İşlev Uygulaması oluşturma](../azure-functions/functions-create-first-azure-function.md#create-a-function-app)
+ Azure İşlev Uygulamanızın [OpenAPI tanımını oluşturma](../azure-functions/functions-openapi-definition.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Arka uç API’sini içeri aktarma ve yayımlama

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Yeni API ekleyin** listesinden **İşlev Uygulaması**’nı seçin.

    ![İşlev uygulaması](./media/import-function-app-as-api/function-app-api.png)
3. Aboneliğinizdeki İşlev Uygulamalarının listesini görmek için **Gözat**’a basın.
4. Uygulamayı seçin. APIM, seçili uygulamayla ilişkili Swagger’ı bulur, getirir ve içeri aktarır. 
5. API URL'si soneki ekleyin. Sonek, bu belirli API’yi bu APIM örneğinde tanımlayan bir addır. Son ekin bu APIM örneğinde benzersiz olması gerekir.
6. API’yi bir ürünle ilişkilendirerek yayımlayın. Bu durumda, "*Sınırsız*" ürünü kullanılır.  API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız, bir ürüne ekleyin. Bunu API oluşturması sırasında yapabilir ya da daha sonra ayarlayabilirsiniz.

    Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    * **Başlangıç**
    * **Sınırsız**   
7. **Oluştur**’u seçin.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Azure API Management’ı Azure İşlevleri anahtarlarıyla doldurma

İçeri aktarılan Azure İşlevleri anahtarlarla korunuyorsa API Management otomatik olarak bu işlevler için **Adlandırılmış değerler** oluşturur ancak girdileri gizli dizilerle doldurmaz. Her girdi için aşağıdaki adımları izlemeniz gerekir.  

1. API Management örneğinde **Adlandırılmış değerler** sekmesine gidin.
2. Girdiye tıklayın ve kenar çubuğundan **Değeri göster**’e basın.

    ![Adlandırılmış değerler](./media/import-function-app-as-api/apim-named-values.png)

3. İçerik *{Azure Function name} koduna* benziyorsa içeri aktarılan Azure İşlevleri Uygulaması’na geçin ve Azure İşlevinize gidin.
4. İstenen Azure İşlevi’nin **Yönet** bölümüne gidin ve Azure İşlevinizin kimlik doğrulama yöntemiyle ilişkili anahtarı kopyalayın.

    ![İşlev uygulaması](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Anahtarı **Adlandırılmış değerler**‘den metin kutusuna yapıştırın ve **Kaydet**’e tıklayın.

    ![İşlev uygulaması](./media/import-function-app-as-api/apim-named-values-2.png)

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

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yayımlanan API’yi dönüştürme ve koruma](transform-api.md)