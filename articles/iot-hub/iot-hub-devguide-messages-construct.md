---
title: Azure IOT Hub ileti biçimi anlama | Microsoft Docs
description: Geliştirici Kılavuzu - descibes biçimi ve IOT Hub iletilerini beklenen içerik.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: a1296565384e60117d883a1f1407362482ba1a3e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125022"
---
# <a name="create-and-read-iot-hub-messages"></a>Oluşturun ve IOT Hub iletilerini okuma

Protokoller üzerinde sorunsuz birlikte çalışabilirlik desteklemek için IOT Hub cihaz'e yönelik tüm protokoller için ortak bir ileti biçimine tanımlar. Bu ileti biçimi için kullanılan [CİHAZDAN buluta] [ lnk-d2c] ve [bulut-cihaz] [ lnk-c2d] iletileri. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Bir [IOT Hub iletisi] [ lnk-messaging] oluşur:

* Bir dizi *Sistem Özellikleri*. IOT hub'ı yorumlar ya da ayarlar özellikleri. Bu önceden tanımlanmış kümesidir.
* Bir dizi *uygulama özellikleri*. Uygulamayı tanımlayan dize özellikleri ve ileti gövdesi seri durumdan gerek olmadan erişim sözlüğü. IOT hub'ı hiçbir zaman bu özellikleri değiştirir.
* Donuk bir ikili gövdesi.

Özellik adlarını ve değerlerini içerebilir ASCII alfasayısal karakterler, artı ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` olduğunda:  

* HTTPS protokolünü kullanarak CİHAZDAN buluta iletiler gönderme.
* Bulut-cihaz iletilerini gönderin.

Kodlama ve kodunu çözme farklı protokolleriyle gönderilen iletiler hakkında daha fazla bilgi için bkz. [Azure IOT SDK'ları][lnk-sdks].

Aşağıdaki tabloda, IOT Hub iletilerini Sistem özelliklerinde kümesini listeler.

| Özellik | Açıklama |
| --- | --- |
| MessageId |İstek-yanıt desenlerinde kullanılan ileti için kullanıcı ayarlanabilir bir tanımlayıcı. Biçim: Bir büyük küçük harfe duyarlı dize (en çok 128 karakterden uzun), ASCII 7 bit alfasayısal karakterlerin + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Sıra numarası |Her bulut buluttan cihaza iletiye IOT Hub tarafından atanmış bir sayı (benzersiz cihaz kuyruk). |
| Alıcı |Belirtilen bir hedef [bulut-cihaz] [ lnk-c2d] iletileri. |
| ExpiryTimeUtc |Tarih ve saat ileti sonu. |
| EnqueuedTime |Tarih ve saat [bulut-cihaz] [ lnk-c2d] ileti, IOT Hub tarafından alındı. |
| CorrelationId |Genellikle istek, istek-yanıt desenleri MessageID içeren bir yanıt iletisi bir dize özelliği. |
| UserId |İletileri kaynağını belirtmek için kullanılan bir kimliği. İletileri IOT Hub tarafından oluşturulduğunda ayarlanır `{iot hub name}`. |
| ACK |Geri bildirim iletisi Oluşturucusu. Bu özellik bulut-cihaz iletilerini cihaz tarafından sonucunda iletinin Tüketim geri bildirim iletileri oluşturmak için IOT hub'ı istemek için kullanılır. Olası değerler: **hiçbiri** (varsayılan): hiçbir geri bildirim iletisi oluşturulur, **pozitif**: ileti tamamlanmışsa, bir geri bildirim iletisi **negatif**: alma bir iletinin süresi (veya en yüksek teslimat sayısı ulaşıldı varsa) ve cihaz tarafından tamamlanmasını olmadan geri bildirim iletisi veya **tam**: pozitif ve negatif. Daha fazla bilgi için [geri bildirim iletisi][lnk-feedback]. |
| ConnectionDeviceId |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik. İçerdiği **DeviceID** iletiyi gönderen cihazın. |
| ConnectionDeviceGenerationId |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik. İçerdiği **Generationıd** (olarak başına [cihaz kimlik özelliklerini][lnk-device-properties]) iletiyi gönderen cihazın. |
| ConnectionAuthMethod |CİHAZDAN buluta iletileri IOT Hub tarafından ayarlanmış bir kimlik doğrulama yöntemi. Bu özellik, iletinin gönderme cihazın kimliğini doğrulamak için kullanılan kimlik doğrulama yöntemi hakkında bilgi içerir. Daha fazla bilgi için [CİHAZDAN yanıltma buluta][lnk-antispoofing]. |
| CreationTimeUtc | Tarih ve saat ileti bir cihazda oluşturuldu. Bir cihaz, bu değeri açıkça ayarlamanız gerekir. |

## <a name="message-size"></a>İleti boyutu

IOT Hub ileti boyutu, bir protokol belirsiz şekilde, yalnızca gerçek yükü dikkate ölçer. Bayt cinsinden boyut aşağıdakilerden toplamı hesaplanır:

* Gövde boyutu bayt cinsinden.
* İleti sistemi özelliklerinin değerlerinin bayt cinsinden boyutu.
* Tüm kullanıcı özellik adlarının ve değerlerinin bayt cinsinden boyutu.

Bayt cinsinden boyutu dize uzunluğu eşittir özellik adlarını ve değerlerini ASCII karakter ile sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ında ileti boyutu sınırları hakkında daha fazla bilgi için bkz: [IOT Hub kotaları ve azaltma][lnk-quotas].

Oluşturun ve IOT hub'ı çeşitli programlama dillerinde iletileri okumak öğrenmek için bkz: [hızlı Başlangıçlar][lnk-get-started].

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
