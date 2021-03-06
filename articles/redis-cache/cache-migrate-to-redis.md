---
title: Redis - Azure yönetilen önbellek hizmeti uygulamalarını geçirme | Microsoft Docs
description: Yönetilen önbellek hizmeti ve rol içi önbellek uygulamaları Azure Redis cache'e geçirmeyi öğrenin
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: wesmc
ms.openlocfilehash: f499925ecea8ca127c90691f7d92e74e8df68cf9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38697348"
---
# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Azure Redis önbelleğine yönetilen önbellek Hizmeti'nden geçiş
Azure Redis Cache'e Azure yönetilen önbellek hizmeti kullanan, uygulamaları geçirme uygulamanızın, önbelleğe alma uygulamanız tarafından kullanılan yönetilen önbellek hizmeti özelliklere bağlı olarak küçük değişiklikler ile gerçekleştirilebilir. API'leri tam olarak aynı olsa da benzemez ve yönetilen önbellek hizmeti bir önbelleğe erişmek için kullandığı mevcut kodunuzu çoğunu minimum değişikliklerle yeniden kullanılabilir. Bu makalede, Azure Redis cache'i kullanmak için yönetilen önbellek hizmeti uygulamaları geçirmek için uygulama değişiklikleri ve gerekli yapılandırma yapmak nasıl gösterir ve nasıl bazı özellikleri Azure Redis Cache'in işlevselliğini uygulamak için kullanılabileceğini gösterir bir Yönetilen önbellek hizmeti önbelleği.

>[!NOTE]
>Yönetilen önbellek hizmeti ve rol içi önbellek [kullanımdan](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) 30 Kasım 2016. Azure Redis Cache'e geçirmek istediğiniz tüm rol içi önbellek dağıtımlarınız varsa, bu makaledeki adımları izleyebilirsiniz.

## <a name="migration-steps"></a>Geçiş adımları
Aşağıdaki adımlar, Azure Redis cache'i kullanmak için bir yönetilen önbellek hizmeti uygulamayı geçirmek için gereklidir.

* Azure Redis Cache'e yönetilen önbellek hizmeti özellikleri eşleme
* Bir önbellek teklifi seçin
* Bir önbelleği oluşturma
* Önbellek istemcilerini yapılandırma
  * Yönetilen önbellek hizmeti yapılandırmasını kaldırma
  * StackExchange.Redis NuGet paketi kullanarak bir önbellek istemcisi yapılandırma
* Yönetilen önbellek hizmeti kod geçirme
  * ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma
  * Önbellekte erişim ilkel veri türleri
  * Önbellekte .NET nesneleriyle çalışma
* ASP.NET oturum durumu ve çıktı önbelleğini Azure Redis önbelleğine geçiş 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Azure Redis Cache'e yönetilen önbellek hizmeti özellikleri eşleme
Azure yönetilen önbellek hizmeti ve Azure Redis Cache ile benzerdir, ancak bazı kendi özellikleri farklı şekilde uygular. Bu bölümde, bazı farklılıkları açıklar ve yönetilen önbellek hizmeti özelliklerini Azure Redis Cache'te uygulama hakkında rehberlik sağlar.

| Yönetilen önbellek hizmeti özelliği | Yönetilen önbellek hizmeti desteği | Azure Redis Cache desteği |
| --- | --- | --- |
| Adlandırılmış önbellekler |Varsayılan önbellek yapılandırılır ve isterseniz teklifleri, adlandırılan önbellekler ek dokuz kadar standart ve Premium önbelleğinde yapılandırılabilir. |Azure Redis cache'ler yapılandırılabilir adlandırılmış önbellekler için benzer bir işlevsellik uygulamak için kullanılan veritabanlarının (varsayılan değer 16) sahiptir. Daha fazla bilgi için bkz. [Redis veritabanı nedir?](cache-faq.md#what-are-redis-databases) ve [Varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration). |
| Yüksek Kullanılabilirlik |Standart ve Premium önbellek teklifleri önbellekte öğeleri için yüksek kullanılabilirlik sağlar. Öğeleri bir hata nedeniyle kaybolursa, yedek kopyaları Önbelleği'ndeki öğelerin kullanımını yine kullanılabilir durumdadır. İkincil önbellek için yazma işlemleri zaman uyumlu olarak yapılır. |Yüksek kullanılabilirlik (her bir Premium önbellek parçada bir birincil/çoğaltma çifti sahiptir) bir iki düğümlü birincil/çoğaltma yapılandırması olan standart ve Premium önbellek teklifleri, kullanılabilir. Çoğaltma yazma işlemlerini zaman uyumsuz olarak yapılır. Daha fazla bilgi için [Azure Redis Cache fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/). |
| Bildirimler |İstemcilerinin çeşitli önbellek işlemleri oluştuğunda bir adlandırılmış önbellek üzerinde zaman uyumsuz bildirimler almasına olanak tanır. |İstemci uygulamaları, Redis pub/sub kullanabilir veya [anahtar alanı bildirimleri](cache-configure.md#keyspace-notifications-advanced-settings) bildirimleri benzer bir işlevsellik elde etmek için. |
| Yerel önbellek |Çok hızlı erişim için istemcide önbelleğe alınan nesnelerin bir kopyasını yerel olarak depolar. |İstemci uygulamaları, sözlük veya benzer veri yapısı'nı kullanarak bu işlevselliği uygulamak gerekir. |
| Çıkarma İlkesi |Yok veya LRU. Varsayılan ilke LRU olur. |Azure Redis Cache aşağıdaki çıkarma ilkelerinin destekler: geçici lru, allkeys lru, rastgele geçici, allkeys rastgele, geçici ttl, noeviction. Volatile lru olan varsayılan ilkedir. Daha fazla bilgi için [varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration). |
| Süre sonu ilkesi |Varsayılan süre Dolum İlkesi mutlak ve varsayılan zaman aşımı aralığı 10 dakika. Kayan ve hiçbir zaman ilkelerini de mevcuttur. |Varsayılan olarak önbellekteki öğeleri süresinin sona ermediğinden, ancak bir sona erme önbellek kümesi aşırı yüklemeleri kullanmak yazma başına temelinde yapılandırılabilir. |
| Bölgeler ve etiketleme |Alt grupları için önbelleğe alınmış öğeleri bölgelerdir. Bölgeler, önbelleğe alınmış öğeleri ek açıklama etiketleri adlı ek açıklama dizelerle de destekler. Bölgeler, bu bölgede etiketlenmiş öğeler arama işlemleri gerçekleştirmek için özelliği destekler. Bir bölge içindeki tüm öğeleri tek bir düğüm önbellek kümesi içinde yer alır. |(Redis kümesi etkinleştirilmediği sürece) Redis önbelleği yönetilen önbellek hizmeti bölgeleri kavramını uygulanamaz tek bir düğümünden oluşur. Açıklayıcı etiketler katıştırılmış içinde anahtar adlarını ve daha sonra öğeleri almak için kullanılan anahtarlar alınırken redis aramayı destekler ve joker parçacık işlemleri. Redis etiketleme kullanarak çözümü uygulama örneği için bkz: [etiketleme ile Redis cache uygulanırken](http://stackify.com/implementing-cache-tagging-redis/). |
| Seri hale getirme |Yönetilen önbellek NetDataContractSerializer BinaryFormatter ve özel seri hale getiricileri genişletme kullanımını destekler. NetDataContractSerializer varsayılandır. |Bunları serileştirici istemci uygulama geliştiricisi kadar tercih ettiğiniz önbelleğine yerleştirmeden önce .NET nesneleri serileştirmek için istemci uygulamanın sorumluluğundadır. Daha fazla bilgi ve örnek kod için bkz. [önbellekte .NET nesneleriyle çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Önbellek öykünücüsü |Yönetilen önbellek, bir yerel önbellek öykünücü sağlar. |Azure Redis Cache, bir öykünücü yok, ancak yapabilecekleriniz [redis server.exe MSOpenTech yapısını yerel olarak çalıştırma](cache-faq.md#cache-emulator) bir öykünücü deneyimi sunar. |

## <a name="choose-a-cache-offering"></a>Bir önbellek teklifi seçin
Microsoft Azure Redis Cache aşağıdaki katmanlarda kullanılabilir:

* **Temel** – Tek düğümlü. 53 GB'a kadar birden çok boyut.
* **Standart** – İki düğümlü Birincil/Çoğaltma. 53 GB'a kadar birden çok boyut. %99,9 SLA.
* **Premium** – İki düğümlü Birincil/Çoğaltma, En fazla 10 parça. 6 GB'tan 530 GB'a kadar birden çok boyut. [Redis kümesi](cache-how-to-premium-clustering.md), [Redis kalıcılığı](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md) dahil tüm Standart katman özellikleri ve fazlası. %99,9 SLA.

Her katman özellikler ve fiyatlandırma açısından farklıdır. Özellikler, bu kılavuzun devamında ve fiyatlandırma hakkında daha fazla bilgi için ele alınmaktadır; bkz [önbellek fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cache/).

Geçiş için bir başlangıç noktası, önceki yönetilen önbellek hizmeti önbellek boyutunu eşleşen boyutu seçin ve sonra ölçeği artırın veya azaltın, uygulama gereksinimlerine bağlı olarak sağlamaktır. Doğru Azure Redis önbelleği teklifini seçme hakkında daha fazla bilgi için bkz. [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Bir önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Önbellek istemcilerini yapılandırma
Önbellek oluşturulup yapılandırıldıktan sonra sonraki adıma yönetilen önbellek hizmeti yapılandırmasını kaldırın ve Azure Redis Cache yapılandırmasını eklemek için ve önbellek istemcileri önbelleği erişebilmesi başvuruyor.

* Yönetilen önbellek hizmeti yapılandırmasını kaldırma
* StackExchange.Redis NuGet paketi kullanarak bir önbellek istemcisi yapılandırma

### <a name="remove-the-managed-cache-service-configuration"></a>Yönetilen önbellek hizmeti yapılandırmasını kaldırma
Önce istemci uygulamaları, Azure Redis Cache için var olan yönetilen önbellek hizmeti yapılandırmasını yapılandırılabilir ve yönetilen önbellek hizmeti NuGet paketini kaldırarak derleme başvurularının kaldırılması gerekir.

Yönetilen önbellek hizmeti NuGet paketini kaldırmak için istemci projeye sağ **Çözüm Gezgini** ve **NuGet paketlerini Yönet**. Seçin **yüklü paketleri** düğüm ve türü W**indowsAzure.Caching** kartındaki arama kutusuna yüklü paketler. Seçin **Windows** **Azure Cache** (veya **Windows** **Azure önbelleği** NuGet paketi sürümüne bağlı olarak), tıklayın**Kaldırma**ve ardından **Kapat**.

![Azure yönetilen önbellek hizmeti NuGet Paketi'ni Kaldır](./media/cache-migrate-to-redis/IC757666.jpg)

Yönetilen önbellek hizmeti NuGet paketini kaldırma yönetilen önbellek hizmeti derlemeler ve yönetilen önbellek hizmeti girdileri app.config veya web.config istemci uygulamasının kaldırır. NuGet paketi'ı kaldırırken bazı özelleştirilmiş ayarlar kaldırılamaz çünkü web.config veya app.config açın ve aşağıdaki öğeleri kaldırıldığından emin olun.

Emin `dataCacheClients` giriş kaldırılır `configSections` öğesi. Tüm kaldırmayın `configSections` öğesi; yalnızca Kaldır `dataCacheClients` varsa giriş.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Emin `dataCacheClients` bölümü kaldırılır. `dataCacheClients` Bölümü aşağıdaki örneğe benzer olacaktır.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
    <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Yönetilen önbellek hizmeti yapılandırma kaldırıldıktan sonra önbellek istemcisi aşağıdaki bölümde açıklanan şekilde yapılandırabilirsiniz.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>StackExchange.Redis NuGet paketi kullanarak bir önbellek istemcisi yapılandırma
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Yönetilen önbellek hizmeti kod geçirme
StackExchange.Redis önbellek istemcisi API'si yönetilen önbellek hizmetine benzer. Bu bölümde farklar genel bir bakış sağlar.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma
Yönetilen önbellek hizmeti önbelleği bağlantılar tarafından işlenen `DataCacheFactory` ve `DataCache` sınıfları. Azure Redis Cache'te bu bağlantılar tarafından yönetilen `ConnectionMultiplexer` sınıfı.

Aşağıdaki using deyimi, herhangi bir dosyadan, önbelleğe erişmek istediğiniz dön.

```csharp
using StackExchange.Redis
```

Bu ad alanı sorunu çözmezse, açıklandığı StackExchange.Redis NuGet paketi eklediğinizden emin olun [hızlı başlangıç: kullanım bir .NET uygulaması ile Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md).

> [!NOTE]
> StackExchange.Redis istemcisi .NET Framework 4 veya daha yüksek gerektiğini unutmayın.
> 
> 

Azure Redis Cache örneğine bağlanmak için statik çağrı `ConnectionMultiplexer.Connect` yöntemi ve uç noktasını ve anahtarı geçirin. Uygulamanızda bir `ConnectionMultiplexer` örneği paylaşmaya ilişkin bir yaklaşım, aşağıdaki örneğe benzer bir bağlı örnek döndüren statik özelliğe sahip olmaktır. Bu yaklaşım, tek bir bağlı başlatmak için iş parçacığı güvenli bir yol sağlar `ConnectionMultiplexer` örneği. Bu örnekte `abortConnect` önbelleğine bağlantı kurulmasa bile çağrının başarılı olacağıdır anlamına gelir. false olarak ayarlanır. `ConnectionMultiplexer` temel özelliklerinden biri ağ sorunu ya da diğer nedenler çözümlendiğinde önbellek bağlantısını otomatik olarak geri yüklemesidir.

```csharp
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
});

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

Önbellek uç noktasını, anahtarları ve bağlantı noktaları örneğinden alınabilen **Redis Cache** dikey penceresinde, önbellek örneğinizin. Daha fazla bilgi için [Redis Cache özellikleri](cache-configure.md#properties).

Bağlantı kurulduktan sonra çağırarak Redis cache veritabanına bir başvuru döndürmeyi `ConnectionMultiplexer.GetDatabase` yöntemi. `GetDatabase` yönteminden döndürülen nesne küçük, geçişli bir nesnedir ve depolanması gerekmez.

```csharp
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using the cache object...
// Simple put of integral data types into the cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from the cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

StackExchange.Redis istemcisi kullanan `RedisKey` ve `RedisValue` erişmek ve önbellekte öğe depolama türleri. Bu tür dizesi dahil olmak üzere en basit dil türleri eşleyin ve genellikle doğrudan kullanılmaz. Redis dizeleri Redis değeri en temel türü olan ve verileri seri hale getirilmiş ikili akışlar da dahil olmak üzere birçok türü içerebilir ve türün doğrudan kullanamazsınız sırada içeren yöntemleri kullanacaksınız `String` adı. En basit veri türleri için depolama ve önbellek kullanarak öğeleri alma `StringSet` ve `StringGet` yöntemleri, koleksiyonları veya diğer Redis veri türlerine önbellekte depolamak sürece. 

`StringSet` ve `StringGet` yönetilen önbellek hizmeti için benzer `Put` ve `Get` yöntemleri, önemli bir fark ayarlayın ve bir .NET nesnesini önbelleğe almak için önce onu önce seri gerekir, olmasına. 

Çağrılırken `StringGet`, nesne varsa, döndürülür ve kullanmıyorsa, null döndürülür. Bu durumda istenen veri kaynağından değeri alabilir ve bunu kullanmak için önbellekte saklayabilirsiniz. Bu düzen, edilgen önbellek düzeni olarak bilinir.

Bir öğenin önbellekte sona erme tarihini belirtmek için, `StringSet` dizesine ait `TimeSpan` parametresini kullanın.

```csharp
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis Cache temel veri türlerinin yanı sıra .NET nesnelerini ile çalışabilir, ancak bir .NET nesnesini önbelleğe alabilmek için seri hale getirilmesi gerekir. Bu seri hale getirme, uygulama geliştiricisinin sorumluluğundadır ve geliştirici seri hale getirici tercihinde esneklik. Daha fazla bilgi ve örnek kod için bkz. [önbellekte .NET nesneleriyle çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>ASP.NET oturum durumu ve çıktı önbelleğini Azure Redis önbelleğine geçiş
Azure Redis Cache ASP.NET oturum durumu hem de sayfa çıktı önbelleği için sağlayıcıları vardır. Bu sağlayıcılar yönetilen önbellek hizmeti sürümleri kullanan uygulamanızı geçirmek için önce mevcut bölümler, web.config dosyasından kaldırın ve ardından sağlayıcıları Azure Redis Cache sürümlerinde yapılandırın. Azure Redis Cache ASP.NET sağlayıcıları kullanma ile ilgili yönergeler için bkz: [Azure Redis Cache için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md) ve [Azure Redis Cache için ASP.NET çıktı önbelleği sağlayıcısı](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Sonraki adımlar
Keşfedin [Azure Redis Cache belgeleri](https://azure.microsoft.com/documentation/services/cache/) öğreticiler, örnekler, videoları ve daha fazla bilgi için.

