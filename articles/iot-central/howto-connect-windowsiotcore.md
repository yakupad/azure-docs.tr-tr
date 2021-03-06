---
title: Azure IOT Central uygulamanızı Windows IOT Core cihazı bağlayın | Microsoft Docs
description: Bir cihaz geliştirici olarak, Azure IOT Central uygulamanıza MXChip IOT DevKit cihaz bağlanmayı öğreneceksiniz.
author: miriambrus
ms.author: mriamb
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 39eb6f137750f7f741c88dcdf9a55f34d24eaa59
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205757"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Azure IOT Central uygulamanızı Windows IOT Core cihazı bağlayın

Bu makalede, Microsoft Azure IOT Central uygulamanızı Windows IOT Core cihazı bağlamak için bir cihaz geliştirici olarak nasıl.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için şunlar gereklidir:

1. Oluşturulan bir Azure IOT Central uygulamasına **örnek Devkits** uygulama şablonu. Daha fazla bilgi için [Azure IOT Central uygulaması oluşturmayı](howto-create-application.md).
2. Windows 10 IoT Core işletim sistemi çalıştıran bir cihaz. Bu kılavuz için Raspberry Pi'yi kullanacağız.


## <a name="sample-devkits-application"></a>**Örnek Devkits** uygulama

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Windows IOT Core** cihaz şablonu aşağıdaki özelliklere sahip: 

- Cihaz için ölçüler içeren telemetri **nem**, **sıcaklık** ve **baskısı**. 
- Ayarları gösteren **fanı hızı**.
- Cihaz özelliği içeren özellik **sayı öldürmüş** ve **konumu** bulut özelliği.


Cihaz şablon yapılandırması hakkında tam Ayrıntılar için bkz [Windows IOT Core cihazı şablon ayrıntıları](howto-connect-windowsiotcore.md#windows-iot-core-device-template-details)

## <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **Windows IOT Core** cihaz şablonu ve cihaz bağlantı dizesini Not olun. Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).

### <a name="prepare-the-windows-iot-core-device"></a>Windows IOT Core cihazı hazırlama

Ayarlamak için bir Windows IOT Core cihazı Lütfen izleyin [Windows IOT Core cihazı ayarlama] öğesinde adım adım kılavuz (https://github.com/Microsoft/microsoft-iot-central-firmware/tree/master/WindowsIoT#setup-a-physical-device).

### <a name="add-a-real-device"></a>Gerçek cihaz ekleme

Azure IOT Central uygulamanızda gerçek bir CİHAZDAN ekleme **Windows IOT Core** cihaz şablonu ve cihaz bağlantı dizesini Not olun. Daha fazla bilgi için [Azure IOT Central uygulamanıza gerçek bir cihaz eklemek](tutorial-add-device.md).

## <a name="prepare-the-windows-10-iot-core-device"></a>Windows 10 IoT Core cihazı hazırlama

### <a name="what-youll-need"></a>İhtiyaç duyacaklarınız:

Fiziksel bir Windows 10 IoT Core cihazı ayarlama için öncelikle Windows 10 IoT Core çalıştıran bir cihaza olması gerekir. Bir Windows 10 IoT Core cihazı ayarlama konusunda bilgi [burada](https://developer.microsoft.com/en-us/windows/iot/getstarted/prototype/setupdevice).

Ayrıca, Azure IOT Central ile iletişim kurabilen bir istemci uygulaması gerekir. Azure SDK'sını kullanarak kendi özel uygulamanızı oluşturun ve Visual Studio kullanarak Cihazınızı dağıtın veya karşıdan yükleyebileceğiniz bir [önceden oluşturulmuş bir örnek](https://developer.microsoft.com/en-us/windows/iot/samples) yalnızca dağıtma ve cihazda çalıştırın. 

### <a name="deploying-the-sample-client-application"></a>Örnek istemci uygulaması dağıtma

Bunu hazırlamak için istemci uygulamasına önceki adımdan gelen Windows 10 IOT Cihazınızı dağıtmak için:

**Bağlantı dizesini kullanmak istemci uygulaması için cihazda depolanan emin olun.**
* Masaüstünde, bağlantı dizesini connection.string.iothub adlı bir metin dosyasına kaydedin.
* Metin dosyası, cihazın belge klasörüne kopyalayın: `[device-IP-address]\C$\Data\Users\DefaultAccount\Documents\connection.string.iothub`

Bunu yaptıktan sonra açmanız gerekir [Windows Device Portal](https://docs.microsoft.com/en-us/windows/iot-core/manage-your-device/deviceportal) http://[device-IP-address]:8080 içinde herhangi bir tarayıcıda yazarak.

Orada ve gerekirse gösterildiği gibi, şunları yapmanız gerekir:
1. Sol taraftaki "Uygulamalar" düğümünü genişletin.
2. "Hızlı çalıştırma örnekler"'i tıklatın.
3. "Azure IOT Hub istemci"'yi tıklatın.
4. "Dağıtma ve çalıştırma" seçeneğine tıklayın.

![Azure IOT Hub istemci Windows Device Portal üzerinde GIF'i](./media/howto-connect-windowsiotcore/iothubapp.gif)

Başarılı olduğunda, uygulama bir cihazda başlatın ve şöyle görünür:

![Azure IOT Hub'ın ekran görüntüsü istemci uygulaması](./media/howto-connect-windowsiotcore/IoTHubForegroundClientScreenshot.png)

Azure IOT Central Raspberry Pi üzerinde çalışan kodu uygulaması ile nasıl etkileşim kurduğunu görebilirsiniz:

* Üzerinde **ölçümleri** sayfa gerçek cihazınız için telemetriyi görebilirsiniz.
* Üzerinde **özellikleri** sayfasında, bildirilen öldürmüş numarası özelliğinin değeri görebilirsiniz.
* Üzerinde **ayarları** sayfasında, Raspberry Pi voltaj ve giriş hızı gibi çeşitli ayarları değiştirebilirsiniz.

## <a name="download-the-source-code"></a>Kaynak kodunu indirebilir

Keşfedin ve istemci uygulaması için kaynak kodunu değiştirmek istiyorsanız, Github'dan indirebileceğiniz [burada](https://github.com/Microsoft/Windows-iotcore-samples/tree/develop/Samples/Azure/IoTHubClients). Kodu değiştirin planlıyorsanız, bu ve benioku dosyasındaki yönergeleri izlemelidir [burada](https://github.com/Microsoft/Windows-iotcore-samples) masaüstü işletim sisteminiz için.

> [!NOTE]
> Varsa **git** yüklü geliştirme ortamınızda buradan indirebilirsiniz [ https://git-scm.com/download ](https://git-scm.com/download).

## <a name="windows-iot-core-device-template-details"></a>Windows IOT Core cihazı şablon ayrıntıları

Oluşturulan uygulama **örnek Devkits** uygulama şablonu içeren bir **Windows IOT Core** cihaz şablonu aşağıdaki özelliklere sahip:

### <a name="telemetry-measurements"></a>Telemetri ölçümleri

| Alan adı     | Birimler  | Minimum | Maksimum | Ondalık basamak sayısı |
| -------------- | ------ | ------- | ------- | -------------- |
| Nem oranı       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| basınç       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Ayarlar

Sayısal ayarları

| Görünen ad | Alan adı | Birimler | Ondalık basamak sayısı | Minimum | Maksimum | İlk |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Fan hızı    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |


### <a name="properties"></a>Özellikler

| Tür            | Görünen ad | Alan adı | Veri türü |
| --------------- | ------------ | ---------- | --------- |
| Cihaz özelliği | Sayı öldürmüş   | dieNumber  | number    |
| Metin            | Konum     | location   | Yok       |
