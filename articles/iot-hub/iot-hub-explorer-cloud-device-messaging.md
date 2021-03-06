---
title: Azure IOT Hub bulut cihaz iothub-Gezgini ile Mesajlaşma yönetme | Microsoft Docs
description: Bulut (D2C) iletileri ve bulut Azure IOT Hub cihaz (C2D) iletileri göndermek için İzleyici aygıta iothub-explorer CLI aracı kullanmayı öğrenin.
author: rangv
manager: ''
keywords: ileti, cihaz, cihaz ileti bulut IOT hub'ı buluta bulut cihaz ıothub Gezgini
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: cd8456efb4d8e0b84695f97d379a1ea83e19aba2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634242"
---
# <a name="use-iothub-explorer-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Aygıt ve IOT hub'ı arasında ileti gönderme ve alma için iothub-Gezgini'ni kullanın

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[ıothub explorer](https://github.com/azure/iothub-explorer) IOT hub'ı yönetimini kolaylaştırır komutları sayıda sahiptir. Bu öğretici ıothub explorer Cihazınızı ve IOT hub'ınız arasında ileti gönderme ve alma için nasıl kullanılacağını odaklanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Bilgi edineceksiniz

Cihaz bulut iletilerini izlemek ve bulut-cihaz iletilerini göndermek için iothub-explorer kullanmayı öğrenin. Cihaz bulut iletilerini Cihazınızı toplayan ve IOT hub'ınıza gönderir algılayıcı verilerini olabilir. Bulut-cihaz iletilerini IOT hub'ınızı aygıtınıza aygıtınıza bağlı bir LED blink gönderir komutları olabilir.

## <a name="what-you-will-do"></a>Ne yapacağını

- Cihaz bulut iletilerini izlemek için iothub-Gezgini'ni kullanın.
- Bulut-cihaz iletilerini göndermek için iothub-Gezgini'ni kullanın.

## <a name="what-you-need"></a>Ne gerekiyor

- Öğretici [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md) , aşağıdaki gereksinimleri ele alınmaktadır tamamlandı:
  - Etkin bir Azure aboneliği.
  - Azure IOT hub'ı aboneliğinizdeki.
  - Azure IOT hub'ına iletileri gönderen bir istemci uygulaması.
- iothub-Gezgini. ([Yükleme ıothub explorer](https://github.com/azure/iothub-explorer))

## <a name="monitor-device-to-cloud-messages"></a>Cihaz bulut iletilerini izleme

Cihazınızın IOT hub'ına gönderilen iletileri izlemek için aşağıdaki adımları izleyin:

1. Bir konsol penceresi açın.
1. Şu komutu çalıştırın:

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > Alma `<device-id>` ve `<IoTHubConnectionString>` IOT hub'ınızı gelen. Önceki öğreticiyi bitirdikten sonra emin olun. Veya kullanmayı deneyebilirsiniz `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` varsa `HostName`, `SharedAccessKeyName` ve `SharedAccessKey`.

## <a name="send-cloud-to-device-messages"></a>Buluttan cihaza iletileri gönderme

IOT hub'ından, aygıta bir ileti göndermek için aşağıdaki adımları izleyin:

1. Bir konsol penceresi açın.
1. Bir oturum, aşağıdaki komutu çalıştırarak, IOT hub'ına başlatın:

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. Bir ileti, aşağıdaki komutu çalıştırarak aygıtınıza gönder:

   ```bash
   iothub-explorer send <device-id> <message>
   ```

Komut, cihazınıza bağlı olan ve aygıtınıza iletiyi gönderir ışığı yanıp.

> [!Note]
> Cihazın iletiyi aldıktan sonra geri IOT hub'ına ayrı ack komutu göndermek gerek yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz bulut iletilerini izlemek ve bulut-cihaz iletilerini IOT cihaz ile Azure IOT hub'ı arasında göndermek nasıl öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
