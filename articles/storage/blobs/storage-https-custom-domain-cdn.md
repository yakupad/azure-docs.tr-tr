---
title: BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için Azure CDN'yi kullanma
description: BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için blob storage ile Azure CDN tümleştirileceği hakkında bilgi edinin
services: storage
documentationcenter: ''
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.openlocfilehash: b3b1b5064e51b68bb64cb8c4dbec6075705795d6
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37026006"
---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için Azure CDN'yi kullanma
Azure içerik teslim ağı (CDN), artık HTTPS için özel etki alanı adlarını destekler. HTTPS üzerinden özel etki alanınızı kullanarak depolama BLOB'ları erişmek için bu özelliğinden yararlanabilirsiniz. Bunu yapmak için öncelikle, blob veya web uç noktada Azure CDN'yi etkinleştirme ve CDN özel etki alanı adına eşlemek gerekir. Bu adımları uyguladıktan sonra özel etki alanınız için HTTPS'yi etkinleştirme tek tıklatmayla etkinleştirme, eksiksiz bir sertifika yönetimi ve normal CDN fiyatlandırma için ek ücret ödemeden tümüyle aracılığıyla basitleştirilmiştir.

Bu yetenek, gizlilik ve veri bütünlüğü aktarım sırasında hassas web uygulama verilerinizin korunmasına olanak sağladığından önemlidir. HTTPS üzerinden trafik hizmet vermek için SSL protokolü kullanarak Internet üzerinden gönderilen verilerin şifrelenmesini sağlar. HTTPS güven ve kimlik doğrulaması sağlar ve web uygulamalarınızın saldırılara karşı korur.

> [!NOTE]  
> Özel etki alanı adları SSL desteği sağlamaya ek olarak, Azure CDN uygulamanız dünya yüksek bant genişliği içerik ulaştırmak için ölçek yardımcı olabilir. Daha fazla bilgi için kullanıma [Azure CDN genel bakış](../../cdn/cdn-overview.md).

## <a name="quick-start"></a>Hızlı başlangıç
Bu, HTTPS için özel blob storage uç noktanız etkinleştirmek için gereken adımlar şunlardır:

1.  [Azure storage hesabı Azure CDN ile tümleştirmek](../../cdn/cdn-create-a-storage-account-with-cdn.md).
    Bu makalede, bunu zaten yapmadıysanız, bir depolama hesabı Azure Portalı'nda oluşturmada size yol gösterilir.

    > [!NOTE]  
    > Azure Storage statik Web sitelerine desteği Önizleme sırasında "özel kaynak", "kaynak türü" açılan menüden, depolama web uç noktası eklemek için seçin. Azure Portalı'nda bu CDN profilinizi yerine doğrudan depolama hesabınızdaki yapmak gerekir.

2.  [Azure CDN içeriğini özel bir etki alanına Eşle](../../cdn/cdn-map-content-to-custom-domain.md).
3.  [Azure CDN özel etki alanı üzerinde HTTPS'yi etkinleştirmek](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Paylaşılan erişim imzaları
Blob storage uç noktanız anonim okuma erişimini engellemek için yapılandırılmışsa, sağlamanız gerekir bir [paylaşılan erişim imzası (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) özel etki alanınıza yaptığınız her istekte belirteci. Varsayılan olarak, blob depolama uç noktaları anonim okuma erişimini engellemek. Bkz: [kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](storage-manage-access-to-resources.md) paylaşılan erişim imzaları hakkında daha fazla bilgi için.

Azure CDN SAS belirteci eklenen herhangi bir kısıtlamanın uymaz. Örneğin, tüm SAS belirteci sona erme süresi vardır. Bu içeriği CDN uç düğümlerden temizlenir kadar içerik Bunun anlamı ile süresi dolmuş bir SAS hala erişilebilir. Ne kadar veri üzerinde CDN önbellek yanıt üstbilgisi ayarlayarak önbelleğe kontrol edebilirsiniz. Bkz: [Azure CDN Azure Storage bloblarında sona erme tarihi yönetme](../../cdn/cdn-manage-expiration-of-blob-content.md) yönergeler için.

Aynı blob uç noktası için birden çok SAS URL'leri oluşturursanız, sorgu dizesi, Azure CDN için önbelleğe almayı etkinleştirdikten öneririz. Bu her URL benzersiz bir varlık olarak kabul edilir sağlamaktır. Bkz: [Azure CDN önbelleğe alma davranışını sorgu dizeleriyle denetleme](../../cdn/cdn-query-string.md) daha fazla bilgi için.

## <a name="http-to-https-redirection"></a>HTTP-HTTPS yeniden yönlendirmesi
HTTPS için HTTP trafiği yönlendirmek seçebilirler. Bu Verizon sunan Azure CDN premium kullanılmasını gerektirir. Yapmanız [Azure CDN kurallar altyapısı kullanarak geçersiz kılma HTTP davranışı](../../cdn/cdn-rules-engine.md) şu kurala:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

CDN uç noktanız için yapılandırılan adı "Cdn uç noktası adı" başvuruyor. Bu değer aşağı açılır listeden seçebilirsiniz. "Kaynak yolu" yol, statik içerik bulunduğu kaynak depolama hesabınızın içinde ifade eder. Tek bir kapsayıcıdaki tüm statik içerik barındırıyorsa, "kaynak yolu" Bu kapsayıcı adıyla değiştirin.

Kuralı daha ayrıntılı bilgi edinmek için lütfen bkz. [Azure CDN kurallar altyapısı özellikleri](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
BLOB'ları Azure CDN eriştiğinizde, ödeme [Blob Depolama fiyatları](https://azure.microsoft.com/pricing/details/storage/blobs/) kenar düğümler ve (Blob Depolama) kaynağı arasındaki trafik için ve [CDN fiyatları](https://azure.microsoft.com/pricing/details/cdn/) kenar düğümlerden erişilen veriler için.

Örneğin, bir Azure CDN kullanarak erişiliyor Batı ABD depolama hesabında olduğunu varsayalım. Kişi İngiltere CDN yoluyla o depolama hesabındaki BLOB erişim çalışırsa, Azure, blob UK en yakın kenar düğümüne ilk denetler. Bulunursa, bu kopyayı BLOB erişir ve CDN fiyatlandırması, CDN üzerinde erişilen için kullanacağı varsa. Bulunamazsa, Azure blob çıkış ve işlem ücretleri Blob storage fiyatlandırması belirtildiği gibi yol ve CDN faturalama sonuçlanır kenar düğümüne dosyada erişim kenar düğümüne kopyalayın.

Bakıldığında [CDN fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cdn/), özel etki alanı adları için HTTPS desteği olduğunu unutmayın yalnızca Verizon ürünleri (standart ve Premium) Azure CDN için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Blob storage uç noktanız için özel etki alanı adı yapılandırma](storage-custom-domain-name.md)
* [Azure Storage (Önizleme) statik Web sitesi barındırma](storage-blob-static-website.md)
