---
title: Azure olay kılavuz olay işleyicileri
description: İçin Azure olay kılavuz desteklenen olay işleyicileri açıklar
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: tomfitz
ms.openlocfilehash: 7c012bdf025a352788aec2d2d70bab33d7914577
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849551"
---
# <a name="event-handlers-in-azure-event-grid"></a>Azure olay kılavuzunda olay işleyicileri

Olay işleyici olay burada gönderilen yerdir. İşleyicisi olayını işlemek için bazı başka bir eylem alır. Çeşitli Azure hizmetlerine olayları işlemek için otomatik olarak yapılandırılır. Tüm Web kancası olayları işlemek için de kullanabilirsiniz. Web kancası olayları işlemek için Azure üzerinde barındırılan gerekmez. Olay kılavuz yalnızca HTTPS Web kancası uç noktalarını destekler.

Bu makalede her olay işleyicisi içeriğe bağlantılar sağlar.

## <a name="azure-automation"></a>Azure Otomasyonu

Otomatik runbook olayları işlemek için Azure Otomasyonu kullanın.

|Unvan  |Açıklama  |
|---------|---------|
|[Azure Otomasyonu olay kılavuz ve Microsoft ekipleri ile tümleştirme](ensure-tags-exists-on-new-virtual-machines.md) |Bir olay gönderen bir sanal makine, oluşturun. Sanal Makine etiketleri ve bir Microsoft Teams kanalına gönderilen bir ileti tetikleyen bir Otomasyon runbook'u olay tetiklenir. |

## <a name="azure-functions"></a>Azure İşlevleri

Azure işlevleri sunucusuz olaylarına yanıt olarak kullanın.

İşleyici olarak Azure İşlevleri’ni kullanırken, genel HTTP tetikleyicileri yerine Event Grid tetikleyicisini kullanın. Event Grid, Event Grid İşlevi tetikleyicilerini otomatik olarak doğrular. Genel HTTP tetikleyicileri ile [doğrulama yanıtını](security-authentication.md#webhook-event-delivery) uygulamanız gerekir.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure işlevleri için olay kılavuz tetikleyici](../azure-functions/functions-bindings-event-grid.md) | Olay kılavuz tetikleyici işlevler kullanılarak genel bakış. |
| [Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme](resize-images-on-storage-blob-upload-event.md) | Kullanıcılar web uygulaması aracılığıyla görüntüleri depolama hesabına yükleyin. Depolama blobu oluşturulduğunda, olay kılavuz karşıya yüklenen resmi yeniden boyutlandırır işlev uygulaması için bir olay gönderir. |
| [Veri ambarına büyük veri akışı yapma](event-grid-event-hubs-integration.md) | Olay hub'ları yakalama dosyası oluşturduğunda, olay kılavuz işlev uygulaması için bir olay gönderir. Uygulama yakalama dosyasını alır ve bir veri ambarına veri taşır. |
| [Azure olay kılavuz tümleştirme örnekler için Azure hizmet veri yolu](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay kılavuz, uygulama ve mantıksal uygulama çalışması için Service Bus konusundan iletileri gönderir. |

## <a name="event-hubs"></a>Event Hubs

Çözümünüzü olayları olayları işleyebileceğinden daha hızlı aldığında olay hub'ları kullanın. Uygulamanız kendi zamanlama olayları Event Hubs'dan onu adresindeki işler. Olay gelen olayları işlemek için işleme ölçeklendirebilirsiniz.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure Event Hubs ile Azure CLI ve olay kılavuz için rota özel olaylar](custom-event-to-eventhub.md) | Bir uygulama tarafından işlenmesi için bir olay hub'ına özel bir olay gönderir. |

## <a name="hybrid-connections"></a>Karma Bağlantılar

Bir kurumsal ağ içinde ve genel olarak erişilebilen bir uç nokta yok uygulamalara olayları göndermek için Azure geçişi karma bağlantıları kullanın.

|Unvan  |Açıklama  |
|---------|---------|
| [Karma bağlantı olayları göndermek](custom-event-to-hybrid-connection.md) | Dinleyici uygulama tarafından işlenmesi için mevcut bir karma bağlantıyı için özel bir olay gönderir. |

## <a name="logic-apps"></a>Logic Apps

Logic Apps olaylara yanıt verme için iş süreçlerini otomatikleştirmek için kullanın.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure Event Grid ve Logic Apps ile sanal makine değişikliklerini izleme](monitor-virtual-machine-changes-event-grid-logic-app.md) | Bir mantıksal uygulama, bir sanal makine yapılan değişiklikleri izler ve bu değişiklikleri hakkında e-posta gönderir. |
| [Logic Apps kullanarak Azure IOT Hub olayları hakkında e-posta bildirimleri gönder](publish-iot-hub-events-to-logic-apps.md) | Bir cihaz IOT hub'ınıza eklenen her zaman bir mantıksal uygulama bildirim e-posta gönderir. |
| [Azure olay kılavuz tümleştirme örnekler için Azure hizmet veri yolu](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Olay kılavuz, uygulama ve mantıksal uygulama çalışması için Service Bus konusundan iletileri gönderir. |

## <a name="queue-storage"></a>Kuyruk Depolama

Kuyruk depolama çekebilir gerek olaylarını almak için kullanın. Yanıt çok uzun sürdüğü uzun süre çalışan bir işlem olduğunda kuyruk depolama kullanabilir. Kuyruk depolama olayları göndererek uygulama çekebilir ve işlem kendi zamanlamaya göre olaylar.

|Unvan  |Açıklama  |
|---------|---------|
| [Azure CLI ve olay kılavuz Azure kuyruk depolamaya için rota özel olaylar](custom-event-to-queue-storage.md) | Özel olaylar için bir kuyruk depolama göndermeyi açıklar. |

## <a name="webhooks"></a>WebHooks

Olaylara yanıt özelleştirilebilir uç noktaları için Web kancası kullanın.

|Unvan  |Açıklama  |
|---------|---------|
| [Bir HTTP uç noktası olayları alma](receive-events.md) | Bir olay abonelikten olaylarını almak ve almak ve olayları serisini kaldırmak için bir HTTP uç noktası doğrulamak açıklar. |

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Hızlı bir şekilde olay Kılavuzu ile çalışmaya başlamak için bkz: [Azure olay kılavuz oluşturma ve rota özel olaylarla](custom-event-quickstart.md).
