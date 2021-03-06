---
title: Azure Search'te dizinler sorgulamak için arama Gezgini | Microsoft Docs
description: Azure Search'te dizin sorgulama için arama Gezgini'ni kullanma hakkında bilgi edinin.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 520d9e7b1899c54d922ff6fb77e0901f9609b029
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004142"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Azure Search'te sorgu dizinler için arama Gezgini'ni kullanma 

Bu makale, mevcut bir Azure Search dizini kullanarak nasıl sorgulanacağını gösterir **arama Gezgini** Azure portalında. Hizmetinizde var olan bir dizine basit veya tam Lucene sorgu dizeleri göndermek için arama Gezgini'ni kullanabilirsiniz.

## <a name="open-the-service-dashboard"></a>Hizmet panosunu açma
1. [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)'ın sol tarafındaki atlama çubuğunda **Tüm kaynaklar**’a tıklayın.
2. Azure Search hizmetinizi seçin.

## <a name="select-an-index"></a>Dizin seçme

**Dizinler** kutucuğunda aramak istediğiniz dizini seçin.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Arama Gezgini'ni Aç

Ve sonuçlar bölmesinde arama çubuğuna kaydırarak açmak için arama Gezgini kutucuğuna tıklayın.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Aramayı başlatma

Arama Gezgini'ni kullanırken belirtebileceğiniz [sorgu parametreleri](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) sorguyu formüle etmek için.

1. **Sorgu dizesi**’nde sorguyu yazın ve **Ara**’ya basın. 

   Azure Search REST API'sine HTTP isteği göndermek için, sorgu dizesi uygun istek URL'si içine otomatik olarak ayrıştırılır.   
   
   İsteği oluşturmak için herhangi bir geçerli basit veya tam Lucene sorgu söz dizimini kullanabilirsiniz. `*` karakteri, tüm belgelerin belirli bir sırada olmaksızın döndürüldüğü boş veya belirtilmemiş aramaya eşdeğerdir.

2. **Sonuçlar**’da sorgu sonuçları ham JSON olarak verilir; bu, HTTP Yanıt Gövdesinde programlama aracılığıyla istek verdiğinizde döndürülen yükle özdeştir.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynaklar ek sorgu söz dizimi bilgileri ve örnekler içerir.

 + [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Lucene sorgu söz dizimi örnekleri](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData Filtre ifadesinin söz dizimi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 