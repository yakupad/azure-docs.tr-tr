---
title: Azure IOT hub'ı işleri anlama | Microsoft Docs
description: Geliştirici Kılavuzu - birden fazla cihazda çalıştırılacak işleri zamanlama, IOT hub'ınıza bağlı. İşler, etiketler ve istenen özelliklerini güncelleştirmek ve birden fazla cihazda doğrudan metotları çağırma.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 460c7d24b2810de41e20ea803ded2ea988613f10
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223805"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Birden fazla cihazda işleri zamanlama

Azure IOT hub'ı etkinleştirir yapı taşları gibi bir dizi [cihaz ikizi özelliklerini ve etiketlerini] [ lnk-twin-devguide] ve [doğrudan yöntemler][lnk-dev-methods].  Genellikle, arka uç uygulamaları güncelleştirme ve IOT cihazlarını toplu ve zamanlanan tarihte ile etkileşim kurmak cihaz yöneticilerin ve operatörlerin olanak tanır.  İşleri zamanlanan saatte cihaz ikizi güncelleştirmeleri ve bir dizi cihazda karşı doğrudan metotları yürütme.  Örneğin, operatör başlatır ve bir grup için yapı işlemlerini kesintiye uğratan olmazdı birer 43 ve 3 kat oluşturmadaki cihazı yeniden başlatmak için bir iş izleyen bir arka uç uygulaması kullanmanız gerekir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Planlamak ve ilerlemeyi izlemek gerektiğinde aşağıdaki etkinliklerin herhangi bir dizi cihazda işlemleriyle göz önünde bulundurun:

* İstenen özellikleri güncelleştirme
* Etiketleri güncelleştirin
* Doğrudan metotları çağırma

## <a name="job-lifecycle"></a>İş yaşam döngüsü
İşleri çözüm arka ucu tarafından başlatılan ve IOT Hub tarafından korunur.  Hizmet kullanıma yönelik bir URI aracılığıyla bir işi başlatabilirsiniz (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) ve hizmeti kullanıma yönelik bir URI aracılığıyla yürütülürken bir işin ilerlemesi için sorgu (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`). Bir iş başlatıldıktan sonra işleri çalıştırma durumunu yenilemek için bir iş sorgusu çalıştırın.

> [!NOTE]
> Bir işi başlattığınızda, özellik adları ve değerleri yalnızca US-ASCII yazdırılabilir içerebilir dışında aşağıdaki, tüm alfasayısal: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`.

## <a name="jobs-to-execute-direct-methods"></a>Doğrudan yöntemler çalıştırılacak işleri
Aşağıdaki kod parçacığını yürütmeye yönelik HTTPS 1.1 istek ayrıntılarını gösterir bir [doğrudan yöntemini] [ lnk-dev-methods] bir iş kullanarak cihazları bir dizi:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }

Sorgu koşulu, bir tek bir cihaz kimliği veya cihaz aşağıdaki örneklerde gösterildiği gibi kimlikleri listesi de olabilir:

```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[IOT Hub sorgu dili] [ lnk-query] ek ayrıntılı IOT Hub sorgu dili kapsar.

## <a name="jobs-to-update-device-twin-properties"></a>Cihaz ikizi özelliklerini güncelleştirmek için işlemler
Aşağıdaki kod parçacığında, bir iş kullanarak cihaz ikizi özelliklerini güncelleştirmek için HTTPS 1.1 istek ayrıntılarını gösterir:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }

## <a name="querying-for-progress-on-jobs"></a>Devam eden işler üzerinde sorgulama
Aşağıdaki kod parçacığı için işleri sorgulamak için HTTPS 1.1 istek ayrıntılarını gösterir:

    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

Yanıttan continuationToken sağlanır.  

Her bir cihaz kullanarak iş yürütme durumunu sorgulayabilirsiniz [cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili][lnk-query].

## <a name="jobs-properties"></a>İş özellikleri
Aşağıdaki liste, sorgulanırken işleri veya iş sonuçları için kullanılabilir ilgili açıklamalar ve özelliklerini gösterir.

| Özellik | Açıklama |
| --- | --- |
| **jobId** |Uygulama için İş Kimliği sağlanmadı. |
| **startTime** |Uygulama, işin başlangıç saati (ISO 8601) sağlanır. |
| **endTime** |IOT Hub, iş tamamlandığında (ISO 8601) tarihi sağlanır. Yalnızca iş 'Tamamlandı' durumuna ulaştıktan sonra geçerli. |
| **type** |İşleri türleri: |
| | **scheduledUpdateTwin**: İstenen özellikleri veya etiketleri güncelleştirmek için kullanılan bir proje. |
| | **scheduledDeviceMethod**: cihaz çiftleri kümesi üzerinde bir cihaz yöntemini çağırmak için kullanılan bir proje. |
| **Durumu** |İşin geçerli durumu. Durum için olası değerler: |
| | **Bekleyen**: Zamanlanmış ve iş hizmeti tarafından işlenmek üzere bekleniyor. |
| | **Zamanlanmış**: gelecekteki bir zamanı için zamanlandı. |
| | **çalışan**: şu anda etkin iş. |
| | **İptal**: işi iptal edildi. |
| | **başarısız**: işi başarısız oldu. |
| | **Tamamlanan**: İş tamamlandı. |
| **deviceJobStatistics** |İşin yürütme hakkındaki istatistiklerdir. |
| | **deviceJobStatistics** özellikleri: |
| | **deviceJobStatistics.deviceCount**: iş cihazların sayısı. |
| | **deviceJobStatistics.failedCount**: iş başarısız olduğu cihaz sayısı. |
| | **deviceJobStatistics.succeededCount**: Burada iş başarılı cihaz sayısı. |
| | **deviceJobStatistics.runningCount**: işi çalışmakta olan cihaz sayısı. |
| | **deviceJobStatistics.pendingCount**: işlemi çalıştırmak için bekleyen cihaz sayısı. |

### <a name="additional-reference-material"></a>Ek başvuru malzemesi
IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları] [ lnk-endpoints] her IOT hub'ı ortaya koyan çalışma zamanı ve yönetim işlemleri için çeşitli uç noktaları açıklar.
* [Azaltma ve kotalar] [ lnk-quotas] IOT Hub hizmeti ve hizmetin kullandığınızda beklenir azaltma davranışını uygulanan kotalar açıklar.
* [Azure IOT cihaz ve hizmet SDK'ları] [ lnk-sdks] çeşitli dil IOT hub'ı ile etkileşim kuran hem cihaz hem de hizmet uygulamaları geliştirirken kullanabileceğiniz SDK'ları listeler.
* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili] [ lnk-query] IOT Hub sorgu dili açıklar. IOT Hub'ından, cihaz ikizleri ve işler hakkında bilgi almak için bu sorgu dili kullanın.
* [IOT hub'ı MQTT desteği] [ lnk-devguide-mqtt] ve MQTT protokolünü için IOT hub'ı desteği hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede açıklanan kavramları bazıları denemek için aşağıdaki IOT hub'ı öğreticiye bakın:

* [İşleri zamanlama ve yayınlama][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: quickstart-control-device-node.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
