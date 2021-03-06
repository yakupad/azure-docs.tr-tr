---
title: Azure haritalar içinde GeoJSON geometriler genişletme | Microsoft Docs
description: Azure haritalar içinde GeoJSON geometriler genişletmeyi öğrenin
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 319f9cba23d088553f361b6a0d648bbde94e0743
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968570"
---
# <a name="extending-geojson-geometries"></a>GeoJSON geometriler genişletme

Azure haritalar iç/boyunca coğrafi özellikleri aramak için güçlü API listesini sağlar.
Bu API'ler üzerinde standartlaştırmak [GeoJSON spec] [ 1] coğrafi özellikleri temsil etmek için (örneğin: durumu sınırları, yollar).  

[GeoJSON spec] [ 1] yalnızca aşağıdaki geometriler destekler:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Noktası
* Çokgen

Bazı Azure haritalar API (örneğin: [içinde arama geometri](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)) olmayan geometriler "Daire" gibi kabul parçası [GeoJSON spec][1].

Bu makalede Azure haritalar nasıl genişlettiğini ayrıntılı bir açıklama sağlar. [GeoJSON spec] [ 1] belirli geometriler temsil etmek için.

### <a name="circle"></a>Daire

`Circle` Geometri tarafından desteklenmiyor [GeoJSON spec][1]. Kullandığımız `GeoJSON Feature` bir daire temsil eden nesne.

A `Circle` geometrisini kullanılarak temsil edilir; `GeoJSON Feature` nesne __gerekir__ aşağıdakileri içerir:

1. Orta
   >Orta dairenin kullanılarak temsil edilir bir `GeoJSON Point` türü.

2. Yarıçap
   >Dairenin `radius` kullanılarak temsil edilir `GeoJSON Feature`ait özellikleri. Yarıçap değeri bulunduğu _ölçümleri_ ve türünde olmalıdır `double`.

3. Alt tür
   >Daire geometri da içermelidir `subType` özelliği. Bu özellik bir parçası olmalıdır `GeoJSON Feature`ait özellikleri ya da onun değeri _daire_


#### <a name="example"></a>Örnek

İşte, Orta dairenin nasıl temsil (enlem: 47.639754, boylam:-122.126986) ile bir RADIUS 100 kullanarak ölçümleri için eşit bir `GeoJSON Feature` nesnesi:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

[1]: https://tools.ietf.org/html/rfc7946
