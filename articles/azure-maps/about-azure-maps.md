---
title: Azure Haritalar’a Genel Bakış | Microsoft Docs
description: Azure Haritalar tanıtımı
author: dsk-2015
ms.author: dkshir
ms.date: 07/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 93fe8dc3f8ff991cd6c48923d9e2073e4e93f1ad
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040856"
---
# <a name="what-is-azure-maps"></a>Azure Haritalar nedir?
Azure Haritalar, web ve mobil uygulamalarınıza coğrafi bağlam sağlamak amacıyla yeni harita verileriyle desteklenen bir jeo-uzamsal hizmetler koleksiyonudur. Konum hizmetleri için haritaları işleme, ilgi çekici noktaları, ilgi çekici noktaların yollarını, trafik koşullarını, saat dilimlerini ve IP'sini aramaya yönelik REST API'ler içerir. Ayrıca bu API'leri tanıdık araçlarla kullanarak hızla konum bilgilerini Azure çözümlerinizle tümleştiren çözümler geliştirebilir ve bu çözümleri ölçeklendirebilirsiniz. REST API'lerin yanı sıra, geliştirmeyi kolay, esnek ve birden fazla ortam arasında taşınabilir hale getirmek amacıyla web tabanlı bir JavaScript denetimi de sağlar. 

Aşağıdaki videoda Azure Haritalar ayrıntılı olarak açıklanır:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="services-in-azure-maps"></a>Azure Haritalar'ın içindeki hizmetler

Azure Haritalar, Azure uygulamalarınıza coğrafi bağlam sağlayabilecek şu altı hizmetten oluşur. 

### <a name="render-service"></a>İşleme hizmeti

İşleme hizmeti, geliştiricilerin harita verileri kullanan web ve mobil platform uygulamaları oluşturması amacıyla tasarlanmıştır. Hizmet, 19 yakınlaştırma düzeyine sahip yüksek kaliteli hücresel grafik görüntüler veya tamamen özelleştirilebilir vektör biçiminde harita görüntüleri kullanır.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

İşleme hizmeti şimdi geliştiricilerin uydu görüntülemeyle çalışmasına olanak tanıyan önizleme API'leri sunar. Diğer ayrıntılar için [Azure Haritalar İşleme API'leri](https://docs.microsoft.com/rest/api/maps/render) konusunu okuyun.


### <a name="route-service"></a>Yönlendirme hizmeti 

Yönlendirme hizmeti, birden çok taşıma moduna yönelik gerçek dünya altyapısı ve yol tarifleri için sağlam geometri hesaplamaları içerir. Hizmet, geliştiricilerin araba, kamyon, bisiklet veya yürüyüş gibi çeşitli seyahat modları arasında yol tariflerini hesaplamasını sağlar. Hizmet ayrıca trafik koşulları, ağırlık kısıtlamaları veya tehlikeli madde taşıma gibi girdileri de dikkate alabilir.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

Yönlendirme hizmeti şimdi birden çok yol isteğini toplu işleme, bir dizi başlangıç noktasıyla varış noktası arasındaki seyahat süresi ve mesafe matrisleri ve zamanınıza veya benzin gereksinimlerinize göre seyahat edebileceğiniz yolları veya mesafeleri bulma gibi gelişmiş özelliklerin önizlemesini sunar. Yönlendirme özelliklerinin ayrıntıları için, [Azure Haritalar Yönlendirme API'leri](https://docs.microsoft.com/rest/api/maps/route) konusunu okuyun.


### <a name="search-service"></a>Arama hizmeti

Arama hizmeti, geliştiricilerin ad veya kategori veya diğer coğrafi bilgileri kullanarak adres, yer veya işletme aramasını sağlayacak şekilde tasarlanmıştır. Arama Hizmeti adresleri ve caddeleri enlemlere ve boylamlara göre [tersine coğrafi koda dönüştürebilir](https://en.wikipedia.org/wiki/Reverse_geocoding). 

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Arama hizmeti, ayrıca yol boyunca arama, daha geniş bir alanda arama, bir grup arama isteğini toplu işleme ve bir konum noktası yerine daha geniş bir alan için arama yapma gibi gelişmiş özellikler de sağlar. Toplu arama ve alan arama API'leri şu anda önizleme aşamasındadır. Arama özellikleri hakkındaki diğer ayrıntıları için, [Azure Haritalar Arama API'leri](https://docs.microsoft.com/rest/api/maps/search) sayfasını okuyun.


### <a name="time-zone-service"></a>Saat Dilimi hizmeti

Saat Dilimi hizmeti, güncel, geçmiş ve gelecek saat dilimi bilgilerini enlem-boylam çifti veya [IANA ID](http://www.iana.org/) kullanarak sorgulamanızı sağlar. Saat Dilimi Hizmeti ayrıca Microsoft Windows saat dilimi kimliklerini IANA saat dilimlerine dönüştürmenizi, UTC ile saat dilimi farkını öğrenmenizi ve belirli bir saat dilimindeki güncel saati almanızı sağlar. Saat Dilimi Hizmetine gönderilen bir sorguya verilen tipik JSON yanıtı aşağıdaki örnek gibi olacaktır:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Bu hizmetle ilgili ayrıntılar için [Azure Haritalar Saat Dilimi API'leri](https://docs.microsoft.com/rest/api/maps/timezone) sayfasını ziyaret edin.

### <a name="traffic-service"></a>Trafik hizmeti

Trafik hizmeti, geliştiricilerin trafik verilerine ihtiyaç duyan web ve mobil platform uygulamaları geliştirmeleri için tasarlanmış bir web hizmeti paketidir. Hizmet, iki tür veri sağlar:
    * Trafik akışı: Ağ üzerindeki tüm ana yollar için gerçek zamanlı hız ve seyahat süresi bilgileri. 
    * Trafik olayları: Yol ağı üzerindeki trafik sıkışıklıklarının ve olaylarının doğru bir görünümü.

![Azure Haritalar Trafiği](media/about-azure-maps/Introduction_Traffic.png)

Diğer ayrıntılar için [Azure Haritalar Trafik API'leri](https://docs.microsoft.com/rest/api/maps/traffic) sayfasını ziyaret edin.

### <a name="ip-to-location"></a>IP Aracılığıyla Konum

IP Aracılığıyla Konum, belirli bir IP adresinin iki harfli ülke kodunu almanızı sağlayan bir önizleme hizmetidir. Bu hizmet uygulamanızı hem özel jeopolitik kısıtlamalara göre uyarlamanıza hem de coğrafi konum temelinde uygulamanın içeriğini değiştirerek kullanıcı deneyimini geliştirmenize yardımcı olabilir. 


## <a name="programming-model"></a>Programlama modeli

Azure Haritalar mobil kullanım için oluşturulmuştur ve platformlar arası uygulamaları destekleyebilir. Dilden bağımsız bir programlama modeli kullanır ve [REST API'ler](https://docs.microsoft.com/rest/api/maps/) üzerinde JSON çıkışını destekler. 

Azure Haritalar ayrıca hem web hem de mobil platform uygulamalarının hızlı ve kolay bir şekilde geliştirilmesi için basit bir programlama modeliyle kullanışlı bir [JavaScript harita denetimi](https://docs.microsoft.com/javascript/api/azure-maps-javascript/?view=azure-iot-typescript-latest) sunar. 


## <a name="usage"></a>Kullanım

Haritalar hizmetlerine erişmek için [Azure portalına](http://portal.azure.com) gidip Azure Haritalar hesabı oluşturulması gerekir. 

Azure Haritalar anahtar tabanlı bir kimlik doğrulama düzeni kullanır. Hesabınızda önceden oluşturulmuş iki anahtar mevcuttur. Bu anahtarlardan birini Azure Haritalar hizmetine yapılan isteklerde kullanarak bu konum özelliklerini doğrudan uygulamalarınızla tümleştirebilirsiniz.

## <a name="supported-regions"></a>Desteklenen bölgeler
Azure Haritalar API'si şu anda aşağıdaki ülkeler dışında tüm ülkelerde kullanılabilir: 

* Arjantin
* Çin
* Hindistan
* Fas
* Pakistan
* Güney Kore

Geçerli IP adresinizi kontrol edin ve IP adresinizin konumunun yukarıdaki desteklenmeyen ülkelerden birinde olmadığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Haritalar'ın yeni özellikleriyle ilgili daha fazla bilgi için: 
    - [Yol Matrisi, İzokronlar, IP araması ve daha fazlası](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/). 
- Hizmeti tanıtan bir örnek uygulamayı denemek için devam edin
    - [Bir demo etkileşimli arama haritası başlatma](quick-demo-map-app.md)
