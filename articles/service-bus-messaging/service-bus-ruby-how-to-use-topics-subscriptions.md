---
title: Service Bus konu başlıklarını (Ruby) kullanma | Microsoft Docs
description: Azure'da Service Bus konuları ve abonelikleri kullanmayı öğrenin. Kod örnekleri, Ruby uygulamalarına yönelik yazılır.
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38712974"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Service Bus konuları ve abonelikleri ile Ruby kullanma
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Service Bus konuları ve abonelikleri Ruby uygulamalardan kullanmayı açıklar. Senaryoları ele alınmaktadır **konuları ve Abonelikleri, gönderme, abonelik filtreleri oluşturma oluşturma** bir konuya **bir abonelikten ileti alma**, ve **siliniyor konuları ve abonelikleri**. Konuları ve abonelikleri hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Konu başlığı oluşturma
**Azure::ServiceBusService** nesnesi konuları ile çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **Azure::ServiceBusService** nesne. Bir konu oluşturmak için kullanın `create_topic()` yöntemi. Aşağıdaki örnek, bir konu oluşturur veya varsa hataları yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

De geçirebilirsiniz bir **Azure::ServiceBus::Topic** nesne, Canlı ya da en büyük sıra boyutu ileti süresi gibi varsayılan konu ayarlarını geçersiz kılmanıza olanak sağlayan ek seçeneklere sahip. Aşağıdaki örnek, en büyük sıra boyutu 5 GB ve saat 1 dakika için canlı olarak ayarlanması gösterir:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonelik oluşturma
Konu abonelikleri ile de oluşturulur **Azure::ServiceBusService** nesne. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

Abonelikler kalıcıdır ve ya da bunlar kadar var olmaya devam eder veya ilişkili konu ile silindi. Uygulamanız mantıksal bir abonelik oluşturmak için içeriyorsa, ilk getSubscription yöntemi kullanarak abonelik zaten var olup denetlemelisiniz.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** filtresi, yeni bir abonelik oluşturulurken filtre belirtilmeyen durumlarda kullanılan varsayılan filtredir. Zaman **MatchAll** filtresinin kullanılacağının, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek "tüm iletileri" adlı bir abonelik oluşturulur ve varsayılan **MatchAll** filtre.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Hangi belirtmenize olanak sağlayan filtreler de tanımlayabilirsiniz konu başlığına gönderilen iletilerin görünmesini içinde belirli bir abonelik.

En esnek filtre türü, abonelikler tarafından desteklenen **Azure::ServiceBus::SqlFilter**, SQL92 kümesini uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL Filtresi ile kullanılabilen ifadeler hakkında daha fazla ayrıntı için gözden [SqlFilter](service-bus-messaging-sql-filter.md) söz dizimi.

Filtreleri kullanarak için bir abonelik ekleyebilmeniz için `create_rule()` yöntemi **Azure::ServiceBusService** nesne. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak sağlar.

Varsayılan filtre tüm yeni abonelikler için otomatik olarak uygulanan olduğundan, öncelikle varsayılan filtreyi kaldırmanız gerekir veya **MatchAll** , belirttiğiniz diğer filtreleri geçersiz kılar. Varsayılan kural kullanarak kaldırabilirsiniz `delete_rule()` metodunda **Azure::ServiceBusService** nesne.

Aşağıdaki örnek "iletileri yüksek" adlı bir abonelik oluşturur bir **Azure::ServiceBus::SqlFilter** , yalnızca özel bir bulunduran iletileri seçen `message_number` özelliği 3'ten büyük:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturur `low-messages` ile bir **Azure::ServiceBus::SqlFilter** , yalnızca bulunduran iletileri seçen bir `message_number` özelliğini daha az veya eşit 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Ne zaman bir ileti hemen gönderilir `test-topic`, bu her zaman olması girmediklerinden abone alıcılar için `all-messages` konu aboneliği ve seçmeli olarak teslim edilen alıcılar abone için `high-messages` ve `low-messages` (bağlı olarak konu abonelikleri ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Bir Service Bus konusuna bir ileti göndermek için uygulamanızı kullanmalısınız `send_topic_message()` metodunda **Azure::ServiceBusService** nesne. Service Bus konu başlıklarına gönderilen iletiler örnekleridir **Azure::ServiceBus::BrokeredMessage** nesneleri. **Azure::ServiceBus::BrokeredMessage** nesneleri olan bir standart özellikler kümesi (gibi `label` ve `time_to_live`), bir uygulamaya özgü özel özellikleri tutmak için kullanılan sözlüğü ve dize verileri gövdesi içerir. Bir uygulama iletisinin gövdesini dize değeri geçirerek ayarlayabilirsiniz `send_topic_message()` yöntemi ve tüm gerekli standart özellikleri varsayılan değerlerle doldurulur.

Aşağıdaki örnek nasıl beş test iletisi göndereceğinizi gösterir `test-topic`. Unutmayın `message_number` her ileti özel özellik değerini değişen döngü yinelemeyi (Bu aboneliği aldığı belirler):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Kullanarak bir abonelik iletilerin alındığı `receive_subscription_message()` metodunda **Azure::ServiceBusService** nesne. Varsayılan olarak, iletiler read(peak) ve abonelikten silmeden kilitli. Okuma ve ayarlayarak abonelikten ileti silme `peek_lock` seçeneğini **false**.

Varsayılan davranışı, okuma ve silme ayrıca, atlanan iletilere uygulamaları desteklemek mümkün kılar bir iki aşamalı işlemi yapar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar `delete_subscription_message()` yöntemi ve iletiyi bir parametre olarak silinmesini sağlar. `delete_subscription_message()` Yöntemi iletiyi kullanılıyor olarak işaretler ve abonelikten kaldırır.

Varsa `:peek_lock` parametrenin ayarlanmış **false**, okuma ve ileti silme basit model haline gelir ve içinde bir uygulama tolere edebilen bir arıza olması durumunda bir iletiyi işlememeye izin senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

Aşağıdaki örnek nasıl ileti alındı ve işlenen kullanarak gösterir `receive_subscription_message()`. Örneğin ilk alır ve bir ileti siler `low-messages` kullanarak abonelik `:peek_lock` kümesine **false**, başka bir ileti aldıktan sonra `high-messages` ve ardından iletiyi kullanaraksiler`delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlock_subscription_message()` metodunda **Azure::ServiceBusService** nesne. Bu işlem, Service Bus hizmetinin abonelikteki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca abonelikte kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini sonra kilit zaman aşımı dolmadan ' i tıklatın ve tekrar kullanılabilir hale getirin.

Uygulama iletiyi ancak önce çökmesi durumunda, `delete_subscription_message()` yöntemi çağrılır, ardından yeniden başlatıldığında ileti uygulamaya yeniden teslim. Buna genellikle denir *en az bir kez işleme*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu mantık, genellikle kullanılarak gerçekleştirilir `message_id` özelliğini iletinin teslim denemeleri arasında sabit kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konuları ve abonelikleri kalıcıdır ve açıkça olmalıdır aracılığıyla silindi [Azure portalında] [ Azure portal] veya programlama yoluyla. Aşağıdaki örnekte adlı konu nasıl silineceği `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki kodu adlı abonelik nasıl silineceği `high-messages` gelen `test-topic` konu:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Sonraki adımlar
Hizmet veri yolu konuları hakkındaki temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* Bkz: [kuyruklar, konular ve abonelikler](service-bus-queues-topics-subscriptions.md).
* [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter) için API başvurusu
* Ziyaret [Ruby için Azure SDK'sı](https://github.com/Azure/azure-sdk-for-ruby) github deposu.

[Azure portal]: https://portal.azure.com
