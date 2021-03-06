---
title: Azure Event Hubs örnekleri | Microsoft Docs
description: Azure Event Hubs örnekleri
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2018
ms.author: sethm
ms.openlocfilehash: 9d2c38ac589e5120441daf972217e61738fd57a1
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131364"
---
# <a name="event-hubs-samples"></a>Olay hub'ları örnekleri 

Azure Event Hubs örnekleri kümesi anahtar özelliklerini gösteren [Azure Event Hubs](/azure/event-hubs/). Bu makalede, kategorilere ayırır ve her için bağlantılar ile birlikte kullanılabilir örnekleri açıklanmaktadır.

Bu yazma zaman Event Hubs örnekleri birkaç farklı yerlerde bulunur:

- [MSDN Geliştirici kod örnekleri](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

.NET Framework'ün farklı sürümleri hakkında daha fazla bilgi için bkz: [çerçeveler ve hedefleri](/dotnet/articles/standard/frameworks).

Daha fazla örnekleri olacaktır zamanla eklenir, böylece geri burada sık Güncelleştirmeleri denetle.

## <a name="net-standard"></a>.NET standart

Aşağıdaki örnekleri kullanarak olayları alıp göndermek nasıl ekleyebileceğiniz gösterilmektedir [Event Hubs istemcisi](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) için [.NET standart Kitaplığı](/dotnet/articles/standard/library).

### <a name="send-events"></a>Olayları gönderme 

[Göndermeye başlamak](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) örnek olayları bir event hub'ına gönderir .NET Core konsol uygulamasının nasıl yazılacağını gösterir.

### <a name="receive-events"></a>Olayları alma 

[İle olay işleyicisi konağı almaya başlamak](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) olay işleyicisi konağı kullanarak bir event hub iletileri alan bir .NET Core konsol uygulaması örnektir.

## <a name="net-framework"></a>.NET Framework   

Bu örnekler çeşitli hedefleme Azure Event Hubs özelliklerini göstermek [.NET Framework Kitaplığı](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Alınan olayların kullanıcıları bildir

[AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) örnek algılayıcılar veya diğer sistemler alınan veri kullanıcılarına bildirir.

### <a name="get-started-with-event-hubs"></a>Event Hubs kullanmaya başlayın 

[Olay hub'ları Başlarken](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) örnek olay hub'ları, bir olay hub'ı oluşturma, olay hub'ına olayları göndermek ve kullanarak olayları kullanma gibi temel özelliklerini gösteren [olay işleyicisi konağı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .

### <a name="scale-out-event-processing"></a>Olay işleme çıkışı ölçeklendirme 

[Olay işleme genişletme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) örnek nasıl kullanılacağını gösteren [olay işleyicisi konağı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) olay hub'ları akış tüketiminin iş yükünü dağıtmak için. Nasıl uygulandığını gösterir **EventProcessor** ve **EventProcessorFactory** olay akışının yönetilecek nesneleri. 

## <a name="next-steps"></a>Sonraki adımlar

.NET Framework sürümleri hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret ederek öğrenin:

- [Çerçeveler ve hedefler](/dotnet/articles/standard/frameworks)
- [.NET framework 4.6 ve 4.5](/dotnet/framework/index)

Aşağıdaki makalelerde Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

- [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
- [Event Hubs özellikleri](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)