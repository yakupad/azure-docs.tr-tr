---
title: Node.js ile Azure Service Bus konu başlıklarını ve aboneliklerini kullanma | Microsoft Docs
description: Service Bus konuları ve Abonelikleri, Azure'da bir Node.js uygulamasından kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: d3a7ebd135f705a6a3ea91feb4e037a9ed6d0c79
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38705005"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs"></a>Nasıl yapılır kullanım Service Bus konuları ve abonelikleri ile Node.js

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu kılavuzda, Service Bus konuları ve abonelikleri Node.js uygulamalarından kullanmayı açıklar. Kapsanan senaryolar şunlardır:

- Konuları ve abonelikleri oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konu başlığına ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme 

Konuları ve abonelikleri hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Bir Node.js uygulaması oluşturma ile ilgili yönergeler için bkz. [Azure Web sitesine bir Node.js uygulaması oluşturma ve dağıtma], [Node.js bulut hizmeti] [ Node.js Cloud Service] Windows kullanma PowerShell veya WebMatrix ile Web sitesi.

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırma
Service Bus hizmetini kullanmak için Node.js Azure paketini indirin. Bu paket, hizmet veri yolu REST Hizmetleri ile iletişim kuran bir dizi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Bir paketi almasını düğüm paketi Yöneticisi'ni (NPM) kullanın
1. Bir komut satırı arabirimi gibi açın **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (Unix).
2. Örnek uygulamanızı oluşturduğunuz klasöre gidin.
3. Tür **npm yükleme azure** komut penceresinde, sonuç aşağıdaki çıktı:

   ```
       azure@0.7.5 node_modules\azure
   ├── dateformat@1.0.2-1.2.3
   ├── xmlbuilder@0.4.2
   ├── node-uuid@1.2.0
   ├── mime@1.2.9
   ├── underscore@1.4.4
   ├── validator@1.1.1
   ├── tunnel@0.0.2
   ├── wns@0.5.3
   ├── xml2js@0.2.7 (sax@0.5.2)
   └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
   ```
3. El ile çalıştırabileceğiniz **ls** doğrulamak için komutu bir **düğüm\_modülleri** klasörü oluşturuldu. Bu klasörün içinde Bul **azure** Service Bus konu başlıklarını erişmek için ihtiyacınız olan kitaplıkları içeren paket.

### <a name="import-the-module"></a>Modülü içeri aktarın
Not Defteri veya başka bir metin düzenleyicisi kullanarak, aşağıdaki üst kısmına ekleyin **server.js** uygulamanın dosya:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantı kurma
Azure modülünü ortam değişkenini okur `AZURE_SERVICEBUS_CONNECTION_STRING` önceki adımda elde ettiğiniz bağlantı dizesi için "kimlik bilgilerini edinin." Bu ortam değişkeni ayarlanmazsa çağırırken hesap bilgilerini belirtmelisiniz `createServiceBusService`.

Azure bulut hizmeti için ortam değişkenlerini ayarlama örneği için bkz: [Node.js bulut depolama hizmetiyle][Node.js Cloud Service with Storage].



## <a name="create-a-topic"></a>Konu başlığı oluşturma
**ServiceBusService** nesnesi konuları ile çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **ServiceBusService** nesne. En ekleme **server.js** dosya, azure modülü içeri aktarmak için deyim sonra:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Eğer `createTopicIfNotExists` üzerinde **ServiceBusService** nesne, belirtilen konu (varsa) döndürülen veya belirtilen ada sahip yeni bir konu oluşturulur. Aşağıdaki kod `createTopicIfNotExists` oluşturmak veya adlı konuya bağlanmak `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

`createServiceBusService` Yöntemi, varsayılan ileti yaşam süresi veya en büyük konu boyutu gibi konu başlığı ayarlarını geçersiz kılmak etkinleştirdiğiniz ek seçenekler de destekler. 

Aşağıdaki örnekte, bir dakikalık bir süresi ile en büyük konu boyutu 5 GB ila ayarlar:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri kullanılarak gerçekleştirilen işlemler için uygulanabilir **ServiceBusService**. Filtreleme işlemleri içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. Filtreler, imza ile bir yöntem uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri ön işleme gerçekleştirdikten sonra yöntemi çağıran `next`ve bir geri çağırma imzayla geçirir:

```javascript
function (returnObject, finalCallback, next)
```

İşleme sonra bu geri çağırma `returnObject` (istek sunucuya yanıt), geri çağırma gerekir ya da diğer filtrelerle işleme devam etmek için (varsa)'ın yanındaki çağırma veya çağırma `finalCallback` hizmet çağırma sonlandırmak için.

Yeniden deneme mantığı uygulayan iki filtre (**ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**), Node.js için Azure SDK’sına dahil edilir. Aşağıdaki oluşturur bir **ServiceBusService** kullanan nesne **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Abonelik oluşturma
Konu abonelikleri ile de oluşturulur **ServiceBusService** nesne. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

> [!NOTE]
> Abonelikleri kalıcı ya da bunlar kadar veya ilişkili konu ile silindi. Uygulamanız mantıksal bir abonelik oluşturmak için içeriyorsa, ilk abonelik kullanarak mevcut olup olmadığını denetleyin `getSubscription` yöntemi.
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** bir abonelik oluşturulurken kullanılan varsayılan filtresini filtredir. **MatchAll** filtresini kullandığınızda konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek AllMessages adlı bir abonelik oluşturulur ve varsayılan **MatchAll** filtre.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
İletiler konu başlığına gönderilen kapsamı için bir özel konu aboneliği içinde gösterilmesi gerekir sağlayan filtreler de oluşturabilirsiniz.

En esnek filtre türü, abonelikler tarafından desteklenen **SqlFilter**, SQL92 kümesini uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL Filtresi ile kullanılabilen ifadeler hakkında daha fazla ayrıntı için gözden [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] söz dizimi.

Filtre eklenebilir aboneliği kullanarak `createRule` yöntemi **ServiceBusService** nesne. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak tanır.

> [!NOTE]
> Varsayılan filtre tüm yeni abonelikler için otomatik olarak uygulandığından, önce varsayılan filtreyi kaldırmalısınız veya **MatchAll** , belirttiğiniz diğer filtreleri geçersiz kılar. Varsayılan kural kullanarak kaldırabilirsiniz `deleteRule` yöntemi **ServiceBusService** nesne.
>
>

Aşağıdaki örnekte adlı bir abonelik oluşturur `HighMessages` ile bir **SqlFilter** , yalnızca özel bir bulunduran iletileri seçen `messagenumber` özelliği 3'ten büyük:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturur `LowMessages` ile bir **SqlFilter** , yalnızca bulunduran iletileri seçen bir `messagenumber` özelliğini daha az veya eşit 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ne zaman bir ileti hemen gönderilir `MyTopic`, abone alıcılara teslim edilecek `AllMessages` konu aboneliği ve seçmeli olarak teslim edilen alıcılar abone için `HighMessages` ve `LowMessages` (bağlı konu abonelikleri ileti içeriği).

## <a name="how-to-send-messages-to-a-topic"></a>Bir konuya ileti göndermek nasıl
Bir Service Bus konusuna bir ileti göndermek için uygulamanızı kullanmalısınız `sendTopicMessage` yöntemi **ServiceBusService** nesne.
Service Bus konu başlıklarına gönderilen iletiler **BrokeredMessage** nesneleri.
**BrokeredMessage** nesneleri olan bir standart özellikler kümesi (gibi `Label` ve `TimeToLive`), bir uygulamaya özgü özel özellikleri tutmak için kullanılan sözlüğü ve dize verileri gövdesi içerir. Bir uygulama iletisinin gövdesini dize değeri geçirerek ayarlayabilirsiniz `sendTopicMessage` ve standart özellikleri varsayılan değerlerle doldurulacak gerekli.

Aşağıdaki örnek nasıl beş test iletisi göndereceğinizi gösterir `MyTopic`. `messagenumber` Her ileti özelliği değerinin değişen döngü yinelemeyi (Bu alacak abonelikleri belirler):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konu başlığında tutulan ileti sayısına bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu sınırı yoktur. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Kullanarak bir abonelik iletilerin alındığı `receiveSubscriptionMessage` metodunda **ServiceBusService** nesne. Varsayılan olarak, bunlar okurken iletileri abonelikten silinir. Ancak, isteğe bağlı parametre ayarlayabilirsiniz `isPeekLock` için **true** (Özet) okuyun ve iletiyi abonelikten silmeden kilitlemek için.

Okuma ve ileti alma işleminin bir parçası olarak silmeye varsayılan davranışı en basit modeldir ve uygulamanın bir arıza olması durumunda bir iletiyi işlememeye izin dayanabilir senaryolar için en iyi şekilde çalışır. Bu davranışı anlamak için hangi tüketici alma isteği bildirdiğini ve ardından işlenmeden önce kilitleniyor bir senaryo düşünün. Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `isPeekLock` parametrenin ayarlanmış **true**, alma, kaçırılan iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında kullanmak için sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve uygulamaya döndürür.
Uygulama iletiyi işler (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar **deleteMessage** yöntemi ve bir parametre olarak silmek için ileti geçirir. **DeleteMessage** yöntemi iletiyi kullanılmış olarak işaretler ve abonelikten kaldırır.

Aşağıdaki örnek nasıl ileti alındı ve işlenen kullanarak gösterir `receiveSubscriptionMessage`. Örnek ilk alır ve bir ileti 'MessageNumber' abonelikten siler ve ardından gelen 'HighMessages' aboneliğinden bir ileti alır `isPeekLock` true olarak ayarlanmış. Ardından iletiyi kullanarak siler `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        })
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlockMessage` metodunda **ServiceBusService** nesne. Bu yöntem Bus hizmetinin Abonelikteki iletinin kilidini açmasına ve tekrar alınabilir hale neden olur. Bu örnekte aynı kullanıcı uygulama veya başka bir kullanıcı uygulama.

Ayrıca abonelikte kilitlenen iletiye ilişkin bir zaman aşımı vardır. (Örneğin, uygulama çökerse), kilit zaman aşımı süresi dolmadan önce iletiyi işlemek uygulama başarısız olursa, Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilir hale getirir.

Uygulama iletiyi ancak önce çökmesi durumunda `deleteMessage` yöntemi çağrıldığında, yeniden başlatıldığında ileti uygulamaya yeniden teslim. Bu davranışı genellikle adlandırılır *en az bir kez işlenmesini*. Diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulamanızı mantık eklemeniz gerekir. Kullanabileceğiniz **MessageID** özelliğini iletinin teslim denemeleri arasında sabit kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konuları ve abonelikleri kalıcıdır ve açıkça olmalıdır aracılığıyla silindi [Azure portalında] [ Azure portal] veya programlama yoluyla.
Aşağıdaki örnekte adlı konu nasıl silineceği `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki örnek adlı bir abonelik silme gösterir `HighMessages` gelen `MyTopic` konu:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Sonraki Adımlar
Hizmet veri yolu konuları hakkındaki temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* Bkz: [kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions].
* [SqlFilter][SqlFilter] için API başvurusu.
* Ziyaret [düğüm için Azure SDK'sı] [ Azure SDK for Node] github deposu.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure Web sitesine bir Node.js uygulaması oluşturma ve dağıtma]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

