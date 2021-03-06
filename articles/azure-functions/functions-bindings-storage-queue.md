---
title: Azure işlevleri için Azure kuyruk depolama bağlamaları
description: Azure kuyruk depolama tetikleyici kullanmanız ve Azure işlevleri bağlamasında çıkış anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/23/2017
ms.author: tdykstra
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 71adccabc0778e2765c574f3714aab0ed0179deb
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724478"
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Azure işlevleri için Azure kuyruk depolama bağlamaları

Bu makalede Azure kuyruk depolama bağlamaları Azure işlevlerinde ile nasıl çalışılacağını açıklar. Tetikler ve bağlamaları sıralar için çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Kuyruk depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Queue) GitHub depo.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Kuyruk depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 3.x. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Storage/Queue) GitHub depo.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="trigger"></a>Tetikleyici

Yeni bir öğe üzerinde bir sıra alındığında bir işlev başlatmak için sıra tetikleyici kullanın. Kuyruk iletisini işleve giriş olarak sağlanır.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , yoklar `myqueue-items` sıraya ve Kuyruk öğesi işlenir her zaman bir günlüğe yazar.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama sırası tetikleyici gösterir bir *function.json* dosya ve [C# betik (.csx)](functions-reference-csharp.md) bağlama kullanan kod. İşlev yoklamalar `myqueue-items` sıraya ve Kuyruk öğesi işlenir her zaman bir günlüğe yazar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

[Kullanım](#trigger---usage) bölümde `myQueueItem`, tarafından adlandırılan `name` function.json özelliği.  [İleti meta veri bölümünden](#trigger---message-metadata) tüm gösterilen diğer değişkenlerin açıklar.

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama sırası tetikleyici gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev yoklamalar `myqueue-items` sıraya ve Kuyruk öğesi işlenir her zaman bir günlüğe yazar.

Burada *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

[Kullanım](#trigger---usage) bölümde `myQueueItem`, tarafından adlandırılan `name` function.json özelliği.  [İleti meta veri bölümünden](#trigger---message-metadata) tüm gösterilen diğer değişkenlerin açıklar.

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri
 
İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), bir sıra tetikleyici yapılandırmak için aşağıdaki öznitelikler kullanın:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs)

  Özniteliğin Oluşturucusu izlemek için kuyruk adı aşağıdaki örnekte gösterildiği gibi alır:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Depolama hesabı belirtmek için başka bir yol sağlar. Oluşturucusu depolama bağlantı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Depolama hesabı şu sırayla belirlenir:

* `QueueTrigger` Özniteliğin `Connection` özelliği.
* `StorageAccount` Aynı parametre olarak uygulanan öznitelik `QueueTrigger` özniteliği.
* `StorageAccount` İşlevi için uygulanan öznitelik.
* `StorageAccount` Sınıfına uygulanan öznitelik.
* "AzureWebJobsStorage" uygulama ayarı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `QueueTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok| ayarlanmalıdır `queueTrigger`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction**| yok | İçinde *function.json* yalnızca dosya. ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**Adı** | yok |İşlev kodu sırada temsil eden değişken adı.  | 
|**queueName** | **queueName**| Yoklamak için kuyruk adı. | 
|**Bağlantı** | **Bağlantı** |Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım
 
Yöntem parametresi gibi kullanarak, C# ve C# betik ileti veri erişim `string paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. Şu türlerden birine bağlayabilirsiniz:

* Nesnesi - işlevler çalışma zamanı bir JSON yükü kodunuzda tanımlanan rasgele sınıfının bir örneği içine seri durumdan çıkarır. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Bağlanmaya çalışırsanız `CloudQueueMessage` ve bir hata iletisini almak için bir başvuru olduğundan emin olun [doğru depolama SDK sürümü](#azure-storage-sdk-version-in-functions-1x).

JavaScript kullanılması `context.bindings.<name>` sıra öğesi yükü erişmek için. Yükü JSON ise, bir nesnede seri durumdan olan.

## <a name="trigger---message-metadata"></a>Tetikleyici - ileti meta verileri

Sıra tetikleyici birkaç sağlar [meta veri özelliklerini](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Bu özellikler, diğer bağlamaları bağlama ifadelerinde bir parçası olarak ya da kodunuzu parametre olarak kullanılabilir. Özelliklerini bunlar [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) sınıfı.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`QueueTrigger`|`string`|Sıra Yükü (geçerli bir dize ise). Sıranın yükü bir dize olarak olursa `QueueTrigger` tarafından adlı değişken aynı değere sahip `name` özelliğinde *function.json*.|
|`DequeueCount`|`int`|Bu iletiyi kuyruktan çıkarıldı sayısı.|
|`ExpirationTime`|`DateTimeOffset`|İleti sona ereceği saat.|
|`Id`|`string`|Sıra ileti kimliği.|
|`InsertionTime`|`DateTimeOffset`|İleti sıraya eklenen süre.|
|`NextVisibleTime`|`DateTimeOffset`|Sonraki ileti görebileceği süre.|
|`PopReceipt`|`string`|İleti pop giriş.|

## <a name="trigger---poison-messages"></a>Tetikleyici - zarar iletileri

Bir sıra Tetik işlevi başarısız olduğunda, Azure işlevleri işlev en fazla beş kez ilk denemede dahil olmak üzere belirli sıradaki ileti yeniden dener. Tüm beş deneme başarısız olursa işlevleri çalışma zamanı adlandırılan bir kuyruğun bir ileti ekler  *&lt;originalqueuename >-zararlı*. Günlüğe yazma veya el ile ilgili dikkat bir bildirim göndererek zararlı sırasından iletilerini işlemek için bir işlev gerekli yazabilirsiniz.

El ile zarar iletileri işlemek için denetleme [dequeueCount](#trigger---message-metadata) kuyruk iletisi.

## <a name="trigger---polling-algorithm"></a>Tetikleyici - yoklama algoritması

Sıra tetikleyici boşta kuyruk depolama işlem maliyetleri yoklama etkisini azaltmak için bir rastgele üstel geri alma algoritması uygular.  Bir ileti bulunduğunda, çalışma zamanı iki saniye bekler ve ardından başka bir ileti için denetler; bir ileti bulunduğunda, yeniden denemeden önce yaklaşık dört saniye bekler. Bir kuyruk iletisi almak için sonraki başarısız girişimden sonra bekleme süresini bir dakika olarak varsayılan olarak en fazla bekleme süresi ulaşana kadar artmaya devam eder. En fazla bekleme süresi aracılığıyla yapılandırılabilir `maxPollingInterval` özelliğinde [host.json dosya](functions-host-json.md#queues).

## <a name="trigger---concurrency"></a>Tetikleyici - eşzamanlılık

Bekleyen birden çok sıraya ileti olduğunda sıra tetikleyici toplu iletiler alır ve eşzamanlı olarak işlemek için işlevi örnekleri çağırır. Varsayılan olarak, toplu iş boyutu 16'dır. İşlenmekte olan sayısı 8'e aldığında, çalışma zamanı başka bir toplu iş alır ve bu iletileri işleme başlatır. Bu nedenle en fazla eş zamanlı iletileri işlevi bir sanal makineye (VM) başına işlenmekte olan 24 sayısıdır. Bu sınır ayrı ayrı her bir VM üzerinde her sıra tetiklenen işlevi uygulanır. Birden çok VM çıkışı işlevi uygulamanızı ölçekler, her VM için Tetikleyicileri bekleyin ve işlevleri çalıştırmayı deneyin. Bir işlev uygulaması çıkışı 3 VM ölçekler, örneğin, varsayılan en fazla eşzamanlı bir sıra tetiklemeli işlevin örneklerini 72 sayısıdır.

Toplu iş boyutu ve yeni bir toplu işi alınırken için eşik içinde yapılandırılabilir [host.json dosya](functions-host-json.md#queues). Paralel yürütme sırası tetiklenen bir işlev uygulaması işlevlerde için en aza indirmek istiyorsanız, yığın boyutu 1 olarak ayarlayın. Bu ayar, yalnızca tek bir sanal makinede (VM) işlevi uygulamanızın çalıştırdığı sürece eşzamanlılık ortadan kaldırır. 

Sıra tetikleyici otomatik olarak bir işlev bir kuyruk iletisi birden çok kez önlediği; İşlevler ıdempotent olmasını yazılması gerekmez.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#queues) dosyası sırası tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Çıktı

Kuyruğa ileti yazmak için bağlama Azure kuyruk depolama çıkış kullanın.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) alınan her HTTP isteği için bir kuyruk iletisi oluşturur.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama bir HTTP tetikleyicisi gösterir bir *function.json* dosya ve [C# betik (.csx)](functions-reference-csharp.md) bağlama kullanan kod. Bir kuyruk öğesiyle işlev oluşturur bir **CustomQueueMessage** alınan her HTTP isteği için nesne yükü.

Burada *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

Tek bir kuyruk iletisinin oluşturan bir C# kodu şöyledir:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Kullanarak aynı anda birden fazla ileti gönderebilir bir `ICollector` veya `IAsyncCollector` parametresi. HTTP istek verileri biri diğeri sabit kodlanmış değerler ile birden fazla ileti gönderen burada'nın C# betik kodu:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    TraceWriter log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama bir HTTP tetikleyicisi gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev alınan her HTTP isteği için bir kuyruk öğesi oluşturur.

Burada *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Bir ileti dizisi için tanımlayarak aynı anda birden fazla ileti gönderebilir `myQueueItem` bağlama çıktı. Şu JavaScript kodunu alınan her HTTP isteği için sabit kodlanmış değerlerle iki iletileri gönderir.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes"></a>Çıktı - öznitelikleri
 
İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

Öznitelik uygulandığı bir `out` parametresi veya işlevin dönüş değeri. Özniteliğin Oluşturucusu sırasının adı aşağıdaki örnekte gösterildiği gibi alır:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

Kullanabileceğiniz `StorageAccount` öznitelik sınıfı, yöntemi veya parametre düzeyinde depolama hesabı belirtin. Daha fazla bilgi için bkz: [tetikleyici - öznitelikleri](#trigger---attribute).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Queue` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | ayarlanmalıdır `queue`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction** | yok | ayarlanmalıdır `out`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodu sırada temsil eden değişken adı. Kümesine `$return` işlevi dönüş değeri başvurmak için.| 
|**queueName** |**queueName** | Kuyruğun adı. | 
|**Bağlantı** | **Bağlantı** |Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım
 
Yöntem parametresi gibi kullanarak tek bir kuyruk iletisinin, C# ve C# betik yazma `out T paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. Yöntemin dönüş türü yerine kullanabileceğiniz bir `out` parametresi ve `T` aşağıdaki türlerden biri olabilir:

* JSON olarak serileştirilebilir bir nesne
* `string`
* `byte[]`
* [CloudQueueMessage] 

Bağlanmaya çalışırsanız `CloudQueueMessage` ve bir hata iletisini almak için bir başvuru olduğundan emin olun [doğru depolama SDK sürümü](#azure-storage-sdk-version-in-functions-1x).

C# ve C# kod şu türlerden birini kullanarak birden çok sıra iletileri yazın: 

* `ICollector<T>` veya `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

JavaScript işlevlerini kullanmak `context.bindings.<name>` çıkış kuyruk iletisini erişmek için. Bir dize veya JSON serileştirilebilir bir nesne için kuyruk öğesi yükü kullanabilirsiniz.


## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama |  Başvuru |
|---|---|
| Kuyruk | [Sıra hata kodları](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| BLOB, tablo, kuyruk | [Depolama hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tablo, kuyruk |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir sıranın depolama tetikleyicisi kullanan bir hızlı başlangıç gidin](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Bir kuyruk depolama kullanan bir öğretici Git bağlama çıktı](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
