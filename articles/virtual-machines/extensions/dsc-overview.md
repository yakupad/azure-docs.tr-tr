---
title: İstenen durum yapılandırması Azure genel bakış
description: Microsoft Azure uzantısı işleyici PowerShell istenen durum yapılandırması (DSC için) kullanmayı öğrenin. Makaleyi Önkoşullar, mimari ve cmdlet'lerini içerir.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: DSC
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: 60560a4a656d0ad5df15208261ab8462f4271ec5
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012453"
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Azure istenen durum yapılandırması uzantısı işleyici giriş

Azure VM aracısı ve ilişkili uzantıları Microsoft Azure altyapı hizmetleri parçasıdır. VM uzantıları, VM işlevselliğini genişleten ve çeşitli VM yönetim işlemlerini basitleştirmek yazılım bileşenleridir.

Azure istenen durum yapılandırması (DSC) uzantısı için bir VM bootstrap için birincil kullanım örneği [Azure Otomasyonu DSC hizmet](../../automation/automation-dsc-overview.md). Bir VM önyükleme sağlar [avantajları](/powershell/dsc/metaconfig#pull-service) VM yapılandırması ve Azure Monitoring gibi işletimsel diğer araçları ile tümleştirme devam eden Yönetimi içerir.

DSC uzantı Automation DSC hizmet bağımsız olarak kullanabilirsiniz. Ancak, bu dağıtımı sırasında oluşan bir tekil eylemi içerir. Devam eden raporlama veya yapılandırma yönetimi VM yerel olarak dışında kullanılabilir.

Bu makalede her iki senaryoları hakkında bilgi sağlar: DSC uzantısı Otomasyon hazırlanması için kullanarak ve DSC uzantısı Azure SDK'sını kullanarak sanal makineleri için yapılandırmaları atamak için bir araç olarak kullanma.

## <a name="prerequisites"></a>Önkoşullar

- **Yerel makine**: Azure VM uzantısı ile etkileşim kurmak için Azure portalında veya Azure PowerShell SDK'sını kullanın.
- **Konuk Aracısı**: DSC yapılandırması tarafından yapılandırılan Azure VM, Windows Management Framework (WMF) 4.0 veya üstünü destekleyen bir işletim sistemi olmalıdır. Desteklenen işletim sistemi sürümleri tam listesi için bkz: [DSC uzantısı sürüm geçmişi](/powershell/dsc/azuredscexthistory).

## <a name="terms-and-concepts"></a>Terimleri ve kavramları

Bu kılavuz aşağıdaki kavramlar bilindiğini varsayar:

- **Yapılandırma**: bir DSC yapılandırma belgesi.
- **Düğüm**: DSC yapılandırması için hedef. Bu belgede, *düğümü* her zaman bir Azure VM başvuruyor.
- **Yapılandırma verilerini**: bir yapılandırma için ortam verileri içeren bir .psd1 dosyası.

## <a name="architecture"></a>Mimari

Azure DSC uzantısı Azure VM Aracısı framework teslim etmek, yürürlüğe ve DSC yapılandırmaları Azure Vm'lerinde çalıştırılan raporlamak için kullanır. DSC uzantısı yapılandırma belge ve parametreleri kümesini kabul eder. Hiçbir dosya sağlanırsa, bir [varsayılan yapılandırma komut dosyası](#default-configuration-script) uzantısıyla katıştırılır. Varsayılan yapılandırma komut dosyası yalnızca meta veri kümesi için kullanılan [yerel Configuration Manager](/powershell/dsc/metaconfig).

Uzantı ilk kez çağrıldığında, aşağıdaki mantık kullanarak WMF sürümünü yükler:

- Azure VM işletim sistemi Windows Server 2016 ise, hiçbir işlem yapılmadı. Windows Server 2016 yüklü PowerShell'in en son sürümünü zaten var.
- Varsa **wmfVersion** özelliği belirtilmişse, bu sürümü sanal makinenin işletim sistemiyle uyumlu olmadığı sürece WMF bu sürümü yüklü.
- Öyle değilse **wmfVersion** özelliği belirtildi, WMF ' son geçerli sürümü yüklü.

WMF yüklemek için yeniden başlatma gerekir. Yeniden başlattıktan sonra belirtilen .zip dosya uzantısı indirir **modulesUrl** sağladıysanız özelliği. Bu konum Azure Blob depolama alanına ise, bir SAS belirteci belirtebilirsiniz **sasToken** dosyaya erişmek için özellik. Yapılandırma işlevi .zip indirilir ve açılmış sonra tanımlanan **configurationFunction** .mof dosyasını oluşturmak için çalışır. Uzantısı sonra çalışır `Start-DscConfiguration -Force` oluşturulan .mof dosyasını kullanarak. Uzantı çıkış yakalar ve Azure durum kanala yazar.

### <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Azure DSC uzantısı olmaya yönelik bir varsayılan yapılandırma betiğini içerir kullanılır, yerleşik bir Azure Otomasyonu DSC hizmet VM. Betik parametrelerini yapılandırılabilir özellikleriyle hizalanır [yerel Configuration Manager](/powershell/dsc/metaconfig). Komut parametreleri için bkz: [varsayılan yapılandırma komut dosyası](dsc-template.md#default-configuration-script) içinde [istenen durum yapılandırması uzantısı Azure Resource Manager şablonları ile](dsc-template.md). Tam komut dosyası için bkz: [github'da Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-resource-manager-templates"></a>DSC uzantı Resource Manager şablonları

Çoğu senaryoda, Resource Manager dağıtım şablonları DSC uzantısı ile çalışmak için beklenen yoludur. Daha fazla bilgi ve Resource Manager dağıtım şablonları DSC uzantısı içerecek şekilde nasıl örnekleri için bkz: [istenen durum yapılandırması uzantısı Azure Resource Manager şablonları ile](dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>DSC uzantı PowerShell cmdlet'leri

DSC uzantı yönetmek için kullanılan PowerShell cmdlet'lerini en iyi etkileşimli sorun giderme ve bilgi toplama senaryolarında kullanılır. Paket, yayımlama ve DSC uzantısı dağıtımlarını izlemek için cmdlet'lerini kullanabilirsiniz. DSC uzantı için cmdlet'leri olmayan henüz çalışmak için güncelleştirilmiş [varsayılan yapılandırma komut dosyası](#default-configuration-script).

**Yayımla AzureRmVMDscConfiguration** cmdlet'i bir yapılandırma dosyasında alır, bağımlı DSC kaynakları için tarar ve bir .zip dosyası oluşturur. .Zip dosyasını yapılandırma ve yapılandırmasını yürürlüğe için gereken DSC kaynakları içerir. Cmdlet Ayrıca paket kullanarak yerel olarak oluşturabilirsiniz *- OutputArchivePath* parametresi. Aksi takdirde, cmdlet BLOB depolamaya .zip dosyası yayımlar ve bir SAS belirteci ile güvenliğini sağlar.

Arşiv klasörünün kökü, .zip dosyasına cmdlet oluşturur .ps1 yapılandırma komut dosyasıdır. Modül klasörü arşiv klasöründe bulunan kaynaklar yerleştirilir.

**Kümesi AzureRmVMDscExtension** cmdlet'i bir VM yapılandırması nesnesine PowerShell DSC uzantısı için gerekli ayarları yerleştirir.

**Get-AzureRmVMDscExtension** cmdlet'i belirli bir VM'yi DSC uzantı durumunu alır.

**Get-AzureRmVMDscExtensionStatus** cmdlet DSC uzantısı işleyici tarafından geçirilmeden DSC yapılandırması durumunu alır. Bu eylem, bir grup Vm'lere veya tek bir VM üzerinde gerçekleştirilebilir.

**Kaldır AzureRmVMDscExtension** cmdlet'i uzantısı işleyicinin belirli bir sanal makineden kaldırır. Bu cmdlet mu *değil* yapılandırmasını kaldırmak, WMF kaldırma veya VM uygulanan ayarlarını değiştirin. Yalnızca uzantısı işleyici de kaldırır. 

Resource Manager DSC uzantısı cmdlet'leri hakkında önemli bilgiler:

- Azure Resource Manager cmdlet'lerini zaman uyumlu.
- *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *sürüm*, ve *konumu*gerekli tüm parametreleri.
- *ArchiveResourceGroupName* isteğe bağlı bir parametredir. VM oluşturulduğu olandan farklı bir kaynak grubu için depolama hesabınıza ait olduğunda, bu parametre belirtebilirsiniz.
- Kullanım **otomatik güncelleştirme** kullanılabilir olduğunda uzantısı işleyici en son sürüme otomatik olarak güncelleştirmek için anahtar. Bu parametre WMF yeni bir sürümü yayımlandığında, yeniden başlatmaları VM neden olma olasılığı vardır.

### <a name="get-started-with-cmdlets"></a>Cmdlet'leri kullanmaya başlama

Azure DSC uzantısı DSC yapılandırma belgeleri doğrudan Azure VM dağıtımı sırasında yapılandırmak için kullanabilirsiniz. Bu adım, Otomasyon düğüme kaydetmeniz değil. Düğüm *değil* merkezi olarak yönetilen.

Aşağıdaki örnek bir yapılandırma basit bir örneği gösterir. Yapılandırmayı IisInstall.ps1 yerel olarak kaydedin.

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

Aşağıdaki komutları IisInstall.ps1 komut dosyası belirtilen VM'de yerleştirin. Komutları da yapılandırma yürütün ve ardından durumuna raporlar.

```powershell
$resourceGroup = 'dscVmDemo'
$location = 'westus'
$vmName = 'myVM'
$storageName = 'demostorage'
#Publish the configuration script to user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVMDscExtension -Version '2.76' -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName 'iisInstall.ps1.zip' -AutoUpdate $true -ConfigurationName 'IISInstall'
```

## <a name="azure-portal-functionality"></a>Azure portal işlevi

Portalda DSC ayarlamak için:

1. Bir VM öğesine gidin.
2. Altında **ayarları**seçin **uzantıları**.
3. Oluşturulan yeni sayfasında seçin **+ Ekle**ve ardından **PowerShell istenen durum Yapılandırması**.
4. Tıklatın **oluşturma** uzantısı bilgileri sayfanın sonundaki.

Portal şu girdi toplar:

- **Yapılandırma modülleri veya komut dosyası**: Bu alan zorunludur (form için güncelleştirilmemiş [varsayılan yapılandırma komut dosyası](#default-configuration-script)). Yapılandırma modülleri ve komut dosyaları bir yapılandırma komut dosyası varsa bir .ps1 dosyası ya da bir .zip dosyası kökünde bir .ps1 yapılandırma komut dosyalarıyla gerektirir. Bir .zip dosyası kullanıyorsanız, tüm bağımlı kaynaklarla .zip modülü klasörlerdeki eklenmesi gerekir. .Zip dosyasını kullanarak oluşturabileceğiniz **Yayımla AzureVMDscConfiguration - OutputArchivePath** Azure PowerShell SDK'da bulunan cmdlet'i. .Zip dosyasını kullanıcı blob depolama alanına yüklediğiniz ve bir SAS belirteci tarafından güvenliği.

- **Yapılandırma, modül adı**: bir .ps1 dosyasına birden çok yapılandırma işlevleri içerebilir. Ardından yapılandırma .ps1 komut dosyasının adını girin \\ ve yapılandırma işlevin adı. Örneğin, .ps1 komut dosyanızın adı configuration.ps1 ve yapılandırmasını ise **IisInstall**, girin **configuration.ps1\IisInstall**.

- **Yapılandırma değişkenleri**: yapılandırma işlevi bağımsız değişken alıyorsa, bunları burada biçiminde girin **argumentName1 value1, argumentName2 = Value2**. Bu biçim, PowerShell cmdlet'lerini veya Resource Manager şablonları yapılandırma bağımsız değişkenleri kabul edilir farklı bir biçimidir.

- **Yapılandırma verileri PSD1 dosyası**: Bu alan isteğe bağlıdır. Yapılandırmanızı .psd1 yapılandırma veri dosyası gerektiriyorsa, veri alanını seçin ve kullanıcı blob depolama alanına yüklemek için bu alanı kullanın. Yapılandırma veri dosyasının bir SAS belirteci blob depolamada tarafından korunmaktadır.

- **WMF sürüm**: VM üzerinde yüklenmesi gereken Windows Management Framework (WMF) sürümünü belirtir. Bu özellik için en son yükler WMF en son sürümünü ayarlama. Şu anda yalnızca olası bu özellik için değerleri 4.0, 5.0, 5.1 ve en son. Olası değerler tabi güncelleştirmelerdir. Varsayılan değer **son**.

- **Veri toplama**: uzantısı bir telemetri toplayacak belirler. Daha fazla bilgi için bkz: [Azure DSC uzantısı veri toplama](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/).

- **Sürüm**: yüklemek için DSC uzantısı sürümünü belirtir. Sürümleri hakkında daha fazla bilgi için bkz: [DSC uzantısı sürüm geçmişi](/powershell/dsc/azuredscexthistory).

- **Alt sürüm yükseltme otomatik**: Bu alan eşlendiği **otomatik güncelleştirme** geçiş cmdlet'leri ve yükleme sırasında en son sürüme otomatik olarak güncelleştirmek uzantı sağlar. **Evet** kullanılabilir en son sürüm kullanılacak uzantı işleyicisi girmemesini ve **Hayır** zorlayacağı **sürüm** belirtilen yüklenecek. Hiçbiri seçme **Evet** ya da **Hayır** seçme ile aynı **Hayır**.

## <a name="logs"></a>Günlükler

Uzantı için günlükleri şu konumda depolanır: `C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>`

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](/powershell/dsc/overview).
- İncelemek [Resource Manager şablonu DSC uzantısı](dsc-template.md).
- PowerShell DSC kullanarak yönetebilmeniz için daha fazla işlevsellik ve daha fazla DSC kaynakları Gözat [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
- Yapılandırmaları hassas parametreleri geçirme hakkında daha fazla bilgi için bkz [yönetmek DSC uzantısı işleyici ile güvenli kimlik bilgileri](dsc-credentials.md).