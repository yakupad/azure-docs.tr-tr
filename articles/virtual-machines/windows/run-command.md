---
title: Azure'da bir Windows VM'de PowerShell betikleri çalıştırma
description: Bu konuda, PowerShell betikleri Çalıştır komutunu kullanarak bir Azure Windows sanal makine içinde çalıştırma işlemi açıklanır
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: ad0366d333266af0b16a7acaaa4519f20a951a80
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342717"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>PowerShell betiklerini Windows VM'nizi ile Çalıştır komutunu çalıştırın.

Komutu kullanan bir Azure Windows VM Kabuk PowerShell betikleri çalıştırmak için VM Aracısı çalıştırın. Bu betikler genel bir makine ya da uygulama yönetimi için kullanılabilir ve hızlı bir şekilde tanılayın ve VM erişimi ve ağ sorunları çözmek ve VM'ye iyi bir duruma geri almak için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Sanal makinelerinizi erişmek için kullanılan birden çok seçenek vardır. Komutunu çalıştırın, sanal makinelerinizde uzaktan VM Aracısı'nı kullanarak komut dosyalarını çalıştırabilirsiniz. Run komutu Azure Portalı aracılığıyla kullanılabilir [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Bu yetenek, burada bir sanal makineleri içinde bir betik çalıştırmak istediğiniz ve sorun giderme ve RDP sahip olmayan bir sanal makine düzeltmek için yalnızca yollarından biri veya SSH bağlantı noktası açık hatalı ağ veya yönetici kullanıcının nedeniyle tüm senaryolarda kullanışlıdır yapılandırma.

## <a name="restrictions"></a>Kısıtlamalar

Çalıştır komutu kullanırken aşağıdaki kısıtlamalar uygulanır:

* Çıkış son 4096 bayt ile sınırlı
* Bir betiği çalıştırmak için minimum süre yaklaşık 20 saniyedir
* Sistemi olarak Windows üzerinde çalıştırılan betikler
* Aynı anda tek bir betik çalıştırabilir
* Çalışan bir betik iptal edilemiyor
* Bir betiği çalıştırabilirsiniz en uzun süreyi sonra hangi BT zaman aşımına uğrar 90 dakika olan
* VM'den giden bağlantı betik sonuçlarını döndürmek için gereklidir.

## <a name="run-a-command"></a>Bir komut çalıştırın

Bir VM'de gidin [Azure](https://portal.azure.com) seçip **komutu çalıştırın** altında **OPERATIONS**. VM'de çalıştırmak için kullanılabilir komutların listesini ile sunulur.

![Komut listesini çalıştırın.](./media/run-command/run-command-list.png)

Bir komutu çalıştırmak için seçin. Bazı komutlar, isteğe bağlı veya gerekli giriş parametreleri olabilir. Bu komutlar için parametreleri giriş değerleri sağlamasına metin alanları olarak sunulur. Her komut genişleterek çalıştırıldığı betik görüntüleyebilirsiniz **komut dosyasını Göster**. **RunPowerShellScript** kendi özel betik sağlamak sağladığından diğer komutlardan farklıdır.

> [!NOTE]
> Yerleşik komutlar düzenlenebilir değil.

Komut seçilir bitince **çalıştırma** betiği çalıştırmak için. Betik çalışır ve tamamlandığında, çıktı ve hatalar çıkış penceresinde döndürür. Çalışan bir örnek çıktı aşağıdaki ekran görüntüsünde gösterilmektedir **RDPSettings** komutu.

![Komut betiği çıktısını çalıştırın](./media/run-command/run-command-script-output.png)

## <a name="commands"></a>Komutlar

Bu tabloda Windows Vm'leri için kullanılabilir komutların listesini gösterir. **RunPowerShellScript** komutu istediğiniz herhangi bir özel betik çalıştırmak için kullanılabilir.

|**Ad**|**Açıklama**|
|---|---|
|**RunPowerShellScript**|Bir PowerShell betiğini yürütür|
|**EnableRemotePS**|Uzak PowerShell etkinleştirmek için makineyi yapılandırır.|
|**EnableAdminAccount**|Yerel yönetici hesabını devre dışı bırakıldı ve bu durumda bağlayabileceğinizi denetler.|
|**IP yapılandırması**| Ayrıntılı bilgileri IP adresi, alt ağ maskesi ve varsayılan ağ geçidi IP'ye her bağdaştırıcı için.|
|**RDPSettings**|Kayıt defteri ayarları ve etki alanı ilkesi ayarlarını denetler. Makine bir etki alanının parçası olan veya ayarları varsayılan değerlerine değiştiriyorsa İlkesi eylemleri önerir.|
|**ResetAccountPassword**| Yerleşik yönetici hesabı parolasını sıfırlar.|
|**ResetRDPCert**|RDP dinleyiciye bağlı SSL sertifikası'nı kaldırır ve RDP listerner güvenlik varsayılana geri yükler. Sertifika herhangi bir sorun görürseniz, bu betiği kullanın.|
|**SetRDPPort**|Ayarlar varsayılan veya kullanıcı Uzak Masaüstü bağlantıları için bağlantı noktası numarası belirtildi. Bağlantı noktasına gelen erişim için güvenlik duvarı kuralı sağlar.|

## <a name="powershell"></a>PowerShell

Bir örneği verilmiştir kullanarak [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet'i bir Azure VM'de bir PowerShell betiğini çalıştırmak için.

```azurepowershell-interactive
Invoke-AzureRmVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Komutu Çalıştır erişimi sınırlandırma

Çalıştırma komutları listesi veya bir komut ayrıntılarını gösteren gerektiren `Microsoft.Compute/locations/runCommands/read` izni, yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol ve daha yüksek.

Bir komut çalıştırmak gerektirir `Microsoft.Compute/virtualMachines/runCommand/action` izni olan [katkıda bulunan](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol ve daha yüksek.

Birini kullanabilirsiniz [yerleşik](../../role-based-access-control/built-in-roles.md) rolleri veya oluşturma bir [özel](../../role-based-access-control/custom-roles.md) rol Çalıştır komutunu kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bkz, [betikler, Windows sanal makinesinde](run-scripts-in-vm.md) , sanal betikleri ve komutları uzaktan çalıştırmak için diğer yollar hakkında bilgi edinmek için.