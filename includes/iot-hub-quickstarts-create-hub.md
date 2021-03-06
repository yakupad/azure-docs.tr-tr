---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 63acf0297a694ff442d56e67d52fd9b4e49f812d
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008595"
---
Birinci adım, Azure portalını kullanarak aboneliğinizde bir IoT hub’ı oluşturmaktır. IoT hub’ı, çok sayıda cihazdan buluta yüksek hacimlerde telemetri almanızı sağlar. Hub daha sonra o telemetriyi okuyup işlemek üzere bulutta çalışan bir veya daha fazla arka uç hizmetini etkinleştirir.

1. [Azure Portal](http://portal.azure.com)’da oturum açın.

1. **Kaynak oluştur** > **Nesnelerin İnterneti** > **Iot Hub** seçeneğini belirleyin.

    ![IoT Hub’ı yüklemek için seçin][1]

1. **IoT hub** bölmesine IoT hub’ınızla ilgili aşağıdaki bilgileri girin:

   * **Abonelik**: Bu IoT hub'ını oluşturmak için kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu**: IoT hub’ını içerecek bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. **TestResources** gibi ilgili tüm kaynakları aynı gruba birlikte koyarak, bunların tümünü birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde o grupta bulunan tüm kaynaklar da silinir. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma][lnk-resource-groups] konusunu inceleyin.
   * **Bölge**: Cihazlarınıza en yakın konumu seçin.
   * **Ad**: IoT hub'ınız için benzersiz bir ad oluşturun. Girdiğiniz ad kullanılabilir durumdaysa yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![IoT Hub temel bilgileri penceresi][2]

2. IoT hub’ınızı oluşturmaya devam etmek için **Sıradaki: Boyut ve ölçek** öğesini seçin. 

3. **Fiyatlandırma ve ölçek katmanınızı** seçin. Bu makale için, aboneliğinizde hala mevcutsa **F1 - Ücretsiz** katmanını seçin. Daha fazla bilgi için [Fiyatlandırma ve ölçek katmanı][lnk-pricing] konusunu inceleyin.

   ![IoT Hub boyut ve ölçek penceresi][3]

4. **İncele ve oluştur**’u seçin.

1. IoT hub bilgilerinizi gözden geçirin, ardından **Oluştur**’a tıklayın. IoT hub’ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.


<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md