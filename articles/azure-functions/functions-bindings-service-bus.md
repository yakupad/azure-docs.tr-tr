---
title: Azure işlevleri için Azure Service Bus bağlamaları
description: Azure Service Bus Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: tdykstra
ms.openlocfilehash: 0e9e7dcab208d1ffd8410a02a7c1cd713d11b277
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753498"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure işlevleri için Azure Service Bus bağlamaları

Bu makalede Azure işlevlerinde Azure Service Bus bağlamaları ile nasıl çalışılacağını açıklar. Tetikler ve Service Bus kuyrukları ve konuları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Hizmet veri yolu bağlamaları sağlanan [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi, sürüm 2.x. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/) GitHub depo.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Hizmet veri yolu bağlamaları sağlanan [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi, sürüm 3.x. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/) GitHub depo.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Hizmet veri yolu kuyruğu ya da konu iletilerine yanıt için Service Bus tetikleyici kullanın. 

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) okuyan [ileti meta veri](#trigger---message-metadata) ve bir hizmet veri yolu kuyruk iletisi günlüğe kaydeder:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

Bu örnek için Azure işlevleri sürümdür 1.x; 2.x için [erişim hakları parametreyi](#trigger---configuration).
 
### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama Service Bus tetikleyici gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev okur [ileti meta veri](#trigger---message-metadata) ve bir hizmet veri yolu kuyruk iletisi günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

C# betik kod aşağıdaki gibidir:

```cs
using System;

public static void Run(string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F # örnek

Aşağıdaki örnek, bağlama Service Bus tetikleyici gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlevi bir Service Bus kuyruk iletisi günlüğe kaydeder. 

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

F # betik kod aşağıdaki gibidir:

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama Service Bus tetikleyici gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev okur [ileti meta veri](#trigger---message-metadata) ve bir hizmet veri yolu kuyruk iletisi günlüğe kaydeder. 

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

JavaScript kodu şöyledir:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), Service Bus tetikleyici yapılandırmak için aşağıdaki öznitelikler kullanın:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin Oluşturucusu kuyruk veya konu ve abonelik adını alır. Azure işlevleri sürümünde 1.x, bağlantının erişim hakları de belirtebilirsiniz. Erişim hakları belirtmezseniz, varsayılan değer `Manage`. Daha fazla bilgi için bkz: [tetikleyici - yapılandırma](#trigger---configuration) bölümü.

  Aşağıda, bir dize parametresi ile kullanılan öznitelik gösteren bir örnek verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliği kullanmak için Service Bus hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)

  Kullanılacak hizmet veri yolu hesabını belirtmek için başka bir yol sağlar. Oluşturucusu hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

Hizmet veri yolu hesabı aşağıdaki sırayla belirlenir:

* `ServiceBusTrigger` Özniteliğin `Connection` özelliği.
* `ServiceBusAccount` Aynı parametre olarak uygulanan öznitelik `ServiceBusTrigger` özniteliği.
* `ServiceBusAccount` İşlevi için uygulanan öznitelik.
* `ServiceBusAccount` Sınıfına uygulanan öznitelik.
* "AzureWebJobsServiceBus" uygulama ayarı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `ServiceBusTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "ServiceBusTrigger" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction** | yok | "İçin" ayarlanması gerekir. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodu kuyruk veya konu iletisinde temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return" ayarlayın. | 
|**queueName**|**QueueName**|İzlemek için sırasının adı.  Yalnızca bir konu için bir sıra izliyorsanız seçin.
|**topicName**|**TopicName**|İzlemek için konu adı. Yalnızca bir sıra için bir konu izliyorsanız seçin.|
|**varlığıyla subscriptionName**|**Varlığıyla SubscriptionName**|İzlemek için Abonelik adı. Yalnızca bir sıra için bir konu izliyorsanız seçin.|
|**Bağlantı**|**Bağlantı**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyServiceBus." adlı "MyServiceBus" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan hizmet veri yolu bağlantı dizesi "AzureWebJobsServiceBus" adlı uygulama ayarını kullanır.<br><br>Bir bağlantı dizesi edinmek için gösterilen adımları izleyin [yönetim kimlik bilgileri elde](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). Bağlantı dizesi, belirli bir kuyruğa ya da konu bunlarla sınırlı olmamak bir hizmet veri yolu ad alanı için olmalıdır. |
|**erişimHakları**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, hangi gösterir `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izni, `accessRights` "dinlemek için". Aksi halde, çalışma zamanı gerektiren işlemleri yapmaya başarısız olabilir işlevleri hakları yönetin. Azure işlevleri sürümünde 2.x, bu özellik kullanılamıyor depolama SDK'ın en son sürümünü desteklemediğinden işlemleri yönetme.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve C# betiği aşağıdaki parametre türleri kuyruk veya konu ileti için kullanabilirsiniz:

* `string` -İleti metni ise.
* `byte[]` -İkili veriler için kullanışlıdır.
* JSON, ileti içeriyorsa, özel bir tür - Azure işlevleri JSON verilerini seri durumdan dener.
* `BrokeredMessage` -Seri durumdan çıkarılmış iletiyle verir [BrokeredMessage.GetBody<T>()](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemi.

Bu parametreler için Azure işlevleri sürümü olan 1.x; 2.x için kullanmak [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) yerine `BrokeredMessage`.

JavaScript'te, kuyruk veya konu ileti kullanarak erişim `context.bindings.<name from function.json>`. Hizmet veri yolu ileti işlevine bir dize veya JSON nesnesi olarak geçirilir.

## <a name="trigger---poison-messages"></a>Tetikleyici - zarar iletileri

Zehirli ileti işleme denetlenen veya Azure işlevlerinde yapılandırılmamış. Hizmet veri yolu kendisi zarar iletileri işler.

## <a name="trigger---peeklock-behavior"></a>Tetikleyici - PeekLock davranışı

İşlevler çalışma zamanı bir iletisinde aldığı [PeekLock modu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Çağırır `Complete` işlevi başarıyla tamamlanırsa ileti veya çağrıları `Abandon` işlevi başarısız olursa. İşlev süreden uzun çalışırsa `PeekLock` zaman aşımı, kilit işlevi çalıştığı sürece otomatik olarak yenilenir. 

İşlevler 1.x yapılandırmanıza olanak sağlayan `autoRenewTimeout` içinde *host.json*, hangi eşlendiğini [OnMessageOptions.AutoRenewTimeout](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout?view=azure-dotnet#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout). Bu ayar için izin verilen en fazla 10 dakika için varsayılan 5 dakika olan işlevler zaman sınırı artırabilirsiniz ancak Service Bus belgelerine göre 5 dakikadır. Hizmet veri yolu yenileme sınırını aşacağından Service Bus işlevleri için daha sonra bunu yapmak istemezsiniz.

## <a name="trigger---message-metadata"></a>Tetikleyici - ileti meta verileri

Hizmet veri yolu tetikleyici birkaç sağlar [meta veri özelliklerini](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Bu özellikler, diğer bağlamaları bağlama ifadelerinde bir parçası olarak ya da kodunuzu parametre olarak kullanılabilir. Özelliklerini bunlar [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sınıfı.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Teslimat sayısı.|
|`DeadLetterSource`|`string`|Sahipsiz kaynağı.|
|`ExpiresAtUtc`|`DateTime`|Sona erme zamanı, UTC.|
|`EnqueuedTimeUtc`|`DateTime`|Sıraya alınan zamanı, UTC.|
|`MessageId`|`string`|Hizmet veri yolu yinelenen iletileri tanımlamak için etkinleştirilirse kullanabileceğiniz bir kullanıcı tanımlı bir değer.|
|`ContentType`|`string`|Gönderici ve alıcı uygulama belirli mantığı tarafından kullanılan bir içerik türü tanımlayıcısı.|
|`ReplyTo`|`string`|Sıra adrese yanıt.|
|`SequenceNumber`|`Int64`|Bir iletiyi hizmet veri yolu tarafından atanmış benzersiz bir numara.|
|`To`|`string`|Adres Gönder.|
|`Label`|`string`|Uygulama özel etiket.|
|`CorrelationId`|`string`|Bağıntı Kimliği|
|`Properties`|`IDictionary<String,Object>`|Uygulama özel ileti özellikleri.|

Bkz: [kod örnekleri](#trigger---example) bu makalede daha önce bu özellikleri kullanın.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#servicebus) dosyası Service Bus tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-service-bus.md)]

## <a name="output"></a>Çıktı

Kuyruk veya konu iletileri göndermek için Azure Service Bus çıkış bağlama kullanın.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) Service Bus kuyruğu ileti gönderir:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama Service Bus çıkış gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. 15 dakikada bir kuyruk iletisi göndermek için Zamanlayıcı tetikleyicisi işlevini kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Tek bir ileti oluşturan bir C# kodu şöyledir:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Birden çok ileti oluşturan burada'nın C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıktı - F # örnek

Aşağıdaki örnek, bağlama Service Bus çıkış gösterir bir *function.json* dosyası ve bir [F # betik işlevi](functions-reference-fsharp.md) bağlama kullanır. 15 dakikada bir kuyruk iletisi göndermek için Zamanlayıcı tetikleyicisi işlevini kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Tek bir ileti oluşturan bir F # kodu şöyledir:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama Service Bus çıkış gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. 15 dakikada bir kuyruk iletisi göndermek için Zamanlayıcı tetikleyicisi işlevini kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Aşağıda, tek bir ileti oluşturur JavaScript kodu verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Aşağıda, birden fazla ileti oluşturur JavaScript kodu verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = [];
    context.bindings.outputSbQueue.push("1 " + message);
    context.bindings.outputSbQueue.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs).

Özniteliğin Oluşturucusu kuyruk veya konu ve abonelik adını alır. Bağlantının erişim hakları de belirtebilirsiniz. Erişim hakları ayarlama seçme açıklandığı [çıktı - yapılandırma](#output---configuration) bölümü. Aşağıda, işlevin dönüş değeri için uygulanan öznitelik gösteren bir örnek verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliği kullanmak için Service Bus hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

Kullanabileceğiniz `ServiceBusAccount` öznitelik sınıfı, yöntemi veya parametre düzeyinde kullanılacak hizmet veri yolu hesabını belirtin.  Daha fazla bilgi için bkz: [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `ServiceBus` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "ServiceBus" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction** | yok | Out"için" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**Adı** | yok | Sıra veya işlev kodu konudaki temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return" ayarlayın. | 
|**queueName**|**QueueName**|Kuyruk adı.  Yalnızca bir konu için sıraya ileti göndermek istiyorsanız ayarlayın.
|**topicName**|**TopicName**|İzlemek için konu adı. Yalnızca bir kuyruk için konu ileti göndermek istiyorsanız ayarlayın.|
|**Bağlantı**|**Bağlantı**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyServiceBus." adlı "MyServiceBus" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan hizmet veri yolu bağlantı dizesi "AzureWebJobsServiceBus" adlı uygulama ayarını kullanır.<br><br>Bir bağlantı dizesi edinmek için gösterilen adımları izleyin [yönetim kimlik bilgileri elde](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). Bağlantı dizesi, belirli bir kuyruğa ya da konu bunlarla sınırlı olmamak bir hizmet veri yolu ad alanı için olmalıdır.|
|**erişimHakları**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, hangi gösterir `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izni, `accessRights` "dinlemek için". Aksi halde, çalışma zamanı gerektiren işlemleri yapmaya başarısız olabilir işlevleri hakları yönetin. Azure işlevleri sürümünde 2.x, bu özellik kullanılamıyor depolama SDK'ın en son sürümünü desteklemediğinden işlemleri yönetme.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Azure işlevlerinde 1.x, çalışma zamanı oluşturur sıranın mevcut değil ve ayarladığınız `accessRights` için `manage`. İşlevler sürümünde 2.x, kuyruk veya konu zaten mevcut olmalıdır; bir kuyruk veya yok konu belirtirseniz, işlev başarısız olur. 

C# ve C# betiği aşağıdaki parametre türleri için çıktı bağlama kullanabilirsiniz:

* `out T paramName` - `T` JSON seri hale getirilebilir türler olabilir. Parametre değeri null ise, işlev çıktığında işlevleri null bir nesne ile iletisi oluşturur.
* `out string` -İşlevi çıktığında parametre değeri null ise, işlevleri oluşturmaz bir ileti.
* `out byte[]` -İşlevi çıktığında parametre değeri null ise, işlevleri oluşturmaz bir ileti.
* `out BrokeredMessage` -İşlevi çıktığında parametre değeri null ise, işlevleri oluşturmaz bir ileti.
* `ICollector<T>` veya `IAsyncCollector<T>` - birden çok ileti oluşturmak için. Çağırdığınızda bir ileti oluşturulur `Add` yöntemi.

Zaman uyumsuz işlevlerde dönüş değerini kullanın veya `IAsyncCollector` yerine bir `out` parametresi.

Bu parametreler için Azure işlevleri sürümü olan 1.x; 2.x için kullanmak [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) yerine `BrokeredMessage`.

JavaScript'te, kuyruk veya konu başlığı kullanarak erişim `context.bindings.<name from function.json>`. Bir dize, bir bayt dizisi veya (JSON'a seri durumdan) bir Javascript nesnesi atayabileceğiniz `context.binding.<name>`.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Service Bus | [Hizmet veri yolu hata kodları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Hizmet veri yolu sınırları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
