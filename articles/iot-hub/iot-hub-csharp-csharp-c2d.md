---
title: Bulut-cihaz iletilerini Azure IOT hub'ı (.NET) | Microsoft Docs
description: .NET için Azure IOT SDK'larını kullanarak bir Azure IOT hub'ından bir cihaza bulut-cihaz iletilerini göndermek nasıl. Bulut-cihaz iletilerini ve bulut-cihaz iletilerini göndermek için bir arka uç uygulaması değiştirmek için bir cihaz uygulaması değiştirin.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: e2090977813f5335f2170ff543bde19cd8bf123b
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185803"
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Cihazınızı IOT hub'ı (.NET) ile buluttan iletiler gönderme
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Giriş
Azure IOT hub'ı yardımcı olan tam olarak yönetilen bir hizmet, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimi etkinleştirmek ve bir çözüm arka ucu ' dir. [IOT Hub ile çalışmaya başlama] öğretici, IOT hub oluşturma, bir cihaz kimliği da sağlamak ve CİHAZDAN buluta iletiler gönderen bir cihaza uygulama kodu nasıl gösterir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Bu öğreticide yapılar [IOT Hub ile çalışmaya başlama]. Bunun nasıl yapılacağı anlatılmaktadır için:

* Çözüm arka ucunuz, tek bir cihaz IOT hub'ı aracılığıyla bulut-cihaz iletilerini gönderin.
* Bir cihazda bulut-cihaz iletilerini alır.
* Çözüm arka ucunuz, teslimat alındısı istek (*geri bildirim*) için bir cihaz IOT Hub'ından gönderilen iletileri.

Bulut-cihaz iletileri hakkında daha fazla bilgi bulabilirsiniz [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].

Bu öğreticinin sonunda iki .NET konsol uygulaması çalıştırın:

* **SimulatedDevice**, oluşturulan uygulamayı değiştirilmiş bir sürümünü [IOT Hub ile çalışmaya başlama], IOT hub'ınıza bağlanır ve bulut-cihaz iletilerini alır.
* **SendCloudToDevice**, IOT hub'ı aracılığıyla cihaz uygulamasına bulut-cihaz ileti gönderir ve ardından, teslimat alındısı.

> [!NOTE]
> IOT Hub aracılığıyla SDK desteği birçok cihaz platformlarını ve dilini (C, Java ve Javascript gibi) sahip [Azure IOT cihaz SDK'ları]. Bu öğreticinin koda ve genellikle Azure IOT hub'a Cihazınızı bağlamak hakkında adım adım yönergeler için bkz. [IOT Hub Geliştirici Kılavuzu].
> 
> 

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Visual Studio 2015 veya Visual Studio 2017
* Etkin bir Azure hesabı. (Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.)

## <a name="receive-messages-in-the-device-app"></a>Cihaz uygulamasında ileti alma
Bu bölümde oluşturduğunuz cihaz uygulamasını değiştireceksiniz [IOT Hub ile çalışmaya başlama] IOT hub'ından bulut-cihaz iletilerini almak için.

1. Visual Studio içinde **SimulatedDevice** projesinde, aşağıdaki yöntemi ekleyin **Program** sınıfı.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    `ReceiveAsync` Yöntemi zaman uyumsuz olarak döndürür alınan ileti, cihaz tarafından alınan zaman. Döndürür *null* specifiable zaman aşımı süresinden sonra (Bu durumda, varsayılan bir dakika kullanılır). Uygulama aldığında bir *null*, yeni iletileri için beklemeye devam etmelidir. Bu gereksinim sebebi `if (receivedMessage == null) continue` satır.
   
    Çağrı `CompleteAsync()` IOT Hub ileti başarıyla işlendi bildirir. İleti, cihaz kuyruktan güvenli bir şekilde kaldırılabilir. IOT Hub cihaz uygulaması iletisinin işlenmesi tamamlamasını engelleyen bir sorun oluştu, yeniden sağlar. Ardından işleme mantığı cihaz uygulamasında bu ileti önemli olduğu *ıdempotent*, böylece birden çok kez aynı iletiyi almak için aynı sonucu üretir. Bir uygulamanın da geçici olarak IOT hub'ı gelecekteki kullanım için bir Kuyruktaki iletinin koruma sonuçlanır bir ileti birleştirileceğine. Veya uygulamayı kalıcı olarak iletiyi kuyruktan kaldırır. bir ileti reddedebilirsiniz. Bulut buluttan cihaza iletinin yaşam döngüsü hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > Aktarım olarak, MQTT veya AMQP yerine HTTPS kullanarak `ReceiveAsync` yöntemi hemen döndürür. Bulut-cihaz iletilerini HTTPS ile desteklenen desenini denetleyen aralıklı olarak bağlanan seyrek iletileri (küçüktür 25 dakikada bir) cihazlar içindir. Daha fazla HTTPS verme istekleri azaltma IOT hub'da sonuçlarını alır. MQTT, AMQP ve HTTPS desteği ve IOT hub'ı azaltma arasındaki farklar hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].
   > 
   > 
2. Aşağıdaki yöntemi ekleyin **ana** yöntemi, hemen önce `Console.ReadLine()` satırı:
   
        ReceiveC2dAsync();

> [!NOTE]
> Basitlik'ın çok için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), örneğin MSDN makalesinde önerildiği uygulamalıdır [geçici hata işleme].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Bulut buluttan cihaza ileti gönderme
Bu bölümde, cihaz uygulaması için bulut-cihaz iletilerini gönderen bir .NET konsol uygulaması yazma.

1. Geçerli Visual Studio çözümünde kullanarak bir Visual C# masaüstü uygulaması projesi oluşturma **konsol uygulaması** proje şablonu. Projeyi adlandırın **SendCloudToDevice**.
   
    ![Visual Studio'da yeni proje][20]
2. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **çözüm için NuGet paketlerini Yönet...** . 
   
    Bu eylem açar **NuGet paketlerini Yönet** penceresi.
3. Arama **Microsoft.Azure.Devices**, tıklayın **yükleme**ve kullanım koşullarını kabul edin. 
   
    Bu indirir, yükler ve bir başvuru ekler [Azure IOT hizmeti SDK'sı NuGet paketi].

4. Aşağıdaki `using` deyimini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini IOT hub'ı bağlantı dizesi ile değiştirin [IOT Hub ile çalışmaya başlama]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Bu yöntem cihazı kimliği ile yeni bir bulut-cihaz ileti gönderir `myFirstDevice`. Yalnızca kullanılan bir değişiklik varsa, bu parametreyi değiştirmek [IOT Hub ile çalışmaya başlama].
7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. Visual Studio içinde çözümü sağ tıklatın ve seçin **başlangıç projelerini Ayarla...** . Seçin **birden fazla başlangıç projesi**, ardından **Başlat** için eylem **ReadDeviceToCloudMessages**, **SimulatedDevice**, ve **SendCloudToDevice**.
9. Tuşuna **F5**. Üç uygulama başlamanız gerekir. Seçin **SendCloudToDevice** windows ve ENTER tuşuna **Enter**. Cihaza uygulama tarafından alınan iletiyi görmeniz gerekir.
   
   ![Uygulama alma iletisi][21]

## <a name="receive-delivery-feedback"></a>Teslim geri bildirim alın
İstek teslim (veya zaman aşımı) bildirimleri için IOT Hub'ından her bulut-cihaz ileti için mümkündür. Bu seçenek, kolayca yeniden deneyin ya da tazminat ödemeden mantıksal bildirmek çözüm arka ucu sağlar. Bulut-cihaz geri bildirim hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici kılavuzunun][IoT Hub developer guide - C2D].

Bu bölümde, değişiklik **SendCloudToDevice** geri bildirim isteme ve IOT Hub'ından almak için uygulama.

1. Visual Studio içinde **SendCloudToDevice** projesinde, aşağıdaki yöntemi ekleyin **Program** sınıfı.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Bu alma Düzen cihaz uygulamasından bulut-cihaz iletilerini almak için kullanılan aynı olduğuna dikkat edin.
2. Aşağıdaki yöntemi ekleyin **ana** yöntemi, sonra sağ `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` satırı:
   
        ReceiveFeedbackAsync();
3. Bir özelliğin belirtmek zorunda bulut buluttan cihaza iletinin teslim için geri bildirim istemek için **SendCloudToDeviceMessageAsync** yöntemi. Hemen sonra aşağıdaki satırı ekleyin `var commandMessage = new Message(...);` satırı:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Tuşlarına basarak uygulamaları çalıştırma **F5**. Başlangıç üç uygulama görmeniz gerekir. Seçin **SendCloudToDevice** windows ve ENTER tuşuna **Enter**. Birkaç saniye sonra ve cihaz uygulaması tarafından alınan geri bildirim iletisi alınan iletiyi görmeniz gerekir, **SendCloudToDevice** uygulama.
   
   ![Uygulama alma iletisi][22]

> [!NOTE]
> Basitlik'ın çok için bu öğreticiyi herhangi bir yeniden deneme ilkesi uygulamaz. Üretim kodunda yeniden deneme ilkelerini (üstel geri alma), örneğin MSDN makalesinde önerildiği uygulamalıdır [geçici hata işleme].
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bulut-cihaz iletilerini gönderip öğrendiniz. 

IOT hub'ı kullanan tam uçtan uca çözümler örneklerini görmek için bkz: [Azure IOT Uzaktan izleme çözüm Hızlandırıcısını].

IOT Hub ile çözümleri geliştirme hakkında daha fazla bilgi için bkz. [IOT Hub Geliştirici Kılavuzu].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IOT hizmeti SDK'sı NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Geçici hata işleme]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[IOT Hub Geliştirici Kılavuzu]: iot-hub-devguide.md
[IOT Hub ile çalışmaya başlama]: quickstart-send-telemetry-dotnet.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IOT Uzaktan izleme çözüm Hızlandırıcısını]: https://docs.microsoft.com/azure/iot-suite/
[Azure IOT cihaz SDK'ları]: iot-hub-devguide-sdks.md