---
title: Azure işlevleri için olay Kılavuzu tetikleyicisi
description: Azure işlevleri'nde Event Grid olayların nasıl işleneceğini anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/08/2018
ms.author: tdykstra
ms.openlocfilehash: 6678109414eaa71ced369e87e1cd15544fee5ee5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723808"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Azure işlevleri için olay Kılavuzu tetikleyicisi

Bu makalede nasıl yapılacağını açıklar [Event Grid](../event-grid/overview.md) Azure işlevleri'nde olayları.

Event Grid, içinde gerçekleşen olaylar hakkında bilgilendirmek için HTTP istekleri gönderen bir Azure hizmetidir; *yayımcılar*. Bir hizmet veya olay kaynaklı kaynak yayımcısıdır. Örneğin, bir Azure blob depolama hesabındaki bir yayımcı olur ve [blob karşıya yükleme veya silme bir olaydır](../storage/blobs/storage-blob-event-overview.md). Bazı [olayları Event Grid'e yayımlamak için yerleşik desteğe sahiptir. Azure Hizmetleri](../event-grid/overview.md#event-sources). 

Olay *işleyicileri* almak ve olay işleme. Azure işlevleri, çeşitli birini [Event Grid olaylarını işlemek için yerleşik desteğe sahip olan Azure Hizmetleri](../event-grid/overview.md#event-handlers). Bu makalede, bir olay Kılavuzu tetikleyicisi Event Grid'den gelen bir olay alındığında bir işlevi çağırmak için nasıl kullanılacağını öğrenin.

Tercih ederseniz, Event Grid olaylarını işlemek için bir HTTP tetikleyicisi kullanabilirsiniz; bkz: [HTTP tetikleyicisi bir Event Grid tetikleyici olarak kullanma](#use-an-http-trigger-as-an-event-grid-trigger) bu makalenin ilerleyen bölümlerinde. Olay iletildiğinde şu anda bir Event Grid tetikleyicisinin bir Azure işlev uygulaması için kullanamazsınız [CloudEvents şeması](../event-grid/cloudevents-schema.md). Bunun yerine bir HTTP tetikleyicisi kullanın.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Event Grid tetikleyicisinin sağlanan [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure işlevleri eventgrid uzantı](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Event Grid tetikleyicisinin sağlanan [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure işlevleri eventgrid uzantı](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example"></a>Örnek

Bir olay Kılavuzu tetikleyicisi için dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betiği (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

HTTP tetikleyicisi örneği için bkz. [HTTP tetikleyicisi kullanmayı](#use-an-http-trigger-as-an-event-grid-trigger) bu makalenin ilerleyen bölümlerinde.

### <a name="c-example"></a>C# örneği

Aşağıdaki örnek, bir işlevler gösterir 1.x [C# işlevi](functions-dotnet-class-library.md) için bağlar `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

Aşağıdaki örnek, bir işlevler gösterir 2.x [C# işlevi](functions-dotnet-class-library.md) için bağlar `EventGridEvent`:

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.Data.ToString());
        }
    }
}
```

Daha fazla bilgi için [paketleri](#packages), [öznitelikleri](#attributes), [yapılandırma](#configuration), ve [kullanım](#usage).

### <a name="c-script-example"></a>C# betiği örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

İşte bağlar işlevleri 1.x C# betik kodu `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

İşte bağlar işlevleri 2.x C# betik kodu `EventGridEvent`:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;

public static void Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.Data.ToString());
}
```

Daha fazla bilgi için [paketleri](#packages), [öznitelikleri](#attributes), [yapılandırma](#configuration), ve [kullanım](#usage).

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```
     
## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs) özniteliği.

İşte bir `EventGridTrigger` özniteliği bir yöntem imzası:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, TraceWriter log)
{
    ...
}
 ```

Tam bir örnek için bkz. [C# örneği](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya. Oluşturucu parametresi veya ayarlamak için özellikler yok `EventGridTrigger` özniteliği.

|Function.JSON özelliği |Açıklama|
|---------|---------|----------------------|
| **type** | Gerekli - kümesine olmalıdır `eventGridTrigger`. |
| **direction** | Gerekli - kümesine olmalıdır `in`. |
| **Adı** | Gereklidir - işlev kodu olay verileri alan parametresi için kullanılan bir değişken adı. |

## <a name="usage"></a>Kullanım

C# ve F # Azure işlevleri için 1.x İşlevler, aşağıdaki parametre türleri için Event Grid tetikleyicisinin kullanabilirsiniz:

* `JObject`
* `string`

Azure işlevleri'nde C# ve F # işlevleri için 2.x de aşağıdaki parametre türü için Event Grid tetikleyicisinin kullanılacak seçeneğiniz vardır:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`-Tüm olay türlerine abone ortak alanlar için özellikleri tanımlar.

> [!NOTE]
> Bağlanılacak çalışırsanız işlevleri v1'de `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`, derleyici bir "kullanım dışı" iletisini görüntülemek ve kullanmanızı öneriyoruz `Microsoft.Azure.EventGrid.Models.EventGridEvent` yerine. Daha yeni türü kullanmak için başvuru [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet paketini ve tam olarak nitelemek `EventGridEvent` tür adı ile önek tarafından `Microsoft.Azure.EventGrid.Models`. Bir C# betik işlevi NuGet paketlerine başvurmak hakkında daha fazla bilgi için bkz. [kullanarak NuGet paketleri](functions-reference-csharp.md#using-nuget-packages)

JavaScript işlevleri için parametre adlı tarafından *function.json* `name` özelliği, olay nesnesine bir başvuru içeriyor.

## <a name="event-schema"></a>Olay şeması

Bir Event Grid olay verilerini bir HTTP istek gövdesindeki bir JSON nesnesi olarak alınır. JSON, aşağıdaki örneğe benzer:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Gösterilen örnek bir öğeden oluşan bir dizidir. Event Grid, her zaman bir dizi gönderir ve dizideki birden fazla olay gönderebilir. Çalışma zamanının işlevinizi her dizi öğesi için bir kez çalıştırır.

JSON verilerini olay üst düzey özelliklere içeriğini sırasında tüm olay türleri arasında aynıdır `data` özelliği her olay türüne özeldir. Gösterilen örnekte, bir blob depolama olayı bulunur.

Ortak ve özel olay özellikleri açıklamaları için bkz. [olay özellikleri](../event-grid/event-schema.md#event-properties) Event Grid belgelerinde.

`EventGridEvent` Türü yalnızca üst düzey özellikler tanımlar; `Data` özelliği bir `JObject`. 

## <a name="create-a-subscription"></a>Abonelik oluşturma

Event Grid HTTP isteklerini almaya başlaması için işlevi çağıran uç nokta URL'sini belirten bir Event Grid aboneliği oluşturun.

### <a name="azure-portal"></a>Azure portalına

Azure portalında Event Grid tetikleyicisinin geliştirme işlevleri için seçin **Event Grid aboneliği Ekle**.

![Abonelik portalında oluşturma](media/functions-bindings-event-grid/portal-sub-create.png)

Bu bağlantıyı seçtiğinizde, portal açar **olay aboneliği oluşturma** sayfasında uç nokta URL'si ile doldurulmuş.

![Doldurulmuş bir uç nokta URL'si](media/functions-bindings-event-grid/endpoint-url.png)

Azure portalını kullanarak abonelikleri oluşturma hakkında daha fazla bilgi için bkz. [özel olay oluşturma - Azure portalı](../event-grid/custom-event-quickstart-portal.md) Event Grid belgelerinde.

### <a name="azure-cli"></a>Azure CLI

Kullanarak bir abonelik oluşturmak için [Azure CLI'yı](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), kullanın [az eventgrid olay aboneliği oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) komutu.

Komut işlevi çağıran uç nokta URL'sini gerektirir. Aşağıdaki örnek URL deseni gösterir:

```
https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}
```

Sistem, bir olay Kılavuzu tetikleyicisi için uç nokta URL'si dahil edilecek bir yetkilendirme anahtar anahtardır. Aşağıdaki bölümde, sistem anahtarını almak açıklanmaktadır.

(Sistem anahtarı için bir yer tutucu ile) bir blob depolama hesabına abone olan bir örnek aşağıda verilmiştir:

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name glengablobstorage --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://glengastorageevents.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=LUwlnhIsNtSiUjv/sNtSiUjvsNtSiUjvsNtSiUjvYb7XDonDUr/RUg==
```

Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [blob depolama hızlı başlangıcı](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) veya diğer Event Grid hızlı başlangıçları.

### <a name="get-the-system-key"></a>Sistem anahtarını alma

Aşağıdaki API (HTTP GET) kullanarak sistem anahtarı alabilirsiniz:

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={adminkey}
```

Gerektiriyorsa Bu olduğundan bir yönetici API'sini, [yönetici anahtarını](functions-bindings-http-webhook.md#authorization-keys). (Bir olay Kılavuzu tetikleyicisi işlevi çağırma için) sistemi anahtarı (Yönetim görevleri işlev uygulamasını gerçekleştirdiği) için yönetici anahtarı ile karıştırmayın. Bir olay Kılavuzu konu başlığına abone olduğunuzda, sistem anahtarını kullandığınızdan emin olun.

Sistem anahtarı sağlayan yanıtın bir örnek aşağıda verilmiştir:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Daha fazla bilgi için [yetkilendirme anahtarları](functions-bindings-http-webhook.md#authorization-keys) makaledeki HTTP tetikleyici başvurusu. 

Alternatif olarak, anahtar değeri kendiniz belirlemek için bir HTTP PUT gönderebilirsiniz.

## <a name="local-testing-with-viewer-web-app"></a>Yerel Görüntüleyici web uygulamasını test etme

Bir olay Kılavuzu tetikleyicisi test etmek için yerel olarak, Event Grid HTTP istekleri, kaynaktan bulutta yerel makinenize teslim gerekir. Bunu yapmanın bir yolu, çevrimiçi ve el ile bunları yerel makinenizde yeniden gönderme istekleri yakalayarak şöyledir:

2. [Görüntüleyicisi web uygulaması oluşturma](#create-a-viewer-web-app) , olay iletileri yakalar.
3. [Bir Event Grid aboneliği oluşturmak](#create-an-event-grid-subscription) , Olay Görüntüleyicisi'ni uygulamaya gönderir.
4. [Bir isteği](#generate-a-request) ve Görüntüleyicisi uygulamadan istek gövdesine kopyalayın.
5. [El ile istek gönderin](#manually-post-the-request) , Event Grid localhost URL'sini işlevi tetikleyin.

Bitirdiğinizde testi, aynı abonelik için üretim uç noktası güncelleştirerek kullanabilirsiniz. Kullanım [az eventgrid olay aboneliği güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) Azure CLI komutu.

### <a name="create-a-viewer-web-app"></a>Görüntüleyicisi web uygulaması oluşturma

Yakalama olay iletileri basitleştirmek için dağıtabileceğiniz bir [önceden oluşturulmuş bir web uygulaması](https://github.com/dbarkol/azure-event-grid-viewer) , olay iletileri görüntüler. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fdbarkol%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

![Yeni siteyi görüntüleme](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturun

Test etmek istediğiniz türde bir Event Grid aboneliği oluşturun ve URL web uygulamanızdan uç noktası olarak olay bildirimi için verin. Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir. Bu nedenle, tam URL'si olur `https://<your-site-name>.azurewebsites.net/api/updates`

Azure portalını kullanarak abonelikleri oluşturma hakkında daha fazla bilgi için bkz. [özel olay oluşturma - Azure portalı](../event-grid/custom-event-quickstart-portal.md) Event Grid belgelerinde.

### <a name="generate-a-request"></a>Bir isteği oluştur

Web uygulaması uç noktanıza HTTP trafiğini oluşturacak bir olayı tetikleyin.  Örneğin, bir blob depolama aboneliği oluşturduysanız, karşıya yükleyin veya bir blobu silme. Web uygulamanıza istek gösterilir, istek gövdesine kopyalayın.

Abonelik doğrulama isteği ilk alınır; tüm doğrulama isteklerini yoksayar ve olay istek kopyalayın.

![Web uygulamasından istek gövdesine kopyalayın](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>El ile istek gönderin

Event Grid işleviniz yerel olarak çalıştırın.

Gibi bir araç kullanın [Postman](https://www.getpostman.com/) veya [curl](https://curl.haxx.se/docs/httpscripting.html) bir HTTP POST isteği oluşturmak için:

* Ayarlanmış bir `Content-Type: application/json` başlığı.
* Ayarlanmış bir `aeg-event-type: Notification` başlığı.
* RequestBin veri istek gövdesine yapıştırın. 
* Şu biçimi kullanarak URL'ye Event Grid tetikleyici işlevinizin gönderin:

```
http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

`functionName` Parametresi, belirtilen adı olmalıdır `FunctionName` özniteliği.

Aşağıdaki ekran görüntüleri, üst bilgilerini göster ve istek gövdesi postman'deki:

![Postman üstbilgileri](media/functions-bindings-event-grid/postman2.png)

![İstek gövdesinde Postman](media/functions-bindings-event-grid/postman.png)

Olay Kılavuzu tetikleyicisi işlevi yürütür ve günlükleri aşağıdaki örneğe benzer gösterir:

![Örnek olay Kılavuzu tetikleyicisi işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Yerel ngrok ile test etme

Bir olay Kılavuzu tetikleyicisi yerel olarak test etmek için başka bir HTTP bağlantısı Internet ile geliştirme bilgisayarınızda arasında otomatik hale getirmek için yoludur. Adlı bir açık kaynak aracı ile bunu yapabilirsiniz [ngrok](https://ngrok.com/):

3. [Ngrok uç nokta oluşturma](#create-an-ngrok-endpoint).
4. [Olay Kılavuzu tetikleyicisi işlevi çalıştırmak](#run-the-event-grid-trigger-function).
5. [Bir Event Grid aboneliği oluşturmak](#create-a-subscription) , olayları ngrok uç noktasına gönderir.
6. [Bir olayı tetikleyin](#trigger-an-event).

Bitirdiğinizde testi, aynı abonelik için üretim uç noktası güncelleştirerek kullanabilirsiniz. Kullanım [az eventgrid olay aboneliği güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) Azure CLI komutu.

### <a name="create-an-ngrok-endpoint"></a>Ngrok uç nokta oluşturma

İndirme *ngrok.exe* gelen [ngrok](https://ngrok.com/)ve aşağıdaki komutu çalıştırın:

```
ngrok http -host-header=localhost 7071
```

Host-komut localhost üzerinde çalıştığında, İşlevler çalışma zamanı localhost gelen istekleri beklediği için üstbilgi parametresi gereklidir. çalışma zamanı yerel olarak çalıştığında 7071 varsayılan bağlantı noktası numarasıdır.

Komut şuna benzer bir çıktı oluşturur:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

Https://{subdomain}.ngrok.io URL'si, Event Grid aboneliğiniz için kullanacaksınız.

### <a name="run-the-event-grid-trigger-function"></a>Olay Kılavuzu tetikleyicisi işlevi çalıştırın

Abonelik oluşturulurken, işlevi yerel olarak çalışıyor olması gerekir böylece ngrok URL'si, Event Grid ile özel işlem elde edemez. Aksi takdirde, gönderilen doğrulama yanıtı değil ve abonelik oluşturma başarısız olur.

### <a name="create-a-subscription"></a>Abonelik oluşturma

Test etmek istediğiniz türde bir Event Grid aboneliği oluşturun ve şu biçimi kullanarak ngrok uç noktanıza, verin:

```
https://{subdomain}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

`functionName` Parametresi, belirtilen adı olmalıdır `FunctionName` özniteliği.

Azure CLI kullanarak bir örnek aşağıda verilmiştir:

```
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName=EventGridTrigger
```

Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [abonelik oluşturma](#create-a-subscription) bu makalenin üst kısmındaki.

### <a name="trigger-an-event"></a>Bir olay tetikleme

HTTP trafiğini ngrok uç noktanıza oluşturacak bir olayı tetikleyin.  Örneğin, bir blob depolama aboneliği oluşturduysanız, karşıya yükleyin veya bir blobu silme.

Olay Kılavuzu tetikleyicisi işlevi yürütür ve günlükleri aşağıdaki örneğe benzer gösterir:

![Örnek olay Kılavuzu tetikleyicisi işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>HTTP tetikleyicisi bir Event Grid tetikleyici olarak kullanma

Olayları bir Event Grid INSTEAD OF tetikleyicisi bir HTTP tetikleyicisini kullanarak işleyebilmesi için event Grid olaylarını HTTP isteklerini alınır. Böylece olası bir nedeni hakkında daha fazla denetime işlevi çağıran bir uç nokta URL'si almaktır. Başka bir nedenle olayları almaya ihtiyacınız olduğunda [CloudEvents şeması](../event-grid/cloudevents-schema.md). Şu anda, Event Grid tetikleyicisinin CloudEvents şeması desteklemiyor. Bu bölümdeki örneklerde, Event Grid şema hem de CloudEvents şeması için çözümleri gösterir.

HTTP tetikleyicisi kullanıyorsanız, Event Grid tetikleyicisinin otomatik olarak yaptığı için kod yazmak zorundasınız:

* Doğrulama yanıt gönderir bir [abonelik doğrulama isteği](../event-grid/security-authentication.md#webhook-event-delivery).
* İstek gövdesinde yer alan olay dizi öğesi için bir sefer işlevi çağırır.

İşlevi yerel olarak veya Azure'da çalıştırıldığında çağırmak için kullanılacak URL hakkında daha fazla bilgi için bkz. [HTTP tetikleyici bağlama başvuru belgeleri](functions-bindings-http-webhook.md)

### <a name="event-grid-schema"></a>Olay ızgarası şeması

Aşağıdaki örnek C# kod HTTP tetikleyicisi için olay Kılavuzu tetikleyicisi davranışını taklit eder. Bu örnek olay ızgarası şema teslim olaylar için kullanın.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"], 
        "Microsoft.EventGrid.SubscriptionValidationEvent", 
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.Info("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.Info($"Subject: {eventGridEvent.Subject}");
        log.Info($"Time: {eventGridEvent.EventTime}");
        log.Info($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Aşağıdaki örnek JavaScript kodunu HTTP tetikleyicisi için Event Grid tetikleyici davranışını taklit eder. Bu örnek olay ızgarası şema teslim olaylar için kullanın.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = messages[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        // Event Grid schema delivers events in an array.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

Döngü içinde olay işleme kodunuzu gider `messages` dizisi.

### <a name="cloudevents-schema"></a>CloudEvents şeması

Aşağıdaki örnek C# kod HTTP tetikleyicisi için olay Kılavuzu tetikleyicisi davranışını taklit eder.  CloudEvents şeması teslim edilen olaylara için bu örneği kullanın.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    if (message.Type == JTokenType.Array)
    {
        // If the request is for subscription validation, send back the validation code.
        if (string.Equals((string)message[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
        {
            log.Info("Validate request received");
            return req.CreateResponse<object>(new
            {
                validationResponse = message[0]["data"]["validationCode"]
            });
        }
    }
    else
    {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        log.Info($"Source: {message["source"]}");
        log.Info($"Time: {message["eventTime"]}");
        log.Info($"Event data: {message["data"].ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Aşağıdaki örnek JavaScript kodunu HTTP tetikleyicisi için Event Grid tetikleyici davranışını taklit eder. CloudEvents şeması teslim edilen olaylara için bu örneği kullanın.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var message = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (message.length > 0 && message[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = message[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Event Grid hakkında daha fazla bilgi edinin](../event-grid/overview.md)
