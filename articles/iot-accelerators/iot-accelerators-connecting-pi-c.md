---
title: Raspberry Pi C - Azure'ı kullanarak Uzaktan izleme sağlama | Microsoft Docs
description: C dilinde yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını bir Raspberry Pi cihazın bağlanması açıklar
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 23e84a8d577bb1c4950de3acd76b0f8528551ae0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611450"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Raspberry Pi'yi Cihazınızı Uzaktan izleme çözüm hızlandırıcısına (C) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, fiziksel bir cihazı Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir. Kısıtlanmış cihazlarında çalışan en katıştırılmış uygulamalarında olduğu gibi Raspberry Pi cihaz uygulaması için istemci kodu c dilinde yazılan Bu öğreticide, Raspbian işletim sistemi çalıştıran Raspberry Pi üzerinde uygulama oluşturun.

### <a name="required-hardware"></a>Gerekli donanım

Raspberry Pi komut satırında bilgisayarlarına uzaktan bağlanabilmelerini sağlamak için bir masaüstü bilgisayar.

[Microsoft IOT Starter Kit, Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) veya eşdeğer bileşenleri. Bu öğreticide setindeki aşağıdaki öğeleri kullanır:

- Raspberry Pi 3
- (İle NOOBS) MicroSD kartı
- Bir Mini USB kablosu
- Ethernet kablosu

### <a name="required-desktop-software"></a>Gerekli masaüstü yazılımı

Komut satırı Raspberry Pi üzerinde uzaktan erişim sağlamak için Masaüstü makinenizde SSH istemcisi gerekir.

- Windows, bir SSH istemcisi içermez. Kullanmanızı öneririz [PuTTY](http://www.putty.org/).
- Çoğu Linux dağıtımları ve MAC'te SSH komut satırı yardımcı programı içerir. Daha fazla bilgi için [SSH kullanarak Linux veya Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Raspberry Pi'yi yazılım gerekli

Bu makalede, en son sürümünü yüklediğinizden varsayılır [Raspberry Pi'yi Raspbian OS'de](https://www.raspberrypi.org/learning/software-guide/quickstart/).

Aşağıdaki adımları Raspberry Pi'yi çözüm hızlandırıcısına bağlanan bir C uygulaması oluşturmak için hazırlama işlemini gösterir:

1. Kullanarak, Raspberry Pi bağlanmak **ssh**. Daha fazla bilgi için [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) üzerinde [Raspberry Pi Web sitesi](https://www.raspberrypi.org/).

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Raspberry Pi'yi gerekli geliştirme araçları ve kitaplıkları eklemek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get install g++ make cmake gcc git libssl1.0-dev build-essential curl libcurl4-openssl-dev uuid-dev
    ```

1. İndirme, oluşturun ve IOT Hub istemci kitaplıkları Raspberry Pi'yi yüklemek için aşağıdaki komutları kullanın:

    ```sh
    cd ~
    git clone --recursive https://github.com/azure/azure-iot-sdk-c.git
    mkdir cmake
    cd cmake
    cmake ..
    make
    sudo make install
    ```

## <a name="create-a-project"></a>Proje oluşturma

Kullanarak aşağıdaki adımları tamamlayın **ssh** Raspberry Pi'yi bağlantısı:

1. Adlı bir klasör oluşturun `remote_monitoring` giriş klasörünüzde Raspberry Pi üzerinde. Kabuğunuz bu klasöre gidin:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Dört dosyaları oluşturmak **main.c**, **remote_monitoring.c**, **remote_monitoring.h**, ve **CMakeLists.txt** içinde `remote_monitoring` klasör.

1. Bir metin düzenleyicisinde açın **remote_monitoring.c** dosya. Raspberry Pi üzerinde kullanabilirsiniz **nano** veya **VI** metin düzenleyici. Aşağıdaki `#include` deyimlerini ekleyin:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include <string.h>
    ```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

Kaydet **remote_monitoring.c** dosya ve düzenleyiciden çıkın.

## <a name="add-code-to-run-the-app"></a>Uygulamayı çalıştırmak için kod ekleyin

Bir metin düzenleyicisinde açın **remote_monitoring.h** dosya. Aşağıdaki kodu ekleyin:

```c
void remote_monitoring_run(void);
```

Kaydet **remote_monitoring.h** dosya ve düzenleyiciden çıkın.

Bir metin düzenleyicisinde açın **main.c** dosya. Aşağıdaki kodu ekleyin:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

Kaydet **main.c** dosya ve düzenleyiciden çıkın.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Aşağıdaki adımları nasıl kullanılacağını açıklayan *CMake* istemci uygulamanızı oluşturmak için.

1. Bir metin düzenleyicisinde açın **CMakeLists.txt** dosyası `remote_monitoring` klasör.

1. İstemci uygulamanızı nasıl tanımlamak için aşağıdaki yönergeleri ekleyin:

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "/usr/local/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
      serializer
      iothub_client_mqtt_transport
      umqtt
      iothub_client
      aziotsharedutil
      parson
      pthread
      curl
      ssl
      crypto
      m
    )
    ```

1. Kaydet **CMakeLists.txt** dosya ve düzenleyiciden çıkın.

1. İçinde `remote_monitoring` klasörünü depolamak için bir klasör oluşturun *olun* CMake oluşturan dosyaları. Ardından çalıştırın **cmake** ve **olun** gibi komutlar:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. İstemci uygulamasını çalıştırın ve IOT Hub'ına telemetri gönderme:

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
