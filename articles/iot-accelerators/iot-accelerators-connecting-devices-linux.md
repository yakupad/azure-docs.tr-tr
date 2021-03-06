---
title: Uzaktan izleme c - Azure Linux cihaz sağlamayı | Microsoft Docs
description: Linux üzerinde çalışan C dilinde yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını için bir cihaz bağlamak açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 5d7d6522dc663f13ce40cc638ba90ac4043d435c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611421"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-linux"></a>Cihazınızı Uzaktan izleme çözüm Hızlandırıcısını için (Linux) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, fiziksel bir cihazı Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir.

## <a name="create-a-c-client-project-on-linux"></a>Linux üzerinde C istemci projesi oluşturma

Kısıtlanmış cihazlarında çalışan en katıştırılmış uygulamalarında olduğu gibi cihaz uygulaması için istemci kodu c dilinde yazılan Bu öğreticide, Ubuntu (Linux) çalıştıran bir makinede uygulama oluşturun.

Bu adımları tamamlamak için Ubuntu sürümü 15.04 veya üzerini çalıştıran bir cihaz gerekir. Devam etmeden önce aşağıdaki komutu kullanarak Ubuntu Cihazınızda önkoşul paketleri yükleyin:

```sh
sudo apt-get install cmake gcc g++
```

### <a name="install-the-client-libraries-on-your-device"></a>Cihazınıza istemci kitaplıkları yükleme

Azure IOT Hub istemci kitaplıkları, Ubuntu cihaz kullanarak yükleyebileceğiniz bir paket olarak kullanılabilir **apt-get** komutu. Ubuntu bilgisayarınızdaki IOT Hub istemci kitaplığı ve üstbilgi dosyaları içeren paket yüklemek için aşağıdaki adımları tamamlayın:

1. Bir Kabuğu'nda AzureIoT depoyu bilgisayarınıza ekleyin:

    ```sh
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

1. Azure-IOT-sdk-c-geliştirme paketi yükleyin

    ```sh
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

### <a name="install-the-parson-json-parser"></a>Parson JSON ayrıştırıcısını yüklemek

IOT Hub istemci kitaplıkları Parson JSON ayrıştırıcı ileti yüklerini ayrıştırmak için kullanın. Bilgisayarınızda uygun bir klasörde aşağıdaki komutu kullanarak Parson GitHub deposunu kopyalayın:

```sh
git clone https://github.com/kgabis/parson.git
```

### <a name="prepare-your-project"></a>Projenizi hazırlama

Ubuntu makinenizde adlı bir klasör oluşturun `remote_monitoring`. İçinde `remote_monitoring` klasörü:

- Dört dosyaları oluşturmak `main.c`, `remote_monitoring.c`, `remote_monitoring.h`, ve `CMakeLists.txt`.
- Adlı bir klasör oluşturma `parson`.

Dosyaları kopyalama `parson.c` ve `parson.h` Parson depoya yerel kopyasından `remote_monitoring/parson` klasör.

Bir metin düzenleyicisinde açın `remote_monitoring.c` dosya. Aşağıdaki `#include` deyimlerini ekleyin:

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

## <a name="add-code-to-run-the-app"></a>Uygulamayı çalıştırmak için kod ekleyin

Bir metin düzenleyicisinde açın `remote_monitoring.h` dosya. Aşağıdaki kodu ekleyin:

```c
void remote_monitoring_run(void);
```

Bir metin düzenleyicisinde açın `main.c` dosya. Aşağıdaki kodu ekleyin:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

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

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```

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
