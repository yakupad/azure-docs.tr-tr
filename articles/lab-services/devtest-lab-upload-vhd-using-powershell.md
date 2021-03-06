---
title: PowerShell kullanarak Azure DevTest Labs için VHD dosyasını karşıya | Microsoft Docs
description: PowerShell kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: ea2687c61239e893f46dfa12c5b822a51823e1f3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788735"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>PowerShell kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'de VHD dosyaları, sanal makineler sağlamak için kullanılan özel görüntülerinizi oluşturmak için kullanılabilir. Aşağıdaki adımlar bir VHD dosyasının Laboratuvar ait depolama hesabına yüklemek için PowerShell kullanılarak üzerinden yol. VHD dosyası yüklediğiniz sonra [bölüm'sonraki adımlar](#next-steps) karşıya yüklenen VHD dosyasından özel bir görüntü oluşturmak nasıl çalışılacağını bazı makaleleri listeler. Diskleri ve Azure içinde VHD'leri hakkında daha fazla bilgi için bkz: [diskler ve sanal makineler için VHD'ler hakkında](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar PowerShell kullanarak Azure DevTest Labs için bir VHD dosyasını karşıya yükleme aracılığıyla yol. 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar 's dikey penceresinde, seçin **yapılandırma**. 

1. Laboratuvar üzerinde **yapılandırma** dikey penceresinde, select **özel görüntülerini (VHD)**.

1. Üzerinde **özel görüntüleri** dikey penceresinde, seçin **+ Ekle**. 

1. Üzerinde **özel görüntü** dikey penceresinde, select **VHD**.

1. Üzerinde **VHD** dikey penceresinde, select **PowerShell kullanarak bir VHD'yi karşıya**.

    ![PowerShell kullanarak VHD karşıya yükle](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. Üzerinde **PowerShell kullanarak bir görüntüyü karşıya yüklemeden** dikey penceresinde, oluşturulan PowerShell komut dosyası bir metin düzenleyicisine kopyalayın.

1. Değiştirme **LocalFilePath** parametresinin **Ekle AzureVhd** karşıya yüklemek istediğiniz VHD dosyasının konumunu gösterecek şekilde cmdlet'i.

1. Bir PowerShell isteminde çalıştırın **Ekle AzureVhd** cmdlet (değiştirilmiş ile **LocalFilePath** parametresi).

> [!WARNING] 
> 
> VHD dosyasını karşıya yükleme işlemi VHD dosyasını ve bağlantı hızı boyutuna bağlı olarak uzun olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs Azure portalını kullanarak bir VHD dosyasında özel bir görüntü oluşturun](devtest-lab-create-template.md)
- [Azure DevTest Labs PowerShell kullanarak bir VHD dosyasında özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
