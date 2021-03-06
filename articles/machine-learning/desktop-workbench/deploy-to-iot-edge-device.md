---
title: Bir Azure Machine Learning modeli için bir Azure IOT sınır cihazı dağıtma | Microsoft Docs
description: Bu belgede, Azure IOT sınır cihazları Azure Machine Learning modellerini nasıl dağıtılabilir açıklanmaktadır.
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.topic: article
ms.date: 2/1/2018
ms.openlocfilehash: 1dffdee032c5b079aa5b81284cebe8f6471efebd
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833647"
---
# <a name="deploy-an-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Bir Azure IOT sınır cihazı bir Azure Machine Learning modeli dağıtma

Docker tabanlı web Hizmetleri olarak kapsayıcılı tüm Azure Machine Learning modellerini Azure IOT sınır cihazları üzerinde de çalıştırabilirsiniz. Ek komut dosyaları ve yönergeleri bulunabilir [AI Araç Seti için Azure IOT kenar](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Model faaliyete
Yönergeleri izleyerek modelinizi faaliyete [Azure Machine Learning modeli Yönetim Web hizmet dağıtımı](model-management-service-deploy.md) modelinizi Docker görüntüsü oluşturulamadı.

## <a name="deploy-to-azure-iot-edge"></a>Azure IOT kenarına dağıtma
Azure IOT kenar bulut analizi ve özel iş mantığı cihazlara taşır. Tüm Machine Learning modellerini IOT kenar cihazlarda çalıştırabilirsiniz. Bir IOT kenar aygıtı kurma ve dağıtımı oluşturmak için belgeleri şu adreste bulunabilir: [aka.ms/azure-IOT-edge-doc](https://aka.ms/azure-iot-edge-doc).

Dikkat edilecek ek noktalar şunlardır:

### <a name="add-registry-credentials-to-the-edge-runtime-on-your-edge-device"></a>Edge aygıtınızda kenar çalışma zamanı için kayıt defteri kimlik bilgilerini ekleyin
Çalışma zamanı kapsayıcısı çekmesini erişebilir şekilde IOT kenar kullanmakta olduğunuz nerede makinede kaydınız kimlik bilgilerini ekleyin.

Windows için şu komutu çalıştırın:
```cmd/sh
iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```
Linux için şu komutu çalıştırın:
```cmd/sh
sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```

### <a name="find-the-machine-learning-container-image-location"></a>Machine Learning kapsayıcı görüntü konumunu bulma
Machine Learning kapsayıcı görüntüsünün konumu gerekir. Kapsayıcı görüntü konumu bulmak için:

1. [Azure portalında](http://portal.azure.com/) oturum açın.
2. İçinde **Azure kapsayıcı kayıt defteri**, incelemek istediğiniz kayıt defteri seçin.
3. Kayıt defterinde tıklatın **depoları** tüm depoları ve resimlerinin listesini görmek için.













