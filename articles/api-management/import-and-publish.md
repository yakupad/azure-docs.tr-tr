---
title: Azure API Yönetimi’nde ilk API’nizi içeri aktarma ve yayımlama | Microsoft Docs
description: API Yönetimi ile ilk API’nizin nasıl içeri aktarılacağını ve yayımlanacağını öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 538977b9057a5699d61d6c2cc44209367e3550e2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722856"
---
# <a name="import-and-publish-your-first-api"></a>İlk API’nizi içeri aktarma ve yayımlama 

Bu öğreticide, http://conferenceapi.azurewebsites.net?format=json konumunda bulunan bir "OpenAPI belirtimi" arka uç API’sinin nasıl içeri aktarılacağı gösterilmektedir. Bu arka uç API’si, Microsoft tarafından sağlanır ve Azure’da barındırılır. 

Arka uç API’si, API Yönetimi’ne (APIM) içeri aktarıldıktan sonra APIM API’si, arka uç API’si için bir cephe olur. Arka uç API'sini içeri aktardığınızda hem kaynak API’si hem de APIM API’si aynı olur. APIM, arka uç API’sine dokunmadan ihtiyaçlarınıza göre cepheyi özelleştirmenize olanak sağlar. Daha fazla bilgi için bkz. [API’nizi dönüştürme ve koruma](transform-api.md). 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * İlk API’nizi içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

![Yeni API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Ön koşullar

Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Arka uç API’sini içeri aktarma ve yayımlama

Bu bölümde, bir OpenAPI belirtimi arka uç API’sinin nasıl içeri aktarılacağı ve yayımlanacağı gösterilmektedir.
 
1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. Listeden **OpenAPI belirtimini** seçin.

    ![Bir API oluşturma](./media/api-management-get-started/create-api.png)

    **Ayarlar** sekmesine giderek oluşturma sırasında veya sonrasında API değerlerini ayarlayabilirsiniz. Bir alanın yanındaki kırmızı yıldız, alanın zorunlu olduğunu belirtir.

    İlk API'nizi oluşturmak için aşağıdaki tabloda yer alan değerleri kullanın.

    |Ayar|Değer|Açıklama|
    |---|---|---|
    |**OpenAPI Belirtimi**|http://conferenceapi.azurewebsites.net?format=json|API’yi uygulayan hizmete başvurur. API Management istekleri bu adrese iletir.|
    |**Görünen ad**|*Tanıtım Konferansı API’si*|Hizmet URL’sini girdikten sonra sekme tuşuna basarsanız APIM, json'da ne olduğuna bağlı olarak bu alanı doldurur. <br/>Bu ad, Geliştirici portalında görüntülenir.|
    |**Ad**|*demo-conference-api*|API için benzersiz bir ad sağlar. <br/>Hizmet URL’sini girdikten sonra sekme tuşuna basarsanız APIM, json'da ne olduğuna bağlı olarak bu alanı doldurur.|
    |**Açıklama**|API için isteğe bağlı bir açıklama sağlayın.|Hizmet URL’sini girdikten sonra sekme tuşuna basarsanız APIM, json'da ne olduğuna bağlı olarak bu alanı doldurur.|
    |**URL düzeni**|*HTTPS*|API’ye erişmek için hangi protokollerin kullanılabileceğini belirler. |
    |**API URL’si soneki**|*conference*|Sonek, API yönetimi hizmetinin temel URL’sinin sonuna eklenir. API Management API'leri soneklerine bakarak ayırt eder; dolayısıyla belirli bir yayımcıdaki her API için sonekin benzersiz olması gerekir.|
    |**Ürünler**|*Sınırsız*|Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir Ürüne birkaç API ekleyebilir ve bunları geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. <br/>API’yi bir ürünle ilişkilendirerek yayımlarsınız (bu örnekte, *Sınırsız*). Bu yeni API’yi bir ürüne eklemek için ürün adını yazın (**Ayarlar** sayfasından da bunu yapabilirsiniz). Bu adım, API’yi birden fazla ürüne eklemek için birçok defa yinelenebilir.<br/>API’ye erişim elde etmek için geliştiricilerin önce bir ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. <br/> APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve tüm ürünlere abone olmuşsunuz demektir.<br/> Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir: **Başlangıç** ve **Sınırsız**. |
    |Bu API'nin sürümü oluşturulsun mu?||Sürüm oluşturma hakkında daha fazla bilgi için bkz. [API’nizin birden çok sürümünü yayımlama](api-management-get-started-publish-versions.md)|
    
    >[!NOTE]
    > API’yi yayımlamak için bir ürünle ilişkilendirmeniz gerekir. **Ayarlar sayfasından** bunu yapabilirsiniz.
    
3. **Oluştur**’u seçin.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>Azure portalında yeni APIM API’sini test etme

İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan Azure portalından çağrılabilir.  
1. Önceki adımda oluşturduğunuz API’yi seçin (**API’ler** sekmesinden).
2. **Test** sekmesine basın.  ![API'yi test et](./media/api-management-get-started/test-api.png)
3. **GetSpeakers** seçeneğine tıklayın.
    Sayfa, sorgu parametreleri alanlarını ve üst bilgileri görüntüler, ancak bu durumda bir alan yok. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. Anahtar otomatik olarak doldurulur.
4. **Gönder**’e basın.

    Arka uç, **200 OK** ve bazı verilerle yanıt verir.

## <a name="call-operation"> </a>Geliştirici portalından işlem çağırma

API’leri test etmek için **Geliştirici portalından** da işlemler çağrılabilir.

1. **Geliştirici portalına** gidin.
![Geliştirici portalı](./media/api-management-get-started/developer-portal.png)

2. **API'ler**’i seçip **Tanıtım Konferansı API'si**’ne ve ardından **GetSpeakers**’a tıklayın.
    
    Sayfa, sorgu parametreleri alanlarını ve üst bilgileri görüntüler, ancak bu durumda bir alan yok. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur.
3. **Deneyin**’e basın.
4. **Gönder**’e basın.
    
    Bir işlem çağrıldıktan sonra geliştirici portalı yanıtları gösterir.  

## <a name="next-steps"> </a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İlk API’nizi içeri aktarma
> * Azure portalında API’yi test etme
> * Geliştirici portalında API’yi test etme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Ürün oluşturma ve yayımlama](api-management-howto-add-products.md)
