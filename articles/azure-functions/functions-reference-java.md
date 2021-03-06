---
title: Azure işlevleri için Java Geliştirici Başvurusu | Microsoft Docs
description: Java ile işlevleri geliştirmek nasıl anlayın.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancalarını, dinamik işlem, sunucusuz mimarisi, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw
ms.openlocfilehash: 3f63cb5a16b74458f9b53fddaea13a61ec1196a5
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31514028"
---
# <a name="azure-functions-java-developer-guide"></a>Azure işlevleri Java Geliştirici Kılavuzu

## <a name="programming-model"></a>Programlama modeli 

Azure işlevinizi işler giriş ve çıkışı üretir bir durum bilgisiz sınıfı yöntemi olmalıdır. Örnek yöntemleri yazmaya izin verse de işlevinizi sınıfının örneği alanlara bağımlı olmamalıdır. Tüm işlevi yöntemleri olmalıdır bir `public` erişim değiştiricisi.

## <a name="triggers-and-annotations"></a>Tetikleyiciler ve ek açıklamaları

Genellikle bir Azure işlevi nedeniyle dış bir tetikleyici çağrılır. Bu tetikleyici ve ilişkili girdilerinden işlemek ve bir veya daha fazla çıkış üretmek işlevinizi gerekir.

Java ek açıklamalar dahil edilmiştir `azure-functions-java-core` giriş ve çıkış yöntemlerinizi için bağlamak için paket. Aşağıdaki tabloda, desteklenen giriş tetikleyiciler ve ek açıklamaları bağlama çıkış bulunmaktadır:

Bağlama | Ek Açıklama
---|---
CosmosDB | Yok
HTTP | <ul><li>`HttpTrigger`</li><li>`HttpOutput`</li></ul>
Mobile Apps | Yok
Notification Hubs | Yok
Depolama blobu | <ul><li>`BlobTrigger`</li><li>`BlobInput`</li><li>`BlobOutput`</li></ul>
Depolama Kuyruğu | <ul><li>`QueueTrigger`</li><li>`QueueOutput`</li></ul>
Depolama tablosu | <ul><li>`TableInput`</li><li>`TableOutput`</li></ul>
Zamanlayıcı | <ul><li>`TimerTrigger`</li></ul>
Twilio | Yok

Tetikleyici girişleri ve çıkışları içinde de tanımlanabilir [function.json](/azure/azure-functions/functions-reference#function-code) uygulamanız için.

> [!IMPORTANT] 
> Bir Azure Storage hesabı yapılandırmanız gerekir, [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure depolama Blob, kuyruk veya tablo Tetikleyiciler yerel olarak çalıştırmak için.

Örnek: ek açıklamalarını kullanma

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Ek açıklamalar olmadan yazılan aynı işlevi:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

karşılık gelen `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="data-types"></a>Veri Türleri

Java tüm veri türleri yerel türleri dahil olmak üzere girdi ve çıktı verileri için kullanmakta serbestsiniz; Java türleri ve içinde tanımlanan özel Azure türleri özelleştirilmiş `azure-functions-java-core` paket. Çalışma zamanı çalışır Azure işlevleri, kodunuzu tarafından istenen türü içine alınan giriş dönüştürün.

### <a name="strings"></a>Dizeler

İşlev yöntemlere geçirilen değerleri dönüştürülür dizelere karşılık gelen işlevinin giriş parametresi türü türü ise `String`. 

### <a name="plain-old-java-objects-pojos"></a>Eski basit Java nesnelerini (Pojo'lar)

İşlev yöntemi giriş Java türü görüyorsa JSON ile biçimlendirilmiş dizeler Java türlerine cast. Bu dönüştürme, JSON girişleri işlevlerinizi geçirmek ve kendi kodunuzu dönüştürme uygulamak zorunda kalmadan, kodunuzda Java türleriyle çalışmasına olanak sağlar.

Aynı işlevleri girişleri gerekir olarak kullanılan POJO'ya türleri `public` bunlar kullanıldığından içinde işlev yöntemi olarak erişim değiştiricisi. POJO'ya sınıfı alanları bildirmek yok `public`. Örneğin, bir JSON dizesinde `{ "x": 3 }` aşağıdaki POJO'ya türe dönüştürülüp yapabiliyor:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>İkili veriler

İkili veriler olarak temsil edilir bir `byte[]` Azure işlevleri kodunuzda. İkili girişleri veya çıkışları işlevlerinizi için ayarlayarak bağlama `dataType` , function.json alanındaki `binary`:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

Ardından işlevi kodunuzda kullanın:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Kullanım `OutputBinding<byte[]>` bir ikili çıktıyı bağlaması yapma türü.


## <a name="function-method-overloading"></a>İşlev yöntemi aşırı yüklemesi

Aşırı yükleme işlevi yöntemleri aynı adı ile ancak farklı türleri için izin verilir. Örneğin, her ikisi de sağlayabilirsiniz `String echo(String s)` ve `String echo(MyType s)` bir sınıf ve Azure işlevleri çalışma zamanı tarafından çağrılacak hangisinin inceleyin gerçek giriş türü karar (HTTP giriş, MIME türü için `text/plain` için müşteri adayları `String` sırada `application/json` temsil eden `MyType`).

## <a name="inputs"></a>Girişler

Azure işlevlerinin iki kategoriye giriş bölünen: Tetikleyici giriş biridir ve diğer ek giriş olabilir. Farklı olmasına rağmen `function.json`, kullanım Java kodda aynıdır. Aşağıdaki kod parçacığını bir örnek olarak atalım:

```java
package com.example;

import com.microsoft.azure.serverless.functions.annotation.BindingName;
import java.util.Optional;

public class MyClass {
    public static String echo(Optional<String> in, @BindingName("item") MyObject obj) {
        return "Hello, " + in.orElse("Azure") + " and " + obj.getKey() + ".";
    }

    private static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

`@BindingName` Ek açıklama kabul eden bir `String` bağlama/tetikleyici adını temsil eden özellik tanımlanan `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "put" ],
      "route": "items/{id}"
    },
    {
      "type": "table",
      "name": "item",
      "direction": "in",
      "tableName": "items",
      "partitionKey": "Example",
      "rowKey": "{id}",
      "connection": "ExampleStorageAccount"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```

Bu işlev çağrıldığında, HTTP isteği yükü isteğe başarılı şekilde `String` bağımsız değişkeni için `in` ve bir Azure Table Storage `MyObject` türü geçirilen bağımsız değişken `obj`. Kullanım `Optional<T>` null olabilir işlevlerinizi girişleri işlemek için türü.

## <a name="outputs"></a>Çıkışlar

Çıkış hem dönüş değeri veya çıkış parametreleri ifade edilebilir. Yalnızca bir çıktı ise, dönüş değeri kullanmak için önerilir. Birden çok çıkışlar için çıkış parametreleri kullanmak zorunda.

Dönüş değeri çıkış en basit biçimidir, yalnızca herhangi bir türde değerini döndürür ve Azure işlevleri çalışma zamanı (örneğin, bir HTTP yanıtının) gerçek tür dön sıralama dener. İçinde `functions.json`, kullandığınız `$return` çıkış bağlama adından farklı.

Birden çok çıktı değeri oluşturmak üzere kullanmak `OutputBinding<T>` tanımlı türünü `azure-functions-java-core` paket. Bir HTTP yanıtının yapmak ve bir kuyruğuna ileti gönderme gerekiyorsa, şöyle yazabilirsiniz:

```java
package com.example;

import com.microsoft.azure.serverless.functions.OutputBinding;
import com.microsoft.azure.serverless.functions.annotation.BindingName;

public class MyClass {
    public static String echo(String body, 
    @QueueOutput(queueName = "messages", connection = "AzureWebJobsStorage", name = "queue") OutputBinding<String> queue) {
        String result = "Hello, " + body + ".";
        queue.setValue(result);
        return result;
    }
}
```

Çıktı bağlamasında tanımlayan `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "queue",
      "name": "queue",
      "direction": "out",
      "queueName": "messages",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```
## <a name="specialized-types"></a>Özel türler

Bazen bir işlev girişleri ve çıkışları üzerinde denetim ayrıntılı olması gerekir. Özelleştirilmiş türlerinde `azure-functions-java-core` paket isteği bilgiyi işleyebilir ve bir HTTP tetikleyicisi dönüş durumu uyarlamak için sağlanır:

| Özel tür      |       Hedef        | Tipik kullanım                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP tetikleyici     | Yöntemi, üstbilgileri veya sorguları Al |
| `HttpResponseMessage<T>` | HTTP bağlama çıktı | Dönüş durumu 200 dışında   |

> [!NOTE] 
> Aynı zamanda `@BindingName` HTTP üstbilgilerine ve sorguları almak için ek açıklama. Örneğin, `@BindingName("name") String query` sorgular ve HTTP istek üstbilgilerinin tekrarlanan ve bu değeri yönteme geçirin. Örneğin, `query` olacaktır `"test"` istek URL'si ise `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Meta Veriler

Meta veri gelen HTTP üstbilgileri, HTTP sorguları gibi farklı kaynaklardan ve [meta verileri tetiklemek](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Kullanım `@BindingName` değerini almak için meta veri adı ile birlikte ek açıklama.

Örneğin, `queryValue` aşağıdaki kod parçacığını olacaktır `"test"` istenen URL ise `http://{example.host}/api/metadata?name=test`.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.serverless.functions.annotation.*;

public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="functions-execution-context"></a>İşlevler yürütme bağlamı

Azure işlevleri yürütme ortamı etkileşim `ExecutionContext` tanımlanan nesne `azure-functions-java-core` paket. Kullanmak `ExecutionContext` çağırma bilgileri ve işlevleri çalışma zamanı bilgileri, kodunuzda kullanılacak nesne.

### <a name="logging"></a>Günlüğe kaydetme

İşlevler çalışma zamanı Günlükçü erişimi aracılığıyla kullanılabilir `ExecutionContext` nesnesi. Bu günlükçünün Azure İzleyici bağlıdır ve bayrağı uyarılar ve işlev yürütme sırasında oluşan hatalar sağlar.

Aşağıdaki kod örneği, alınan istek gövdesi boş olduğunda bir uyarı iletisi günlüğe kaydeder.

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="environment-variables"></a>Ortam değişkenleri

Genellikle, güvenlik nedenleriyle kaynak koddan gizli bilgi ayıklamak için tercih edilir. Bu kodu kaynak kodu depoları için kimlik bilgileri yanlışlıkla diğer geliştiricilerine sağlamadan yayımlanmasına izin verir. Bu, hem Azure işlevleri yerel olarak çalıştırırken ve işlevlerinizi Azure'a dağıtırken ortam değişkenleri yalnızca kullanılarak sağlanabilir.

Azure işlevleri çalıştırırken kolayca ortam değişkenlerini ayarlama yerel olarak, bu değişkenleri local.settings.json dosyasına eklemek tercih edebilirsiniz. Bir işlev proje kök dizininde mevcut değilse, oluşturmak çekinmeyin. İşte dosyanın aşağıdaki gibi görünmelidir:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Her anahtar / değer eşlemesindeki `values` harita oluşturulacak kullanılabilir çalışma zamanında çağırarak erişilebilir bir ortam değişkeni olarak `System.getenv("<keyname>")`, örneğin, `System.getenv("AzureWebJobsStorage")`. Ekleme ek anahtar / değer çiftlerini kabul edilir ve uygulama önerilir.

> [!NOTE]
> Bu yaklaşım alınmışsa olması emin local.settings.json ekleme olup olmadığını dikkate alınacak dosyayı deponuza yoksay dosya, kaydedilmiş olmaması.

Böylece test yerel olarak ve ne zaman Azure'a dağıtıldığında kodunuzu eşdeğer işlevleri kodunuzla şimdi bu ortam değişkenlerine bağlı olarak, size aynı anahtarı ayarlama / işlevi uygulama ayarlarınızı çiftlerinde değer Azure portalı oturum açabilir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Uzaktan hata ayıklama Java Azure işlevleri ile Visual Studio Code](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
