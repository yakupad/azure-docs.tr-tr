---
title: Kapsayıcıları teklif kılavuzu için Azure Market'te yayımlama
description: Bu makalede kapsayıcı Market'te yayımlamak için gereksinimleri açıklanır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 5eb30c65032332825d05097f86d0275b015a8929
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059427"
---
# <a name="containers-offer-publishing-guide"></a>Yayımlama Kılavuzu'nu kapsayıcılar sunar.

Kapsayıcı teklifler kapsayıcı görüntünüzü Azure Marketi'nde yayımlayabileceğiniz yardımcı olur. Bu teklif gereksinimlerini anlamak için bu kılavuzu kullanın. 

Hangi dağıtılır ve faturalandırılır Market aracılığıyla işlem teklifler şunlardır. Bir kullanıcının gördüğü eylem çağrısı "Şimdi edinin." olan

Çözümünüze bir Kubernetes tabanlı Azure kapsayıcı hizmeti sağlanan bir Docker kapsayıcı görüntüsü olduğunda kapsayıcı teklif türünü kullanın.

>[!NOTE]
>Örneğin, bir Kubernetes tabanlı Azure kapsayıcı hizmeti Azure Kubernetes Service veya Azure Container Instances, Kubernetes tabanlı kapsayıcı çalışma zamanı için Azure müşterileri, tercih ettiğiniz gibi.  

Microsoft şu anda ücretsiz ve-kendi-lisansını getir (KLG) lisanslama modelleri destekler.

## <a name="containers-offer"></a>Kapsayıcıları teklifi

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçüm | Destek ya da ücretsiz veya KLG faturalandırma modeli. |  
| Dockerfile'dan görüntüsü | Kapsayıcı görüntüleri Docker görüntüsünü belirtimini temel alması gerekir ve bir hata dockerfile'dan gerekir.<ul> <li>Docker görüntüleri oluşturma hakkında daha fazla bilgi için adresinde yer alan kullanım bölümünü ziyaret edin ve [docs.docker.com/engine/reference/builder/#usage](https://docs.docker.com/engine/reference/builder/#usage).</li> </ul> |  
| ACR içinde barındırma | Bir Azure Container Registry (ACR) deposunda kapsayıcı görüntülerinin barındırılması gerekir.<ul> <li>ACR ile çalışma hakkında daha fazla bilgi için hızlı başlangıç sayfasını ziyaret edin: konumunda bulunan Azure portal sayfasındaki kullanarak bir kapsayıcı kayıt defteri oluşturma [docs.microsoft.com/azure/container-registry/container-registry-get-started-portal](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal).</li> </ul> |  
| Görüntü etiketleme | Kapsayıcı görüntüleri, en az 1 etiket içermelidir (en fazla etiket: 16).<ul> <li>Görüntü etiketleme hakkında daha fazla bilgi için adresinde yer alan docker tag sayfasını ziyaret edin [docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag).</li> </ul> |  


## <a name="next-steps"></a>Sonraki Adımlar

Zaten yapmadıysanız, 

- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com) oluşturmak veya teklifiniz tamamlamak için
