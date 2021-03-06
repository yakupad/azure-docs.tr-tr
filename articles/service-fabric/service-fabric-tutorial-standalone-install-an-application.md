---
title: 'Öğretici: Tek başına Service Fabric kümenize uygulama yükleme - Azure Service Fabric | Microsoft Docs'
description: Bu öğreticide, tek başına Service Fabric kümenize bir uygulama yüklemeyi öğreneceksiniz.
services: service-fabric
documentationcenter: .net
author: david-stanford
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/11/2018
ms.author: dastanfo
ms.custom: mvc
ms.openlocfilehash: 65c0ca98393fbb4f54eee2f4864218f231765904
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208582"
---
# <a name="tutorial-deploy-an-application-on-your-service-fabric-standalone-cluster"></a>Öğretici: Service Fabric tek başına kümenize uygulama dağıtma

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici serisinde, AWS üzerinde barındırılan bir tek başına küme oluşturacak ve içine bir uygulama dağıtacaksınız.

Bu öğretici, bir serinin üçüncü bölümüdür.  Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric ile ilgili "her işletim sistemi, her bulut" yaklaşımımızın bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğreticide, bu tek başına kümeyi barındırmak için gereken AWS altyapısını oluşturma işlemi gösterilmektedir.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Örnek uygulamayı indirme
> * Kümeye dağıtma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017’yi yükleyin](https://www.visualstudio.com/).
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>Uygulamayı Service Fabric kümesine dağıtma

Uygulama indirildikten sonra, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

1. Visual Studio’yu açın

2. **Dosya** > **Aç**’ı seçin

3. Git deposunu kopyaladığınız klasöre gidin ve Voting.sln dosyasını seçin

4. Çözüm Gezgini'nde `Voting` uygulama projesine sağ tıklayın ve **Yayımla**’yı seçin

5. **Bağlantı Uç Noktası** açılır listesini seçin ve kümenizdeki düğümlerden birinin genel DNS Adını girin.  Örneğin, `ec2-34-215-183-77.us-west-2.compute.amazonaws.com:19000`

6. Tercih ettiğiniz tarayıcıyı açın ve küme adresini girin (bu uygulamanın 8080 numaralı bağlantı noktasında dağıttığı bağlantı uç noktası - örneğin, ec2-34-215-183-77.us-west-2.compute.amazonaws.com:8080).

    ![Kümeden API Yanıtı](./media/service-fabric-tutorial-standalone-cluster/deployed-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Serinin üçüncü bölümünde, kümenize bir uygulamanın nasıl dağıtılacağı öğrendiniz:

> [!div class="checklist"]
> * Örnek uygulamayı indirme
> * Kümeye dağıtma

Kümenizi temizlemek için serinin dördüncü bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Kaynaklarınızı temizleme](service-fabric-tutorial-standalone-clean-up.md)