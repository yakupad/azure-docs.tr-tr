---
title: Web kancaları kurallarında Azure IOT Central oluşturun | Microsoft Docs
description: Web kancaları, Azure IOT kuralları tetiklendiğinde diğer uygulamaları otomatik olarak bildirim sağlaması için Orta oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 07/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1e21076cafe21e6c0efcdf5a8146278eabd9ebc4
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228110"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Web kancası eylemleri kurallarında Azure IOT Central oluşturun

Web kancaları, diğer uygulama ve hizmetlere uzaktan izleme ve bildirimler için IOT Central uygulamanızın bağlamanızı sağlar. Web kancaları, diğer uygulama ve hizmetlerin bir kuralı tetiklendiğinde IOT Central uygulamanızda bağlantı otomatik olarak bildirin. Kural tetiklendiğinde IOT Central uygulamanızı diğer uygulamanın HTTP uç noktasına bir POST isteği gönderir. Yükü cihaz ayrıntıları ve kural tetikleyici ayrıntılarını içerir. 

## <a name="how-to-set-up-the-webhook"></a>Web kancası ' ayarlama
Bu örnekte, Web kancalarını kullanma kuralları tetiklendiğinde bildirim almak için RequestBin için bağlanır. 

1. Açık [RequestBin](http://requestbin.net/). 
1. Yeni bir RequestBin ve kopyasını oluşturma **URL'sini**. 
1. Oluşturma bir [telemetri kural](howto-create-telemetry-rules.md) veya [olayı kuralı](howto-create-event-rules.md). Kuralı kaydetmek ve yeni bir eylem ekleyin.
![Web kancası oluşturma ekranı](media/howto-create-webhooks/webhookcreate.png)
1. Web kancası eylemi seçin ve bir görünen ad girin ve URL'sini geri çağırma URL'si olarak yapıştırın. 
1. Kuralı Kaydet

Artık bir kural başlatıldığında RequestBin içinde görünen yeni bir istek görmeniz gerekir.

## <a name="payload"></a>Yük
Kural tetiklendiğinde, bir HTTP POST isteği bir json yükü ölçümleri, cihaz, kural ve uygulama ayrıntılarını içeren geri çağırma URL'si için yapılır. Telemetri kuralı için yük aşağıdakine benzer:

```json
{
    "id": "ID",
    "timestamp": "date-time",
    "device" : {
        "id":"ID",
        "name":  "Refrigerator1",
        "simulated" : true,
        "deviceTemplate":{
            "id": "ID",
            "version":"1.0.0"
        },
        "properties":{
            "device":{
                "firmwareversion":"1.0"
            },
            "cloud":{
                "location":"One Microsoft Way"
            }
        },
        "measurements":{
            "telemetry":{
                "temperature":20,
                "pressure":10
            }
        }

    },
    "rule": {
        "id": "ID",
        "name": "High temperature alert",
        "enabled": true,
        "deviceTemplate": {
            "id":"GUID",
            "version":"1.0.0"
        }
    },
    "application": {
        "id": "ID",
        "name": "Contoso app",
        "subdomain":"contoso-app"
    }
}
```

## <a name="known-limitations"></a>Bilinen sınırlamalar
Şu anda abone olma ve aboneliği, bir API aracılığıyla bu Web kancaları'ndan programlı hiçbir yolu yoktur.

Bu özelliği geliştirmeye ilişkin fikirler varsa, önerilerinizi gönderin bizim [Uservoice forumumuzu](https://feedback.azure.com/forums/911455-azure-iot-central).

## <a name="next-steps"></a>Sonraki adımlar
Ayarlama ve Web kancalarını kullanma öğrendiniz, önerilen sonraki keşfetmek için adımdır [Microsoft Flow, iş akışları oluşturarak](howto-add-microsoft-flow.md).
