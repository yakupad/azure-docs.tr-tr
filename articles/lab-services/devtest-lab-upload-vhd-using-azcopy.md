---
title: AzCopy kullanarak Azure DevTest Labs için VHD dosyasını karşıya | Microsoft Docs
description: AzCopy kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle
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
ms.openlocfilehash: e35686e7ba7c2e88d62930082d39856673a661b6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787986"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>AzCopy kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'de VHD dosyaları, sanal makineler sağlamak için kullanılan özel görüntülerinizi oluşturmak için kullanılabilir. Aşağıdaki adımlar bir VHD dosyasının Laboratuvar ait depolama hesabına yüklemek için AzCopy komut satırı yardımcı programını kullanarak aracılığıyla yol. VHD dosyası yüklediğiniz sonra [bölüm'sonraki adımlar](#next-steps) karşıya yüklenen VHD dosyasından özel bir görüntü oluşturmak nasıl çalışılacağını bazı makaleleri listeler. Diskleri ve Azure içinde VHD'leri hakkında daha fazla bilgi için bkz: [diskler ve sanal makineler için VHD'ler hakkında](../virtual-machines/linux/about-disks-and-vhds.md)

> [!NOTE] 
>  
> AzCopy yalnızca Windows bir komut satırı yardımcı programıdır.

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar bir VHD dosyası aracılığıyla karşıya yükleme Azure DevTest Labs kullanmaya yol [AzCopy](http://aka.ms/downloadazcopy). 

1. Azure portalını kullanarak laboratuar ait depolama hesabı adını alın:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.

1. İstenen Laboratuvar labs listesinden seçin.  

1. Laboratuvar 's dikey penceresinde, seçin **yapılandırma**. 

1. Laboratuvar üzerinde **yapılandırma** dikey penceresinde, select **özel görüntülerini (VHD)**.

1. Üzerinde **özel görüntüleri** dikey penceresinde, seçin **+ Ekle**. 

1. Üzerinde **özel görüntü** dikey penceresinde, select **VHD**.

1. Üzerinde **VHD** dikey penceresinde, select **PowerShell kullanarak bir VHD'yi karşıya**.

    ![PowerShell kullanarak VHD karşıya yükle](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. **PowerShell kullanarak bir görüntüyü karşıya yüklemeden** dikey penceresinde görüntüler yapılan bir çağrı **Ekle AzureVhd** cmdlet'i. İlk parametre (*hedef*) blob kapsayıcısı için URI içerir (*yükler*) şu biçimde:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Sonraki adımlarda kullanılmak üzere tam URI not edin.

1. AzCopy kullanarak VHD dosyasını karşıya yükle:
 
1. [En güncel AzCopy sürümünü karşıdan yükleyip](http://aka.ms/downloadazcopy).

1. Bir komut penceresi açın ve AzCopy yükleme dizinine gidin. İsteğe bağlı olarak, sistem yolunuza AzCopy yükleme konumu ekleyebilirsiniz. Varsayılan olarak, AzCopy şu dizine yüklenir:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Depolama hesabı anahtarı ve blob kapsayıcısı URI kullanarak, komut istemine aşağıdaki komutu çalıştırın. *VhdFileName* değeri tırnak işaretleri içine olması gerekir. VHD dosyasını karşıya yükleme işlemi VHD dosyasını ve bağlantı hızı boyutuna bağlı olarak uzun olabilir.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DevTest Labs Azure portalını kullanarak bir VHD dosyasında özel bir görüntü oluşturun](devtest-lab-create-template.md)
- [Azure DevTest Labs PowerShell kullanarak bir VHD dosyasında özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)