---
title: Windows kapsayıcıları Service Fabric ve VS hata ayıklama | Microsoft Docs
description: Visual Studio 2017'yi kullanarak Azure Service fabric'te Windows kapsayıcılarını hata ayıklama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/14/2018
ms.author: mikhegn
ms.openlocfilehash: 437c38a8e674fcdf06e26a7191ceecef9d901470
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968329"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2017"></a>Nasıl yapılır: Visual Studio 2017'yi kullanarak Azure Service fabric'te Windows kapsayıcılarını hata ayıklama

Visual Studio 2017 güncelleştirme 7 ile (15.7), Service Fabric Hizmetleri kapsayıcılar içindeki .NET uygulamalarına ayıklayabilirsiniz. Bu makalede, ortamınızı yapılandırın ve ardından yerel bir Service Fabric kümesinde çalışan bir kapsayıcı bir .NET uygulamasında hata ayıklama işlemini göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

* Bu Hızlı Başlangıç için Windows 10'da izleyin [Windows kapsayıcılarını çalıştırmaya yönelik Windows 10 yapılandırma](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10)
* Bu Hızlı Başlangıç için Windows Server 2016'da izleyin [Windows kapsayıcılarını çalıştırmaya yönelik Windows 2016'yı yapılandırma](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server)
* Yerel Service Fabric ortamınızı izleyerek ayarlama [Windows üzerinde geliştirme ortamınızı hazırlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)

## <a name="configure-your-developer-environment-to-debug-containers"></a>Kapsayıcıları hata ayıklamak için geliştirme ortamınızı yapılandırma

1. Docker Windows hizmeti için bir sonraki adımla devam etmeden önce çalışır durumda olduğundan emin olun.

1. Kapsayıcılar arasında DNS çözümlemesi desteklemek için kendi yerel geliştirme kümesi ayarlama makine adını kullanarak gerekir.
    1. Yönetici olarak PowerShell'i açın
    1. Genellikle SDK'sı küme kurulum klasörüne gidin `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`
    1. Betiği çalıştırmak `DevClusterSetup.ps1` parametresi `-UseMachineName`

    ``` PowerShell
      C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1 -UseMachineName
    ```

    > [!NOTE]
    > Kullanabileceğiniz `-CreateOneNodeCluster` tek düğümlü bir Küme kurulumu için. Varsayılan yerel beş düğümlü bir küme oluşturur.
    >

    Service Fabric DNS hizmeti hakkında daha fazla bilgi için bkz. [Azure Service Fabric DNS hizmetinde](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice).

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Service Fabric'teki kapsayıcıları hata ayıklama sırasında bilinen sınırlamalar

Hata ayıklama kapsayıcıları Service Fabric ve olası çözümleri ile bilinen kısıtlamaların listesi aşağıdadır:

* Localhost için ClusterFQDNorIP kullanarak DNS çözümlemesi kapsayıcılarda desteklemez.
    * Çözüm: makine adı (yukarıya bakın) kullanarak yerel küme oluşturma
* Windows 10 çalıştıran bir sanal makinede DNS yanıtı geri kapsayıcı almazsınız.
    * Çözüm: UDP Sağlama boşaltması sanal makineler NIC'de IPv4 için devre dışı bırak
    * Lütfen bu makinedeki ağ performansı bozar unutmayın.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Docker Compose kullanarak uygulamayı dağıtıldıysa DNS kullanarak aynı uygulama Hizmetleri'nde çözümleme hizmet adını Windows 10 üzerinde çalışmıyor
    * Çözüm: servicename.applicationname hizmet uç noktaları çözümlemek için kullanın.
    * https://github.com/Azure/service-fabric-issues/issues/1062
* IP adresi için ClusterFQDNorIP kullanıyorsanız, konaktaki birincil IP değiştirme DNS işlevselliğinin keser.
    * Çözüm: ana bilgisayarda yeni birincil IP kullanarak bir küme oluşturun veya makine adını kullanın. Bu tasarım gereğidir.
* Küme oluşturulurken FQDN ağ üzerinde çözümlenebilen değil, DNS başarısız olur.
    * Çözüm: ana bilgisayarın birincil IP kullanarak yerel kümeye yeniden oluşturun. Bu tasarım gereğidir.
* Bir kapsayıcı hata ayıklarken docker günlükleri yalnızca Service Fabric Explorer dahil olmak üzere Service Fabric API'leri üzerinden değil Visual Studio çıktı penceresinde kullanılabilir

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Docker kapsayıcıları Service Fabric üzerinde çalışan bir .NET uygulamasında hata ayıklama

1. Visual Studio'yu yönetici olarak çalıştırın.

1. Mevcut bir .NET uygulamasını açın veya yeni bir tane oluşturun.

1. Projeye sağ tıklayıp **Ekle -> kapsayıcı Düzenleyicisi desteği Service Fabric ->**

1. Tuşuna **F5** uygulamada hata ayıklamaya başlayın.

    Visual Studio, .NET ve .NET Core konsol ve ASP.NET proje türleri destekler.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kapsayıcıları ve özellikleri hakkında daha fazla bilgi edinmek için lütfen bu bağlantıyı izleyin: [Service Fabric kapsayıcıları genel bakış](service-fabric-containers-overview.md).