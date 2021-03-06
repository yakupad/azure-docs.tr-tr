---
title: Azure Cosmos DB Gremlin desteği | Microsoft Docs
description: Apache TinkerPop’un Gremlin dili hakkında bilgi edinin. Azure Cosmos DB’de kullanılabilen özellikleri ve adımları öğrenin
services: cosmos-db
author: LuisBosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: overview
ms.date: 01/02/2018
ms.author: lbosq
ms.openlocfilehash: c675f37e50f5b8a259048d9a92fcdbe5b947068c
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797626"
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Azure Cosmos DB Gremlin grafik desteği
Azure Cosmos DB, [Apache Tinkerpop’un](http://tinkerpop.apache.org) grafik varlıkları oluşturmak ve grafik sorgu işlemlerini gerçekleştirmeye yönelik Graph API’si ve aynı zamanda bir grafik geçiş dili olan [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps)’i destekler. Grafik varlıkları (köşeler ve kenarlar) oluşturmak, bu varlıkların içindeki özellikleri değiştirmek, sorgu ve geçiş işlemleri gerçekleştirmek ve varlıkları silmek için Gremlin dilini kullanabilirsiniz. 

Azure Cosmos DB, kurumsal kullanıma hazır özellikleri grafik veritabanlarına getirir. Buna genel dağıtım, depolama ve aktarım hızının bağımsız ölçeklendirmesi, öngörülebilir tek basamaklı milisaniyelik gecikmeler, otomatik dizinleme, iki veya daha fazla Azure bölgesine yayılan veritabanı hesaplarının kullanılabilirliğini okuma dahildir. Azure Cosmos DB TinkerPop/Gremlin’i desteklediğinden başka bir grafik veritabanı kullanılarak yazılmış uygulamaları kod değişikliği yapmanıza gerek kalmadan kolayca geçirebilirsiniz. Azure Cosmos DB, Gremlin desteği sayesinde [Apache Spark GraphX](http://spark.apache.org/graphx/) gibi TinkerPop etkin analitik çerçevelerle de sorunsuz bir şekilde tümleşir. 

Bu makalede Gremlin’e ilişkin hızlı bir adım adım kılavuz sağlıyoruz ve Graph API tarafından desteklenen Gremlin özelliklerinin ve adımlarının listesini oluşturuyoruz.

## <a name="gremlin-by-example"></a>Örneğe göre Gremlin
Sorguların Gremlin’de nasıl ifade edildiğini anlamak için örnek bir grafik kullanalım. Aşağıdaki şekilde kullanıcılar, ilgi alanları ve cihazlar hakkındaki verileri yöneten bir iş uygulaması grafik biçiminde gösterilir.  

![Kişileri, cihazları ve ilgi alanlarını gösteren örnek grafik](./media/gremlin-support/sample-graph.png) 

Bu grafikte aşağıdaki köşe türleri (Gremlin’de “etiket” olarak adlandırılır) vardır:

- Kişiler: Grafikte Robin, Thomas ve Ben olmak üzere üç kişi var
- İlgi Alanları: Bu örnekteki kişilerin ilgi alanı futbol
- Cihazlar: Kişilerin kullandığı cihazlar
- İşletim Sistemleri: Cihazların çalıştığı işletim sistemleri

Aşağıdaki kenar türleri/etiketleri üzerinden bu varlıklar arasındaki ilişkiyi temsil ediyoruz:

- Tanıma: Örneğin, “Thomas, Robin’i tanıyor”
- İlgilenme: Grafiğimizdeki kişilerin ilgilerini temsil eder, örneğin “Ben, futbolla ilgilenmektedir”
- İşletim Sistemi Çalıştırma: Dizüstü bilgisayar, Windows işletim sistemini çalıştırır
- Kullanma: Kişinin kullandığı cihazı temsil eder. Örneğin Robin, seri numarası 77 olan bir Motorola telefon kullanır

Şimdi [Gremlin Console](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console)’u kullanarak bu grafiğe yönelik birkaç işlem yapalım. Dilerseniz bu işlemleri, tercih ettiğiniz platformdaki (Java, Node.js, Python veya .NET) Gremlin sürücülerini kullanarak da gerçekleştirebilirsiniz.  Azure Cosmos DB’de nelerin desteklendiğine bakmadan önce söz dizimine hakkında bilgi edinmek için birkaç örneğe bakalım.

İlk olarak CRUD’a bakalım. Aşağıdaki Gremlin deyimi “Thomas” köşesini grafiğe ekler:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Daha sonra aşağıdaki Gremlin deyimi, Thomas ve Robin arasına bir “Tanıma” kenarı ekler.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Aşağıdaki sorgu, “kişiler” köşesini ilk adlarına göre azalan sırada döndürür:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Grafiklerin asıl iyi olduğu kısımlar, “Thomas’ın arkadaşları hangi işletim sistemini kullanıyor?” gibi sorular sorduğunuzda ortaya çıkıyor. Bu bilgileri grafikten edinmek için bu örnek Gremlin geçişini çalıştırabilirsiniz:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Şimdi de Azure Cosmos DB’nin Gremlin geliştiricilerine neler sunduğuna bakalım.

## <a name="gremlin-features"></a>Gremlin özellikleri
TinkerPop, çeşitli grafik teknolojilerini kapsayan bir standarttır. Bu nedenle bir grafik sağlayıcısı tarafından sağlanan özellikleri tanımlamaya yönelik standart bir terminolojisi vardır. Azure Cosmos DB kalıcı, yüksek eşzamanlılığa sahip, birden çok sunucu ve kümeye ayrılabilen yazılabilir bir grafik veritabanı sağlar. 

Aşağıdaki tabloda Azure Cosmos DB tarafından uygulanan TinkerPop özellikleri listelenmektedir: 

| Kategori | Azure Cosmos DB uygulaması |  Notlar | 
| --- | --- | --- |
| Grafik özellikleri | Kalıcılık ve EşzamanlıErişim sağlar. İşlemleri desteklemek için tasarlanmıştır | Bilgisayar yöntemleri, Spark bağlayıcısı tarafından uygulanabilir. |
| Değişken özellikleri | Boolean, Tamsayı, Bayt, Çift, Kayan Sayı, Uzun, Dize destekler | İlkel türleri destekler ve veri modeli aracılığıyla oluşan karmaşık türlerle uyumludur |
| Köşe özellikleri | RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty işlevlerini destekler  | Köşe oluşturma, değiştirme ve silmeyi destekler |
| Köşe özellikleri | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues işlevlerini destekler | Köşe özelliklerini oluşturma, değiştirme ve silmeyi destekler |
| Kenar özellikleri | AddEdges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Kenar oluşturma, değiştirme ve silmeyi destekler |
| Kenar özellikleri | Properties, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Kenar özelliklerini oluşturma, değiştirme ve silmeyi destekler |

## <a name="gremlin-wire-format-graphson"></a>Gremlin gönderme biçimi: GraphSON

Azure Cosmos DB, Gremlin işlemlerinden sonuçları döndürürken [GraphSON biçimini](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) kullanır. GraphSON köşeleri, kenarları ve özellikleri (tek ve birden çok değerli özellikleri) JSON kullanarak temsil etmeye yönelik standart Gremlin biçimidir. 

Örneğin aşağıdaki kod parçacığında Azure Cosmos DB’den *istemciye döndürülen* bir köşenin temsili gösterilir. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

Köşeler için GraphSON tarafından kullanılan özellikler şunlardır:

| Özellik | Açıklama |
| --- | --- |
| id | Köşenin kimliği. Benzersiz olmalıdır (varsa _partition değeriyle birlikte) |
| etiket | Köşenin etiketi. Bu isteğe bağlıdır ve varlık türünü tanımlamak için kullanılır. |
| type | Grafik olmayan belgelerdeki köşeleri ayırt etmek için kullanılır |
| properties | Köşe ile ilişkili, kullanıcı tanımlı özellikler paketi. Her bir özellik birden çok değere sahip olabilir. |
| _partition (yapılandırılabilir) | Köşenin bölüm anahtarı. Grafiklerin ölçeğini birden çok sunucuya genişletmek için kullanılabilir |
| outE | Bu, bir köşenin dış kenarlarının listesini içerir. Komşuluk bilgilerini köşeyle birlikte depolamak, geçişlerin hızla yürütülmesini sağlar. Kenarlar etiketlerine göre gruplandırılır. |

Kenar, grafiğin diğer bölümlerine gezintiyi kolaylaştırmak için aşağıdaki bilgiyi içerir.

| Özellik | Açıklama |
| --- | --- |
| id | Kenarın kimliği. Benzersiz olmalıdır (varsa _partition değeriyle birlikte) |
| etiket | Kenarın etiketi. Bu özellik isteğe bağlıdır ve ilişki türünü tanımlamak için kullanılır. |
| inV | Bu, bir kenarın iç köşelerinin listesini içerir. Komşuluk bilgilerini kenarla birlikte depolamak, geçişlerin hızla yürütülmesini sağlar. Köşeler etiketlerine göre gruplandırılır. |
| properties | Kenar ile ilişkili, kullanıcı tanımlı özellikler paketi. Her bir özellik birden çok değere sahip olabilir. |

Her bir özellik, bir dizi içinde birden çok değer depolayabilir. 

| Özellik | Açıklama |
| --- | --- |
| değer | Özelliğin değeri

## <a name="gremlin-steps"></a>Gremlin adımları
Şimdi de Azure Cosmos DB tarafından desteklenen Gremlin adımlarına bakalım. Gremlin hakkında eksiksiz bir başvuru için bkz. [TinkerPop başvurusu](http://tinkerpop.apache.org/docs/current/reference).

| adım | Açıklama | TinkerPop 3.2 Belgeleri |
| --- | --- | --- |
| `addE` | İki köşe arasına kenar ekler | [addE step](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) |
| `addV` | Grafiğe bir köşe ekler | [addV step](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) |
| `and` | Tüm geçişlerin bir değer döndürmesini sağlar | [and step](http://tinkerpop.apache.org/docs/current/reference/#and-step) |
| `as` | Bir adımın çıktısına değişken atanmasını sağlayan adım modülatörü | [as step](http://tinkerpop.apache.org/docs/current/reference/#as-step) |
| `by` | `group` ve `order` ile kullanılan bir adım modülatörü | [by step](http://tinkerpop.apache.org/docs/current/reference/#by-step) |
| `coalesce` | Sonuç döndüren ilk geçişi döndürür | [coalesce step](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) |
| `constant` | Sabit bir değer döndürür. `coalesce` ile kullanılır| [constant step](http://tinkerpop.apache.org/docs/current/reference/#constant-step) |
| `count` | Geçiş sayımını döndürür | [count step](http://tinkerpop.apache.org/docs/current/reference/#count-step) |
| `dedup` | Yinelenenlerin kaldırıldığı değerleri döndürür | [dedup step](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) |
| `drop` | Değerleri (köşe/kenar) bırakır | [drop step](http://tinkerpop.apache.org/docs/current/reference/#drop-step) |
| `fold` | Sonuçların toplamını hesaplayan bir engel gibi davranır| [fold step](http://tinkerpop.apache.org/docs/current/reference/#fold-step) |
| `group` | Belirtilen etiketleri temel alarak değerleri gruplandırır| [group step](http://tinkerpop.apache.org/docs/current/reference/#group-step) |
| `has` | Özellikleri, köşeleri ve kenarları filtrelemek için kullanılır. `hasLabel`, `hasId`, `hasNot` ve `has` değişkenlerini destekler. | [has step](http://tinkerpop.apache.org/docs/current/reference/#has-step) |
| `inject` | Değerleri bir akışa ekler| [inject step](http://tinkerpop.apache.org/docs/current/reference/#inject-step) |
| `is` | Boole ifadesi kullanarak bir filtre uygulamak için kullanılır | [is step](http://tinkerpop.apache.org/docs/current/reference/#is-step) |
| `limit` | Geçişteki öğelerin sayısını sınırlamak için kullanılır| [limit step](http://tinkerpop.apache.org/docs/current/reference/#limit-step) |
| `local` | Alt sorgu gibi, geçişin bir bölümünü yerel olarak sarmalar | [local step](http://tinkerpop.apache.org/docs/current/reference/#local-step) |
| `not` | Filtre olumsuzlamayı üretmek için kullanılır | [not step](http://tinkerpop.apache.org/docs/current/reference/#not-step) |
| `optional` | Bir sonuç elde ettiği takdirde, belirtilen geçişin sonucunu döndürür; aksi takdirde çağıran öğeyi döndürür | [optional step](http://tinkerpop.apache.org/docs/current/reference/#optional-step) |
| `or` | En azından bir geçişin değer döndürmesini sağlar | [or step](http://tinkerpop.apache.org/docs/current/reference/#or-step) |
| `order` | Sonuçları, belirtilen sıralama düzeninde döndürür | [order step](http://tinkerpop.apache.org/docs/current/reference/#order-step) |
| `path` | Geçişin tam yolunu döndürür | [path step](http://tinkerpop.apache.org/docs/current/reference/#path-step) |
| `project` | Özellikleri bir Harita gibi projelendirir | [project step](http://tinkerpop.apache.org/docs/current/reference/#project-step) |
| `properties` | Belirtilen etiketlerin özelliklerini döndürür | [properties step](http://tinkerpop.apache.org/docs/current/reference/#properties-step) |
| `range` | Belirtilen değer aralığını filtreler| [range step](http://tinkerpop.apache.org/docs/current/reference/#range-step) |
| `repeat` | Adımı belirtilen sayıda tekrarlar. Döngü için kullanılır | [repeat step](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) |
| `sample` | Sonuçları geçişten örneklendirmek için kullanılır | [sample step](http://tinkerpop.apache.org/docs/current/reference/#sample-step) |
| `select` | Sonuçları geçişten projelendirmek için kullanılır |  [select step](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Geçişteki engelleyici olmayan toplamalar için kullanılır | [store step](http://tinkerpop.apache.org/docs/current/reference/#store-step) |
| `tree` | Bir köşeden ağaca yolları toplar | [tree step](http://tinkerpop.apache.org/docs/current/reference/#tree-step) |
| `unfold` | Adım olarak bir yineleyici açar| [unfold step](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) |
| `union` | Birden çok geçişin sonuçlarını birleştirir| [union step](http://tinkerpop.apache.org/docs/current/reference/#union-step) |
| `V` | Köşe ve kenarlar arasında geçiş için gerekli olan adımları içerir: `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV` ve `otherV`  | [vertex steps](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) |
| `where` | Geçişten alınan sonuçları filtrelemek için kullanılır. `eq`, `neq`, `lt`, `lte`, `gt`, `gte` ve `between` işleçlerini destekler  | [where step](http://tinkerpop.apache.org/docs/current/reference/#where-step) |

Azure Cosmos DB tarafından sağlanan, yazma için iyileştirilmiş altyapı, köşe ve kenarlar içindeki tüm özelliklerin dizinlerinin otomatik olarak oluşturulmasını varsayılan olarak destekler. Bu nedenle herhangi bir özellik üzerindeki sorgulu filtreler, aralık sorguları, sıralama veya toplamalar dizinden işlenir ve etkin bir biçimde sunulur. Azure Cosmos DB’de dizin oluşturmanın işleyişi hakkında daha fazla bilgi için [schema-agnostic dizin oluşturma](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) makalemizi okuyun.

## <a name="next-steps"></a>Sonraki adımlar
* [SDK’larımızı kullanarak](create-graph-dotnet.md) bir grafik uygulaması oluşturmaya başlayın 
* Azure Cosmos DB’de [grafik desteği](graph-introduction.md) hakkında daha fazla bilgi edinin
