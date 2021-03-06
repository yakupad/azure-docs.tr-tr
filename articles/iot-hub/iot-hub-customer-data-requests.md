---
title: Müşteri verilerini talep özellikleri
description: Müşteri verilerini talep özelliklerin özeti
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: c0197b8f0cceab575aa5e830d094ddd2fb6b6b01
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436304"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri verilerini talep özelliklerin özeti

Azure IOT Hub, milyonlarca cihaz ve bölümlenmiş bir Azure hizmeti arasında güvenli, çift yönlü iletişim sağlayan kurumsal müşteriler hedeflenen bir REST API tabanlı bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Cihaz tanımlayıcısı (cihaz kimliği), cihazları tek tek bir kiracı Yöneticisi tarafından atanır. Cihaz verilerini atanan cihaz kimliğini temel alır. Microsoft hiçbir bilgi tutar ve kullanıcı bağıntı cihaz Kimliğine izin verilerine erişim yok.

Azure IOT Hub'ında yönetilen cihazların birçoğu, örneğin bir office thermostat veya Fabrika robot kişisel cihazlarını değildir. Müşteriler, ancak bazı cihazların kişisel olmasını dikkate almanız ve kendi kararımıza kendi varlık veya envanter izleme cihazlar kişilere tie yöntemi olması. Azure IOT hub'ı yönetir ve kişisel veriler gibi cihazlar ile ilişkili tüm verileri depolar.

Kiracı yöneticileri dışarı aktarma veya bir cihaz kimliği ile ilişkili verileri silerek bilgi istekleri için kullanabilirsiniz Azure portal'ı veya hizmetin REST API'leri

Diğer hizmetler için cihaz iletilerini iletecek şekilde Azure IOT Hub hizmetinin yönlendirme özelliğini kullanırsanız, ardından veri isteklerini yönlendirme her uç nokta için Kiracı Yöneticisi tarafından belirli bir cihaz için tam bir istek tamamlamak için gerçekleştirilmelidir. Daha fazla ayrıntı için her uç noktanın başvuru belgelerine bakın. Desteklenen uç noktalar hakkında daha fazla bilgi için bkz: [başvuru - IOT Hub uç noktaları](iot-hub-devguide-endpoints.md).

Azure IOT Hub hizmeti, Azure Event Grid Tümleştirmesi özelliğini kullanırsanız, ardından veri istekleri bu olayların her abone için Kiracı Yöneticisi tarafından gerçekleştirilmelidir. Daha fazla bilgi için [Event Grid kullanarak IOT Hub olaylarına tepki](iot-hub-event-grid.md).

Tanılama günlükleri oluşturmak için Azure IOT Hub hizmetinin Azure İzleyici tümleştirme özelliği kullanırsanız, veri isteklerini depolanan günlükler karşı Kiracı Yöneticisi tarafından gerçekleştirilmelidir. Daha fazla bilgi için [Azure IOT Hub durumunu izleyin](iot-hub-monitor-resource-health.md).

## <a name="deleting-customer-data"></a>Müşteri verileri silme

Kiracı yöneticileri, cihazla ilişkili verileri silen bir cihazı silmek için Azure portalında IOT cihazlar dikey penceresine Azure IOT hub'ı uzantının kullanabilirsiniz.

REST API'lerini kullanarak cihazları silme işlemleri gerçekleştirmek mümkündür. Daha fazla bilgi için [hizmet - cihaz silme](https://docs.microsoft.com/rest/api/iothub/service/deletedevice).

## <a name="exporting-customer-data"></a>Müşteri verilerini dışarı aktarma

Kiracı yöneticileri, kopya yazılımınız ve IOT cihazlar dikey penceresine bir cihazla ilişkili verileri dışarı aktarmak için Azure portalında Azure IOT hub'ı uzantısı'nın içinde yapıştırın.

Dışarı aktarma işlemleri için REST API'lerini kullanarak cihazları da mümkündür. Daha fazla bilgi için [Hizmeti - aygıt alma](https://docs.microsoft.com/rest/api/iothub/service/getdevice).

> [!NOTE]
> Microsoft'un Kurumsal hizmet kullandığınızda, sistem tarafından oluşturulan günlükleri olarak bilinen bazı bilgiler, Microsoft oluşturur. Bazı Azure IOT Hub sistem tarafından oluşturulan günlükler, erişilebilir veya Kiracı yöneticileri tarafından verilebilir değildir. Bu günlükler, cihazlara ayrı ayrı ilgili tanılama veri ve hizmet içinde gerçekleştirilen yansıttığından eylemleri oluşturur.

## <a name="links-to-additional-documentation"></a>Ek belgelere bağlantılar

Azure IOT Hub hizmet API'leri için tüm belgeler adresindedir [ https://docs.microsoft.com/rest/api/iothub/service ](https://docs.microsoft.com/rest/api/iothub/service).