---
title: Bulut (C) - Azure IOT hub'a bağlanma Raspberry Pi'yi raspberry Pi | Microsoft Docs
description: Kurulum ve Raspberry Pi Raspberry Pi, bu öğreticide Azure bulut platformuna veri göndermek için Azure IOT hub'a bağlanma hakkında bilgi edinin.
author: rangv
manager: ''
keywords: Azure IOT raspberry pi, raspberry pi IOT hub, bulut verilerini raspberry pi göndermek, buluta raspberry pi
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 15fd21a25648925d8f78645c49e4127271a9352d
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187811"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-c"></a>Raspberry Pi için Azure IoT Hub (C) bağlanma

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Bu öğreticide, Raspbian çalıştıran Raspberry Pi çalışmanın temel bilgileri öğrenerek başlayın. Daha sonra kullanarak cihazlarınızı buluta sorunsuz bir şekilde bağlanmak nasıl öğrenin [Azure IOT hub'ı](about-iot-hub.md). Windows 10 IoT Core örnekleri için Git [Windows Dev Center](http://www.windowsondevices.com/).

Bir paket henüz yok mu? Deneyin [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md). Veya yeni bir paket satın [burada](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="what-you-do"></a>Neler

* IOT hub oluşturun.
* Bir cihaz IOT hub'ına PI sayısı için kaydedin.
* Raspberry Pi ayarlayın.
* Pi sensör verilerini IOT hub'ınıza gönderilecek örnek uygulamayı çalıştırın.

Raspberry Pi'yi oluşturduğunuz IOT hub'a bağlayın. Ardından BME280 algılayıcıdan sıcaklık ve nem verileri toplamak için Pi üzerinde bir örnek uygulamayı çalıştırın. Son olarak, IOT hub'ınıza sensör verilerini gönderin.

## <a name="what-you-learn"></a>Öğrenecekleriniz

* Azure IOT hub oluşturma ve yeni cihaz bağlantı dizesini almak nasıl.
* Pi BME280 algılayıcısı ile bağlanma.
* Pi üzerinde bir örnek uygulamayı çalıştırarak algılayıcı verilerini toplamak nasıl.
* IOT hub'ınıza sensör verilerini gönderme işlemini.

## <a name="what-you-need"></a>Ne gerekiyor

![Ne gerekiyor](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

* Raspberry Pi 2 veya Raspberry Pi 3 Pano.
* Etkin bir Azure aboneliği. Azure hesabınız yoksa, [ücretsiz Azure deneme hesabı oluşturma](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde.
* Bir izleyici, bir USB klavye ve fare Pi bağlanan.
* Bir Mac veya Windows veya Linux çalıştıran bir bilgisayar.
* Internet bağlantısı.
* 16 GB veya üzeri microSD kartı.
* İşletim sistemi görüntüsünü microSD kartı üzerine yazmak için bir USB SD bağdaştırıcı veya microSD kartı.
* 5-volt 2 amp power 6 ft mikro USB kablosu ile sağlayın.

Aşağıdaki öğeler isteğe bağlıdır:

* Bir birleştirilmiş Adafruit BME280 sıcaklık, baskısı ve nem algılayıcı.
* Bir breadboard.
* 6 F/dk anahtar kablolarına.
* Yayılmış 10 mm LED.


> [!NOTE] 
Kod örneği desteği sensör verilerini benzetimli çünkü bu öğeler isteğe bağlıdır.


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>Raspberry Pi Kurulumu

### <a name="install-the-raspbian-operating-system-for-pi"></a>PI sayısı için Raspbian işletim sistemini yükleme

MicroSD kartı Raspbian görüntüyü yüklemesi için hazırlayın.

1. Raspbian indirin.
   1. [Masaüstü ile Raspbian Jessie indirme](https://www.raspberrypi.org/downloads/raspbian/) (.zip dosyasında).
   1. Raspbian görüntünün bilgisayarınızdaki bir klasöre ayıklayın.
1. Raspbian microSD kartı yükleyin.
   1. [Etcher SD kart yazıcı yardımcı programını yükleyip](https://etcher.io/).
   1. Etcher çalıştırın ve 1. adımda ayıkladığınız Raspbian görüntüsünü seçin.
   1. MicroSD kartı sürücüyü seçin. Etcher zaten doğru sürücü seçmiş olabilirsiniz olduğunu unutmayın.
   1. MicroSD kartı Raspbian yüklemek için Flash'ı tıklatın.
   1. Yükleme tamamlandığında microSD kartı bilgisayarınızdan kaldırın. Etcher otomatik olarak çıkarır veya tamamlandıktan sonra microSD kartı çıkarır çünkü microSD kartı doğrudan kaldırmak güvenlidir.
   1. MicroSD kartı PI yerleştirin.

### <a name="enable-ssh-and-spi"></a>SSH ve SPI etkinleştir

1. Pi monitör, klavye ve fare bağlayın, PI başlatın ve ardından içinde Raspbian kullanarak oturum `pi` kullanıcı adı ve `raspberry` parolası.
1. Raspberry simgesine tıklayın > **tercihleri** > **Raspberry Pi yapılandırma**.

   ![Raspbian tercihleri menüsü](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. Üzerinde **arabirimleri** sekmesinde, belirleyin **SPI** ve **SSH** için **etkinleştirme**ve ardından **Tamam**. Yoksa ve fiziksel algılayıcınız varsa sanal sensör verilerini kullanmak istiyorsanız, bu adım isteğe bağlıdır.

   ![SPI ve Raspberry Pi üzerinde SSH'ı etkinleştir](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
SSH ve SPI etkinleştirmek için daha fazla başvuru belgeleri üzerinde bulabilirsiniz [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) ve [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

### <a name="connect-the-sensor-to-pi"></a>Pi algılayıcı bağlanma

Bir LED'i ve bir BME280 Pi gibi bağlanmak için breadboard ve anahtar kablo kullanın. Algılayıcı yoksa [bu bölümü atlayın](#connect-pi-to-the-network).

![Raspberry Pi ve algılayıcı bağlantı](media/iot-hub-raspberry-pi-kit-c-get-started/3_raspberry-pi-sensor-connection.png)

BME280 algılayıcı sıcaklık ve nem veri toplayabilir. Ve varsa cihaz ve bulut arasındaki iletişimi LED yanıp sönme. 

Algılayıcı sabitlemek için aşağıdaki bağlantı kullanın:

| Başlangıç (sensör ve LED)     | Bitiş (Pano)            | Kablo rengi   |
| -----------------------  | ---------------------- | ------------: |
| LED VDD (PIN 5G)         | Bir GPIO'yu 4 (PIN 7)         | Beyaz kablosu   |
| LED GND (PIN 6G)         | GND (PIN 6)            | Siyah kablo   |
| VDD (PIN 18F)            | 3, 3v PWR (PIN 17)      | Beyaz kablosu   |
| GND (PIN 20F)            | GND (PIN 20)           | Siyah kablo   |
| SCK (PIN 21F)            | SPI0 SCLK (PIN 23)     | Turuncu kablosu  |
| SDO (PIN 22F)            | SPI0 MISO (PIN 21)     | Sarı kablosu  |
| SDI (PIN 23F)            | SPI0 MOSI (PIN 19)     | Yeşil kablosu   |
| CS (PIN 24F)             | SPI0 CS (PIN 24)       | Mavi kablosu    |

Görüntülemek için tıklayın [Raspberry Pi 2 ve 3 PIN eşlemeleri](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referans.

Raspberry Pi'yi BME280 başarıyla bağlandıktan sonra aşağıdaki resim gibi olmalıdır.

![Bağlı Pi ve BME280](media/iot-hub-raspberry-pi-kit-c-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Pi ağa bağlanma

Pi üzerinde mikro USB kablosu ve güç kaynağı kullanarak etkinleştirin. Pi, kablolu ağa bağlanacak veya takip için Ethernet kablosu kullanın [yönergeleri Raspberry Pi Foundation'dan](https://www.raspberrypi.org/learning/software-guide/wifi/) Pi, kablosuz ağa bağlanmak için. Bir not gerek Pi'yi ağa başarıyla bağlandı sonra [Pi'yi IP adresini](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Kablolu ağa bağlı](media/iot-hub-raspberry-pi-kit-c-get-started/5_power-on-pi.jpg)


## <a name="run-a-sample-application-on-pi"></a>Pi üzerinde bir örnek uygulamayı çalıştırma

### <a name="login-to-your-raspberry-pi"></a>Raspberry Pi'yi oturum açın

1. Raspberry Pi'yi bağlanmak için ana bilgisayarınız aşağıdaki SSH istemcilerden birini kullanın.
   
   **Windows kullanıcıları**
   1. İndirme ve yükleme [PuTTY](http://www.putty.org/) Windows için. 
   1. IP adresini, ana bilgisayar adı (veya IP adresi) PI bölümüne kopyalayın ve SSH bağlantı türü olarak seçin.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac ve Ubuntu kullanıcıları**
   
   Yerleşik bir SSH istemcisi, Ubuntu veya Macos'ta kullanın. Çalıştırmanız gerekebilir `ssh pi@<ip address of pi>` Pi SSH aracılığıyla bağlanmak için.
   > [!NOTE] 
   Varsayılan kullanıcı adı `pi` , ve parola `raspberry`.


### <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Aşağıdaki komutu çalıştırarak örnek uygulamayı kopyalayın:

   ```bash
   sudo apt-get install git-core
   git clone https://github.com/Azure-Samples/iot-hub-c-raspberrypi-client-app.git
   ```

2. Kurulum betiğini çalıştırın:

   ```bash
   cd ./iot-hub-c-raspberrypi-client-app
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```

   > [!NOTE] 
   > Varsa, **fiziksel BME280 yoksa**, kullanabilirsiniz '--simulated-data' sıcaklık ve nem verileri benzetimini yapmak için komut satırı parametresi olarak. `sudo ./setup.sh --simulated-data`

### <a name="build-and-run-the-sample-application"></a>Örnek uygulaması derleme ve çalıştırma

1. Örnek uygulama, aşağıdaki komutu çalıştırarak oluşturun:

   ```bash
   cmake . && make
   ```
   ![Derleme çıkışı](media/iot-hub-raspberry-pi-kit-c-get-started/7_build-output.png)

1. Aşağıdaki komutu çalıştırarak örnek uygulamayı çalıştırın:

   ```bash
   sudo ./app '<DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Kopyalama cihaz bağlantı dizesini tek tırnak yapıştırma emin olun.


Algılayıcı verilerini ve IOT hub'ınıza gönderdiği iletileri gösterir aşağıdaki çıktıyı görmeniz gerekir.

![Çıkış - IOT hub'ınıza Raspberry Pi'dan gönderilen algılayıcı verileri](media/iot-hub-raspberry-pi-kit-c-get-started/8_run-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Algılayıcı verilerini toplamak ve IOT hub'ına göndermek için örnek bir uygulama çalıştırdınız. Raspberry Pi'yi Raspberry Pi'yi bir komut satırı arabirimi, IOT hub'ı veya gönderme iletilerini gönderilen iletileri görmek için bkz: [Yönet bulut cihaz iothub-explorer öğreticiyle Mesajlaşma](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
