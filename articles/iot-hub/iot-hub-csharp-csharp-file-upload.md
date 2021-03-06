---
title: .NET ile Azure IOT Hub'ına cihazlardan dosya yükleme | Microsoft Docs
description: .NET için Azure IOT cihaz SDK'sını kullanarak bulutta bir CİHAZDAN dosyaları karşıya yükleme. Karşıya yüklenen dosyaları bir Azure depolama blob kapsayıcısında depolanır.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 677f0e0f17191feb560ac5e9bb72a058e385084d
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185839"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a>.NET kullanarak IOT Hub'la cihazınızdan dosyaları buluta yükleyin

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Bu öğreticide kodda geliştirir [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) dosya karşıya yükleme özellikleri IOT hub'ı kullanmak nasıl gösteren öğretici. Bunun nasıl yapılacağı anlatılmaktadır için:

- Bir cihaz Azure ile güvenli bir şekilde sağlayan bir dosya karşıya yükleme için URI blob.
- IOT hub'ı dosya karşıya yükleme bildirimlerini, uygulama arka ucu dosyasında bir işlem tetiklemek için kullanın.

[IOT Hub ile çalışmaya başlama](quickstart-send-telemetry-dotnet.md) ve [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) öğreticiler, IOT Hub'ın temel CİHAZDAN buluta ve bulut-cihaz Mesajlaşma işlevlerini gösterir. [İşlem CİHAZDAN buluta iletileri](tutorial-routing.md) Öğreticisi, CİHAZDAN buluta iletileri Azure blob depolama alanında güvenilir bir şekilde depolamak için bir yol açıklar. Ancak, bazı senaryolarda cihazlarınızı IOT hub'ı kabul görece küçük bir CİHAZDAN buluta ileti gönderme verileri kolayca eşlenemiyor. Örneğin:

* Görüntüleri içeren büyük dosyaları
* Videolar
* Titreşim veri yüksek sıklıkta örneklenir
* Önceden işlenmiş verilerin bazı formlarıyla

Bu dosyalar genellikle toplu işleme gibi araçları kullanarak bulutta olduğu [Azure Data Factory](../data-factory/introduction.md) veya [Hadoop](../hdinsight/index.yml) yığını. Bir CİHAZDAN karşıya dosya yükleme gerektiğinde, güvenlik ve güvenilirlik IOT hub'ı kullanmaya devam edebilirsiniz.

Bu öğreticinin sonunda iki .NET konsol uygulaması çalıştırın:

* **SimulatedDevice**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) öğretici. Bu uygulama bir dosya depolama, IOT hub tarafından sağlanan bir SAS URI'sini kullanarak yükler.
* **ReadFileUploadNotification**, IOT hub'ından dosya karşıya yükleme bildirimleri alır.

> [!NOTE]
> IOT Hub, Azure IOT cihaz SDK'ları birçok cihaz platformlarını ve dilini (C, Java ve Javascript gibi) destekler. Başvurmak [Azure IOT Geliştirici Merkezi] Cihazınızı Azure IOT Hub'ına bağlanmak adım adım yönergeler için.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Bir cihaz uygulamasından bir dosyayı karşıya yükleyin

Bu bölümde oluşturduğunuz cihaz uygulamasını değiştirmek [IOT Hub ile bulut buluttan cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md) IOT hub'ından bulut-cihaz iletilerini almak için.

1. Visual Studio'da sağ tıklayıp **SimulatedDevice** proje, tıklayın **Ekle**ve ardından **varolan öğe**. Bir görüntü dosyasına gidin ve projenize ekleyin. Bu öğreticide, görüntüyü adlı varsayılır `image.jpg`.

1. Görüntü üzerinde sağ tıklayın ve ardından **özellikleri**. Emin olun **çıkış dizinine Kopyala** ayarlanır **her zaman Kopyala**.

    ![][1]

1. İçinde **Program.cs** dosyasının en üstüne aşağıdaki deyimleri ekleyin:

    ```csharp
    using System.IO;
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    `UploadToBlobAsync` Yöntemi karşıya yüklenecek dosyanın adı ve akış kaynağında dosyanın alır ve depolama için karşıya yükleme işleme. Konsol uygulaması, dosyayı karşıya yüklemek için gereken süreyi görüntüler.

1. Aşağıdaki yöntemi ekleyin **ana** yöntemi, hemen önce `Console.ReadLine()` satırı:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Basitlik'ın çok için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), örneğin MSDN makalesinde önerildiği uygulamalıdır [geçici hata işleme].

## <a name="receive-a-file-upload-notification"></a>Dosya karşıya yükleme bildirim alma

Bu bölümde, IOT Hub'ından dosya karşıya yükleme bildirim iletileri alan bir .NET konsol uygulaması yazma.

1. Geçerli Visual Studio çözümünde kullanarak bir Visual C# Windows projesi oluşturma **konsol uygulaması** proje şablonu. Projeyi adlandırın **ReadFileUploadNotification**.

    ![Visual Studio'da yeni proje][2]

1. Çözüm Gezgini'nde sağ **ReadFileUploadNotification** proje ve ardından **NuGet paketlerini Yönet...** .

1. İçinde **NuGet Paket Yöneticisi** penceresinde, arama **Microsoft.Azure.Devices**, tıklayın **yükleme**ve kullanım koşullarını kabul edin.

    Bu eylem indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sı NuGet paketi] içinde **ReadFileUploadNotification** proje.

1. İçinde **Program.cs** dosyasının en üstüne aşağıdaki deyimleri ekleyin:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini IOT hub'ı bağlantı dizesi ile değiştirin [IOT Hub ile çalışmaya başlama]:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. **Program** sınıfına aşağıdaki yöntemi ekleyin:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Bu alma Düzen cihaz uygulamasından bulut-cihaz iletilerini almak için kullanılan aynı olduğuna dikkat edin.

1. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Uygulamaları çalıştırma

Şimdi uygulamaları çalıştırmaya hazırsınız.

1. Visual Studio Çözümünüze sağ tıklayın ve seçin **başlangıç projelerini Ayarla**. Seçin **birden fazla başlangıç projesi**, ardından **Başlat** için eylem **ReadFileUploadNotification** ve **SimulatedDevice**.

1. Tuşuna **F5**. Her iki uygulamayı başlamanız gerekir. Bir konsol uygulamasında tamamlandı karşıya yükleme ve bir konsol uygulaması tarafından alınan karşıya yükleme bildirim iletisini görmeniz gerekir. Kullanabileceğiniz [Azure portal] veya Visual Studio sunucu Gezgini'nde aygıtını karşıya yüklenen dosya, Azure depolama hesabınızdaki varlığını denetleyin.

    ![][50]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazlardan karşıya dosya yükleme işlemleri basitleştirmek için dosya karşıya yükleme özellikleri IOT hub'ı kullanmayı öğrendiniz. IOT hub özelliklerini ve aşağıdaki makalelerde senaryolarını keşfetmeye devam edebilirsiniz:

* [Programlamalı IOT hub oluşturma][lnk-create-hub]
* [C SDK'ya giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

[Azure portal]: https://portal.azure.com/

[Azure IoT Geliştirici Merkezi]: http://azure.microsoft.com/develop/iot

[Geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure IOT hizmeti SDK'sı NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
