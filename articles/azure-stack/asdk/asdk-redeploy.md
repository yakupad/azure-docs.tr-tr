---
title: Azure yığın Geliştirme Seti (ASDK) dağıtmanız | Microsoft Docs
description: Bu makalede, ASDK yeniden öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: ''
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 91b8a936215e906e6e5b7e6a4fcd0dc88bef6009
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850329"
---
# <a name="redeploy-the-asdk"></a>ASDK yeniden dağıtın
Bu makalede, Azure yığın Geliştirme Seti (ASDK) bir üretim dışı ortamda dağıtmanız öğrenin. ASDK yükseltme desteklenmediğinden, tamamen yeni bir sürüme taşımak için yeniden dağıtmanız gerekir. Ayrıca, üzerinden baştan başlamak istediğiniz herhangi bir zamanda ASDK yeniden dağıtabilirsiniz.

> [!IMPORTANT]
> ASDK yeni bir sürüme yükseltme desteklenmiyor. Azure yığın daha yeni bir sürümü değerlendirmek istediğiniz her zaman Geliştirme Seti ana bilgisayarda ASDK dağıtmanız gerekir.

## <a name="remove-azure-registration"></a>Azure kaydı Kaldır 
Azure ile ASDK yüklemenizi daha önce kaydolduysanız, ASDK dağıtarak önce kayıt kaynağı kaldırmanız gerekir. Market dağıtım ASDK yeniden dağıtırken etkinleştirmek için ASDK yeniden kaydedin. Azure aboneliğiniz ile daha önce ASDK kaydolmadıysanız, bu bölümü atlayabilirsiniz.

Kayıt kaynak kaldırmak için kullanın **Kaldır AzsRegistration** Azure yığın kaydını silmek için cmdlet. Ardından, **Kaldır AzureRMRsourceGroup** Azure aboneliğinizden Azure yığın kaynak grubunu silmek için cmdlet:

1. Ayrıcalıklı uç noktasına erişimi olan bir bilgisayarda bir yönetici olarak bir PowerShell konsolu açın. ASDK için Geliştirme Seti ana bilgisayar olmasıdır.

2. ASDK yüklemenizi kaydı ve silmek için aşağıdaki PowerShell komutlarını çalıştırın **azurestack** , Azure aboneliğinizin kaynak grubundan:

  ```Powershell    
  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Provide Azure subscription admin credentials
  Add-AzureRmAccount

  # Provide ASDK admin credentials
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"

  # Unregister Azure Stack
  Remove-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01

  # Remove the Azure Stack resource group
  Remove-AzureRmResourceGroup -Name azurestack -Force
  ```

3. Komut dosyası çalıştığında, Azure aboneliğinizin ve yerel ASDK yükleme için oturum istenir.
4. Betik tamamlandığında aşağıdaki örneklere benzer iletiler görmeniz gerekir:

    ` De-Activating Azure Stack (this may take up to 10 minutes to complete).` ` Your environment is now unable to syndicate items and is no longer reporting usage data.` ` Remove registration resource from Azure...` ` "Deleting the resource..." on target "/subscriptions/<subscription information>"` ` ********** End Log: Remove-AzsRegistration ********* `



Azure yığın artık başarıyla Azure aboneliğinizden kaydı olması gerekir. Ayrıca, Azure ile ASDK kaydolurken oluşturulmuş azurestack kaynak grubu, aynı zamanda silinmesi gerekir.

## <a name="deploy-the-asdk"></a>ASDK dağıtma
Azure yığını yeniden dağıtmak için üzerinden sıfırdan aşağıda açıklandığı gibi başlatmanız gerekir. Adımları olsun veya olmasın, Azure yığın Yükleyici (asdk-installer.ps1) komut ASDK yüklemek için kullanılan bağlı olarak farklılık gösterir.

### <a name="redeploy-the-asdk-using-the-installer-script"></a>Yükleyici komut dosyasını kullanarak ASDK yeniden dağıtın
1. ASDK bilgisayarda, yükseltilmiş bir PowerShell konsolu açın ve asdk installer.ps1 betik gidin **AzureStack_Installer** dizininde bir sistem dışı sürücü. Komut dosyasını çalıştırın ve tıklatın **yeniden**.

   ![Asdk installer.ps1 komut dosyasını çalıştır](media/asdk-redeploy/1.png)

2. Temel işletim sistemi seçin (değil **Azure yığın**) tıklatıp **sonraki**.

   ![Ana bilgisayar işletim sistemine yükleyin](media/asdk-redeploy/2.png)

3. Geliştirme Seti konak temel işletim sistemiyle yeniden başlatıldıktan sonra yerel bir yönetici olarak oturum açın. Bulup silin **C:\CloudBuilder.vhdx** önceki dağıtımının bir parçası kullanılan dosya. 

4. İçin ilk sürdü aynı adımları yineleyin [ASDK dağıtmak](asdk-install.md).

### <a name="redeploy-the-asdk-without-using-the-installer"></a>Yükleyici kullanmadan ASDK yeniden dağıtın
ASDK yüklemek için asdk installer.ps1 betik kullanmadıysanız ASDK dağıtarak önce Geliştirme Seti ana bilgisayarı el ile yapılandırmalısınız.

1. Sistem Yapılandırması yardımcı programını çalıştırarak Başlat **msconfig.exe** ASDK bilgisayarda. Üzerinde **önyükleme** sekmesinde, ana bilgisayar işletim sistemi (Azure yığını değil) seçin, **varsayılan olarak ayarla**ve ardından **Tamam**. Tıklatın **yeniden** istendiğinde.

      ![Önyükleme yapılandırma kümesi](media/asdk-redeploy/4.png)

2. Geliştirme Seti konak temel işletim sistemiyle yeniden başlatıldıktan sonra Geliştirme Seti ana bilgisayar için yerel yönetici olarak oturum açın. Bulup silin **C:\CloudBuilder.vhdx** önceki dağıtımının bir parçası kullanılan dosya. 

3. İçin ilk sürdü aynı adımları yineleyin [PowerShell kullanarak ASDK dağıtmak](asdk-deploy-powershell.md).


## <a name="next-steps"></a>Sonraki adımlar
[ASDK yükleme sonrası yapılandırma görevleri](asdk-post-deploy.md)




