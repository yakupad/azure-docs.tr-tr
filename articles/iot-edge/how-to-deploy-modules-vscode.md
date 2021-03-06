---
title: Azure IOT Edge modüllerini (VS Code) dağıtma | Microsoft Docs
description: Modüller IOT Edge cihazına dağıtmak için Visual Studio Code'u kullanma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5867012e0322e520b4ef234599fa25a2ec08138d
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325777"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Visual Studio code'dan Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin. 

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra IOT Edge cihazına dağıtım göndermek için bu dosyayı kullanma gösterilmektedir. Birden fazla cihazda kendi paylaşılan etiketlere göre hedefleyen bir dağıtım oluşturma hakkında daha fazla bilgi için bkz. [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki. 
* Bir [IOT Edge cihazı](how-to-register-device-portal.md) yüklü olan bir IOT Edge çalışma zamanı ile. 
* [Visual Studio Code](https://code.visualstudio.com/).
* Visual Studio Code için [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

Visual Studio Code kullanarak modüllerini dağıtmak için dağıtım bildirimi olarak yerel olarak kaydedin. bir. JSON dosyası. Cihazınıza yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolu kullanır.

Örnek olarak bir modülü ile temel bir dağıtım bildirimi şöyledir:

   ```json
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```
   
## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınıza erişmek için oturum açın

Visual Studio Code için Azure IOT uzantıları, IOT hub'ınıza işlemleri gerçekleştirmek için kullanabilirsiniz. Bu işlemleri çalışmak Azure hesabınızda oturum açın ve üzerinde çalıştığınız IOT hub'ı seçmek gerekir.

1. Visual Studio Code'da açmak **Gezgini** görünümü.

2. Explorer alt kısmında, Genişlet **Azure IOT Hub cihazları** bölümü. 

   ![Azure IOT Hub cihazları genişletin](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

3. Tıklayarak **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, üst bilgisinin üzerinde gezdirin. 

4. Seçin **IOT hub'ını seçin**.

5. Azure hesabınızda oturum açmadınız, bunu yapmak için yönergeleri izleyin. 

6. Azure aboneliğinizi seçin. 

7. IOT hub'ınızı seçin. 


## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

Modül bilgileri yapılandırdığınız dağıtım bildirimini uygulayarak modülleri cihazınıza dağıtın. 

1. Visual Studio Code Gezgini görünümünde genişletin **Azure IOT Hub cihazları** bölümü. 

2. Dağıtım bildirimi ile yapılandırmak istediğiniz cihaza sağ tıklayın. 

3. Seçin **IOT Edge cihazı için dağıtım oluşturma**. 

4. Kullanmak istediğiniz dağıtım bildirim JSON dosyasına gidin ve tıklayın **Edge dağıtım bildirimi seçin**. 

   ![Edge dağıtım bildirimi seçin](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)


Dağıtımınızın sonuçlarını, VS Code çıktısında yazdırılır. Başarılı dağıtımlarında hedef cihaza çalışıyorsa birkaç dakika içinde uygulanan ve internet'e bağlı. 

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Cihazınıza modülleri dağıttıktan sonra bunların tümünün görüntüleyebilirsiniz **Azure IOT Hub cihazları** bölümü. Genişletmek için IOT Edge cihazınızın yanındaki oku seçin. Çalışmakta olan tüm modülleri görüntülenir. 

Kısa bir süre önce yeni modülleri bir cihaza dağıtılan, üzerine **Azure IOT Hub cihazları** bölüm başlığı ve Görünümü güncelleştirmek için yenile simgesini seçin. 

Bir modül, modül ikizi görüntüleyip adına sağ tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)