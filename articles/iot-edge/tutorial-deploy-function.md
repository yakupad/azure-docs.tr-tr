---
title: Azure IoT Edge ile Azure işlevlerini dağıtma | Microsoft Docs
description: Azure işlevini bir modül olarak Edge cihazına dağıtın.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f3ba0ccb1cb8961344b605e7ec386b6d6692262
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006886"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules-preview"></a>Öğretici: Azure işlevlerini IoT Edge modülleri olarak dağıtma (önizleme)

İş mantığınızı doğrudan Azure IoT Edge cihazlarınıza uygulayan kodu dağıtmak için Azure İşlevleri'ni kullanabilirsiniz. Bu öğreticide, benzetimi yapılan IoT Edge cihazındaki algılayıcı verilerini filtreleyen bir Azure işlevi oluşturma ve dağıtma işlemlerinde size yol gösterilir. [Windows][lnk-tutorial1-win]'ta veya [Linux][lnk-tutorial1-lin]'ta bir simülasyon cihazına Azure IoT Edge dağıtma hızlı başlangıçlarında oluşturduğunuz simülasyon IoT Edge cihazınızı kullanacaksınız. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:     

> [!div class="checklist"]
> * Visual Studio Code kullanarak Azure işlevi oluşturma.
> * VS Code ve Docker kullanarak Docker görüntüsü oluşturma ve bunu kapsayıcı kayıt defterinde yayımlama.
> * Kapsayıcı kayıt defterindeki modülü IoT Edge cihazınıza dağıtma.
> * Filtrelenmiş verileri görüntüleme.

>[!NOTE]
>Azure IoT Edge üzerindeki Azure işlevi modülleri genel [önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) aşamasındadır. 

Bu öğreticide oluşturacağınız Azure işlevi, cihazınız tarafından oluşturulan sıcaklık verilerini filtreler. İşlev, yalnızca sıcaklık belirtilen bir eşiğin üzerindeyse yukarı yönde Azure IoT Hub'a iletileri gönderir. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide derleyeceğiniz işlev modülünü test etmek için bir IoT Edge cihazına sahip olmanız gerekir. [Linux](quickstart-linux.md) veya [Windows](quickstart.md) hızlı başlangıcında yapılandırdığınız cihazı kullanabilirsiniz.

Geliştirme makinenizde aşağıdaki önkoşulların karşılandığından emin olmalısınız: 
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Visual Studio Code için C# (OmniSharp tarafından desteklenen) uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
* Visual Studio Code için [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download).
* Geliştirme makinenizde [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
Bu öğreticide modül hazırlamak ve dosyalardan bir **kapsayıcı görüntüsü** oluşturmak için VS Code için Azure IoT Edge uzantısını kullanırsınız. Ardından bu görüntüyü, görüntülerinizin depolandığı ve yönetildiği **kayıt defterine** gönderirsiniz. Son olarak, görüntünüzü IoT Edge cihazınızda çalıştırmak üzere kayıt defterinizden dağıtırsınız.  

Bu öğretici için Docker ile uyumlu herhangi bir kayıt defteri kullanabilirsiniz. Bulutta sağlanan iki popüler Docker kayıt defteri hizmeti [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ve [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)'dır. Bu öğreticide Azure Container Registry kullanılır. 

1. [Azure portalında](https://portal.azure.com), **Kaynak oluştur** > **Kapsayıcılar** > **Container Registry**'yi seçin.

    ![Kapsayıcı kayıt defteri oluşturma](./media/tutorial-deploy-function/create-container-registry.png)

2. Kayıt defteriniz için bir ad girin ve bir abonelik seçin.
3. Kaynak grubu için, IoT hub'ınızın bulunduğu kaynak grubu adıyla aynı adı kullanmanız önerilir. Tüm kaynakları aynı grupta tutarak birlikte yönetebilirsiniz. Örneğin, test için kullanılan kaynak grubu silindiğinde, gruptaki tüm test kaynakları silinir. 
4. SKU'yu **Temel** olarak ayarlayın ve **Yönetici kullanıcı** ayarını **Etkinleştir** olarak değiştirin. 
5. **Oluştur**’u seçin.
6. Kapsayıcı kayıt defteriniz oluşturulduktan sonra, bu kayıt defterine gidin ve **Erişim anahtarları**'nı seçin. 
7. **Oturum açma sunucusu**, **Kullanıcı adı** ve **Parola** değerlerini kopyalayın. Öğreticinin sonraki bölümlerinde bu değerleri kullanırsınız. 

## <a name="create-a-function-project"></a>İşlev projesi oluşturma
Aşağıdaki adımlarda, Visual Studio Code'u ve Azure IoT Edge uzantısını kullanarak IoT Edge işlevi oluşturulur.

1. Visual Studio Code'u açın.
2. **View (Görünüm)** > **Integrated Terminal (Tümleşik Terminal)** seçimini yaparak VS Code tümleşik terminalini açın. 
2. **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçerek VS Code komut paletini açın.
3. Komut paletinde **Azure: Sign in** komutunu girin ve çalıştırın. Azure hesabınızda oturum açmak için yönergeleri izleyin. Oturumu önceden açtıysanız bu adımı atlayabilirsiniz.
3. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın. Komut paletinde çözümünüzü oluşturmak için aşağıdaki bilgileri girin: 

   1. Çözümü oluşturmak istediğiniz klasörü seçin. 
   2. Çözümünüz için bir ad girin veya varsayılan **EdgeSolution** adını kabul edin.
   3. Modül şablonu olarak **Azure İşlevleri - C#** seçin. 
   4. Modülünüze **CSharpFunction** adını verin. 
   5. İlk modülünüz için görüntü deposu olarak önceki bölümde oluşturduğunuz Azure Container Registry bileşenini belirtin. **localhost:5000** yerine kopyaladığınız oturum açma sunucusu değerini yazın. Dizenin son hali \<kayıt defteri adı\>.azurecr.io/csharpfunction ifadesine benzer olmalıdır.

4. VS Code penceresi IoT Edge çözümü çalışma alanınızı yükler: \.vscode klasörü, modül klasörü, dağıtım bildirimi şablon dosyası. ve \.env dosyası. VS Code gezgininde, **modules** > **CSharpFunction** > **EdgeHubTrigger-Csharp** > **run.csx** dosyasını açın.

5. Dosyanın içeriğini aşağıdaki kodla değiştirin:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it.
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold.
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object.
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);                }
                // Add a new property to the message to indicate it is an alert.
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message.       
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages.
    class MessageBody
    {
        public Machine machine {get; set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
   ```

6. Dosyayı kaydedin.

## <a name="build-your-iot-edge-solution"></a>IoT Edge çözümünüzü derleyin

Bir önceki bölümde bir IoT Edge çözümü oluşturdunuz ve **CSharpFunction** modülüne makine sıcaklığının kabul edilebilir eşiğin altında olduğunu bildiren iletileri filtrelemek için kod eklediniz. Şimdi çözümü kapsayıcı görüntüsü olarak derlemeniz ve kapsayıcı kayıt defterine göndermeniz gerekiyor.

1. Visual Studio Code tümleşik terminaline aşağıdaki komutu girerek Docker’da oturum açın. Ardından, modül görüntünüzü Azure kapsayıcı kayıt defterinize gönderin: 
     
    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Önceki adımlarda Azure kapsayıcı kayıt defterinden kopyaladığınız kullanıcı adını ve oturum açma sunucusunu kullanın. Parola istenir. Parolanızı isteme yapıştırın ve **Enter** tuşuna basın.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. VS Code gezgininde IoT Edge çözüm çalışma alanınızdaki deployment.template.json dosyasını açın. Bu dosya, IoT Edge çalışma zamanına cihaza dağıtılacak modülleri iletir. Dağıtım bildirimleri hakkında daha fazla bilgi edinmek için bkz. [IoT Edge modüllerinin kullanılmasını, yapılandırılmasını ve yeniden kullanılmasını anlama](module-composition.md).

3. Dağıtım bildiriminin **registryCredentials** bölümünü bulun. **username**, **password** ve **address** yerine kapsayıcı kayıt defterinizin kimlik bilgilerini girin. Bu bölüm, cihazınızdaki IoT Edge çalışma zamanına özel kayıt defterinizde depoladığınız kapsayıcı görüntülerini çekme izni verir. Gerçek kullanıcı adı ve parola çiftleri, git tarafından yoksayılan .env dosyasında depolanır.

5. Bu dosyayı kaydedin.

6. VS Code gezgininde deployment.template.json dosyasına sağ tıklayıp **Build IoT Edge solution** (IoT Edge çözümü oluştur) öğesini seçin. 

Visual Studio Code uygulamasına çözümünüzü derleme komutu verdiğinizde dağıtım şablonundaki bilgileri alır ve **config** adlı yeni bir klasörde deployment.json dosyası oluşturur. Ardından tümleşik terminalde `docker build` ve `docker push` komutlarını çalıştırır. Bu iki komut kodunuzu derler, işlevlerle kapsayıcı oluşturur ve kodu çözümü başlatırken belirttiğiniz kapsayıcı kayıt defterine gönderir. 

## <a name="view-your-container-image"></a>Kapsayıcı görüntünüzü açma

Kapsayıcı görüntünüz, kapsayıcı kayıt defterinize gönderildiğinde Visual Studio Code işlemin başarılı olduğunu belirten bir ileti görüntüler. İşlemin başarılı olduğunu kendiniz onaylamak isterseniz kayıt defterindeki görüntüye bakabilirsiniz. 

1. Azure portalında Azure kapsayıcı kayıt defterinize göz atın. 
2. **Depolar**'ı seçin.
3. Listede **csharpfunction** deposunu görüyor olmalısınız. Diğer ayrıntıları görmek için bu depoyu seçin.
4. **Etiketler** bölümünde **0.0.1-amd64** etiketini görmeniz gerekir. Bu etiket, derlediğiniz görüntünün sürümünü ve platformunu gösterir. Bu değerler CSharpFunction klasöründeki module.json dosyasında belirlenir. 

## <a name="deploy-and-run-the-solution"></a>Çözümü dağıtma ve çalıştırma

Hızlı başlangıçlarda yaptığınız gibi işlev modülünüzü IoT Edge cihazına dağıtmak için Azure portalını kullanabilirsiniz. Ayrıca modülleri Visual Studio Code'un içinden de dağıtabilir ve izleyebilirsiniz. Aşağıdaki bölümlerde önkoşullarda listelenen VS Code için Azure IoT Edge uzantısı kullanılmaktadır. Uzantıyı henüz yüklemediyseniz, şimdi yükleyin. 

1. **View (Görünüm)** > **Command Palette (Komut Paleti)** öğesini seçerek VS Code komut paletini açın.

2. **Azure: Sign in** komutunu arayıp çalıştırın. Azure hesabınızda oturum açmak için yönergeleri izleyin. 

3. Komut paletinde **Azure IoT Hub: Select IoT Hub** komutunu arayıp çalıştırın. 

4. IoT hub'ınızı içeren aboneliği ve ardından erişmek istediğiniz IoT hub'ını seçin.

5. VS Code gezgininde **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü seçin. 

6. IoT Edge cihazınızın adına sağ tıklayın ve ardından **Create Deployment for IoT Edge device** (IoT Edge cihazı için dağıtım oluştur) öğesini seçin. 

7. **CSharpFunction** modülünü içeren çözüm klasörüne göz atın. Config klasörünü açın, deployment.json dosyasını seçin ve ardından **Select Edge Deployment Manifest** (Edge Dağıtım Bildirimini Seç) öğesini seçin.

8. **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünü yenileyin. Yeni **CSharpFunction** ile **TempSensor** modülü ve **$edgeAgent** ile **$edgeHub** bileşenlerinin çalıştığını görmeniz gerekir. 

   ![Dağıtılan modülleri VS Code'da görüntüleme](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Komut paletinden **Azure IoT Hub: Start Monitoring D2C Message** komutunu çalıştırarak IoT hub'ınıza gelen tüm iletileri görebilirsiniz.

IoT hub'ınıza belirli bir cihazdan gelen iletilerin gösterilmesi için görünüme filtre de uygulayabilirsiniz. **Azure IoT Hub Devices** (Azure IoT Hub Cihazları) bölümünde cihaza sağ tıklayıp **Start Monitoring D2C Messages** (D2C İletilerini İzlemeye Başla) öğesini seçin.

İletileri izlemeyi durdurmak için komut paletinden **Azure IoT Hub: Stop monitoring D2C message** komutunu seçin. 


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)]

IoT cihaz platformunuza (Linux veya Windows) göre IoT Edge hizmeti çalışma zamanını kaldırın.

#### <a name="windows"></a>Windows

IoT Edge çalışma zamanını kaldırın.

```Powershell
stop-service iotedge -NoWait
sleep 5
sc.exe delete iotedge
```

Cihazınızda oluşturulan kapsayıcıları silin. 

```Powershell
docker rm -f $(docker ps -a --no-trunc --filter "name=edge" --filter "name=tempSensor" --filter "name=CSharpFunction")
```

#### <a name="linux"></a>Linux

IoT Edge çalışma zamanını kaldırın.

```bash
sudo apt-get remove --purge iotedge
```

Cihazınızda oluşturulan kapsayıcıları silin. 

```bash
sudo docker rm -f $(sudo docker ps -a --no-trunc --filter "name=edge" --filter "name=tempSensor" --filter "name=CSharpFunction")
```

Kapsayıcı çalışma zamanını kaldırın.

```bash
sudo apt-get remove --purge moby
```



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide IoT Edge cihazınız tarafından üretilen ham verileri filtreleme kodunu içeren bir Azure işlevi modülü oluşturdunuz. Kendi modüllerinizi oluşturmaya hazır olduğunuzda [Visual Studio Code için Azure IoT Edge ile Azure işlevleri Geliştirme](how-to-develop-csharp-function.md) hakkında daha fazla bilgi edinebilirsiniz. 

Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yolları öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Stream Analytics'te kayan pencere kullanarak ortalamaları bulma](tutorial-deploy-stream-analytics.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
