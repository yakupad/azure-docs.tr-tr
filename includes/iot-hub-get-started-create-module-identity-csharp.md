---
title: include dosyası
description: include dosyası
services: iot-hub
author: chrissie926
manager: timlt
ms.service: iot-hub
ms.topic: include
ms.date: 04/26/2018
ms.author: menchi
ms.custom: include file
ms.openlocfilehash: d2b409c7454645893665b080b927998402056cdd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34666917"
---
## <a name="create-a-module-identity"></a>Modül kimliği oluşturma

Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde cihaz kimliği ve modül kimliği oluşturan bir .NET konsol uygulaması oluşturursunuz. Kimlik kayıt defterinde girişi olmayan bir cihaz veya modül, IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub geliştirici kılavuzunun][lnk-devguide-identity] "Kimlik kayıt defteri" bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, hem cihaz hem de modül için benzersiz bir kimlik ve anahtar oluşturur. Cihazınız ve modülünüz, IoT Hub’ına cihazdan buluta iletileri gönderdiğinde kendisini tanımlamak için bu değerleri kullanır. Kimlikler büyük/küçük harfe duyarlıdır.


1. **Visual Studio projesi oluşturma** - Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak yeni bir çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.6.1 veya sonraki bir sürüm olduğundan emin olun. Projeye **CreateIdentities** ve çözüme **IoTHubGetStarted** adını verin.

    ![Visual Studio çözümü oluşturma][11]

2. **Azure IoT Hub .NET hizmet SDK’sı V1.16.0-preview-001’i yükleme** - Modül kimliği ve modül ikizi, genel önizleme aşamasındadır. Yalnızca IoT Hub ön sürüm hizmet SDK’larında kullanılabilir. Visual Studio’da araçlar > Nuget paket yöneticisi > çözüm için Nuget paketlerini yönet seçeneğini açın. Microsoft.Azure.Devices öğesini arayın. Ön sürümü dahil et onay kutusunu işaretlediğinizden emin olun. 1.16.0-preview-001 sürümünü seçip yükleyin. Şimdi tüm modül özelliklerine erişiminiz vardır. 

    ![Azure IoT Hub .NET hizmet SDK’sı V1.16.0-preview-001’i yükleme][10]

3. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:

    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Common.Exceptions;
    ```

4. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.

    ```csharp
    const string connectionString = "<replace_with_iothub_connection_string>";
    const string deviceID = "myFirstDevice";
    const string moduleID = "myFirstModule";
    ```

5. Aşağıdaki kodu ekleyin **ana** sınıfı.
    ```csharp
    static void Main(string[] args)
    {
        AddDeviceAsync().Wait();
        AddModuleAsync().Wait();
    }
    ```

6. **Program** sınıfına aşağıdaki yöntemleri ekleyin:

    ```csharp
    private static async Task AddDeviceAsync()
    {
        RegistryManager registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        Device device;

        try
        {
            device = await registryManager.AddDeviceAsync(new Device(deviceID));
        }
        catch (DeviceAlreadyExistsException)
        {
            device = await registryManager.GetDeviceAsync(deviceID);
            }

            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
    }

    private static async Task AddModuleAsync()
    {
        RegistryManager registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        Module module;

        try
        {
            module = await registryManager.AddModuleAsync(new Module(deviceID, moduleID));
        }
        catch (ModuleAlreadyExistsException)
        {
            module = await registryManager.GetModuleAsync(deviceID, moduleID);
        }

        Console.WriteLine("Generated module key: {0}", module.Authentication.SymmetricKey.PrimaryKey);
    }
    ```

    AddDeviceAsync() yöntemi, **myFirstDevice** kimliği ile bir cihaz kimliği oluşturur. (Bu cihaz kimliği, kimlik kayıt defterinde zaten varsa, kod yalnızca mevcut cihaz bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal cihaz uygulamasında bu anahtarı kullanırsınız.

    AddModuleAsync() yöntemi, **myFirstDevice** cihazının altında **myFirstModule** kimliği ile bir modül kimliği oluşturur. (Bu modül kimliği, kimlik kayıt defterinde zaten varsa, kod yalnızca mevcut modül bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal modül uygulamasında bu anahtarı kullanırsınız.

[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. Bu uygulamayı çalıştırın ve cihaz anahtarını ve modül anahtarını not edin.

> [!NOTE]
> IoT Hub kimlik kayıt defteri yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz ve modül kimliklerini depolar. Kimlik kayıt defteri, cihaz kimliklerini ve anahtarlarını güvenlik kimlik bilgileri olarak kullanmak için depolar. Kimlik kayıt defterinin her cihaz için depoladığı etkin/devre dışı bayrağını kullanarak, ilgili cihaza erişimi devre dışı bırakabilirsiniz. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Modül kimlikleri için etkin/devre dışı bayrağı yoktur. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-module-identity-csharp/install-sdk.png
[11]: ./media/iot-hub-get-started-create-module-identity-csharp/create-identities-csharp1.JPG

<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
