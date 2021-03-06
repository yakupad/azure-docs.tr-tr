---
title: Azure Lab Services’teki bir sınıf laboratuvarına erişme | Microsoft Docs
description: Bu öğreticide, bir profesör tarafından ayarlanan sınıf laboratuvarındaki sanal makinelere erişirsiniz.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 85da6989740446c980bcb9f2c89a6c31afe488fb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651566"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Azure Lab Services’teki bir sınıf laboratuvarına erişme
Bu makalede, bir sınıf laboratuvarına nasıl erişileceği, laboratuvardaki sanal makineye nasıl bağlanılacağı ve sanal makinenin nasıl durdurulacağı açıklanmaktadır. 

## <a name="view-all-the-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüleme

1. Profesörden/eğitimciden aldığınız **kayıt URL’sine** gidin. 
2. Kaydı tamamlamak için okul hesabınızı kullanarak hizmette oturum açın. 
3. Kaydolduktan sonra, erişimine sahip olduğunuz laboratuvarlar için sanal makineleri gördüğünüzü onaylayın. 

    ![Tüm laboratuvarları görüntüleme](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Bir sınıf laboratuvarındaki sanal makineye bağlanma

1. Erişmek istediğiniz laboratuvarın sanal makinesini temsil eden kutucukta **Bağlan**’ı seçin.

    ![Tüm laboratuvarları görüntüleme](../media/how-to-use-classroom-lab/connect-button.png)
2. Sanal makinenin hazır olması birkaç dakika sürer.

    ![Sanal makineyi hazırlama](../media/how-to-use-classroom-lab/getting-virtual-machine-ready.png)
3. RDP dosyasını sabit diske kaydedip açın. 
    
    ![RDP dosyasını kaydetme](../media/how-to-use-classroom-lab/save-rdp-file.png)
4. Makinede oturum açmak için eğitimcinizden/profesörünüzden aldığınız **kullanıcı adı** ve **parola** bilgisini kullanın. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Bir sınıf laboratuvarındaki sanal makineyi durdurma

Sınıf laboratuvarını temsil eden kutucukta **Durdur**’u seçin. Sanal makine durdurulduğunda, kutucuktaki **Başlat** düğmesi etkinleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](how-to-manage-classroom-labs.md)
- [Laboratuvar ayarlama](../tutorial-create-custom-lab.md)

 