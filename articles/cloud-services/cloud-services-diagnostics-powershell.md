---
title: PowerShell kullanarak Azure bulut hizmetlerinde tanılamayı etkinleştirme | Microsoft Docs
description: PowerShell kullanarak bulut Hizmetleri için tanılamayı etkinleştirme hakkında bilgi edinin
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 66e08754-8639-4022-ae18-4237749ba17d
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: jeconnoc
ms.openlocfilehash: c46c0665eefd7615bf90aeca7b918ddf9195237f
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004872"
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>PowerShell kullanarak Azure bulut hizmetlerinde tanılamayı etkinleştirme
Uygulama günlükleri gibi Tanılama verileri toplayabilirsiniz performans sayaçları vb. Azure tanılama uzantısını kullanarak bir bulut hizmetinden. Bu makalede, PowerShell kullanarak bir bulut hizmeti için Azure tanılama uzantısını etkinleştirmeyi açıklar.  Bkz: [Azure PowerShell'i yükleme ve yapılandırma konusunda](/powershell/azure/overview) Bu makale için gereken önkoşulları için.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Bulut Hizmeti dağıtımının bir parçası olarak tanılama uzantısını etkinleştirme
Bu yaklaşım, bulut hizmeti dağıtımı bir parçası olarak tanılama uzantısını burada etkinleştirilebilir senaryoları, sürekli tümleştirme türü için geçerlidir. Yeni bir bulut hizmeti dağıtımını oluştururken geçirerek tanılama uzantısını etkinleştirebilirsiniz *ExtensionConfiguration* parametresi [yeni AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet'i. *ExtensionConfiguration* parametresi kullanılarak oluşturulabilir tanılama yapılandırmalarını bir dizi alan [yeni AzureServiceDiagnosticsExtensionConfig](/powershell/module/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet'i.

Aşağıdaki örnek, tanılama WebRole ve WorkerRole, her farklı tanılama yapılandırması olan bir bulut hizmeti için nasıl olanak sağlayabileceğiniz gösterir.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Tanılama yapılandırma dosyası belirtiyorsa bir `StorageAccount` öğesi bir depolama hesabı adı ile sonra `New-AzureServiceDiagnosticsExtensionConfig` cmdlet'i otomatik olarak bu depolama hesabı kullanır. Bunun işe yaraması için depolama hesabı dağıtılan bulut hizmeti ile aynı abonelikte olması gerekiyor.

Azure SDK'sı MSBuild tarafından oluşturulan uzantısı yapılandırma dosyalarını yayımlama 2.6 ileriye doğru hedef çıkış (.cscfg) hizmet yapılandırma dosyasında belirtilen tanılama yapılandırma dizesinde göre depolama hesabı adını içerir. Aşağıdaki betikte Yayımla hedef çıktısından uzantısı yapılandırma dosyalarını ayrıştırmak ve tanılama uzantısını her rol için bulut hizmetini dağıtırken, yapılandırma gösterilmektedir.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online, bulut Hizmetleri tanılama uzantısını otomatik dağıtımlar için benzer bir yaklaşım kullanır. Bkz: [Yayımla AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) tam bir örnek.

Hayır ise `StorageAccount` olarak geçirmenize gerek sonra tanılama Yapılandırması'nda belirtilen *StorageAccountName* cmdlet'e parametre. Varsa *StorageAccountName* parametresi belirtilmediyse, ardından cmdlet her zaman bir tanılama yapılandırma dosyasında belirtilen ve bir parametre içinde belirtilen depolama hesabı kullanır.

Tanılama depolama hesabı bulut hizmetinden farklı bir abonelikte olduğundan sonra açıkça geçirin gerek *StorageAccountName* ve *StorageAccountKey* cmdlet parametreleri. *StorageAccountKey* cmdlet, otomatik olarak sorgulamak ve tanılama uzantısı etkinleştirilirken anahtar değeri ayarlamak gibi tanılama depolama hesabı aynı abonelikte değilse parametresi gerekli değildir. Ancak, anahtar aracılığıyla tanılama depolama hesabı farklı bir abonelikte olduğundan sonra cmdlet anahtarını otomatik olarak almak mümkün olmayabilir ve açıkça ihtiyacınız varsa belirtmeniz *StorageAccountKey* parametresi.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Mevcut bir Bulut Hizmetinde tanılama uzantısını etkinleştirme
Kullanabileceğiniz [kümesi AzureServiceDiagnosticsExtension](/powershell/module/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'ini etkinleştirin veya zaten çalışan bir bulut hizmeti Tanılama yapılandırmasını güncelleştirin.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Güncel tanılama uzantı yapılandırmasını alma
Kullanım [Get-AzureServiceDiagnosticsExtension](/powershell/module/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) geçerli bir bulut hizmeti Tanılama yapılandırmasını almak için cmdlet.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>Tanılama uzantısını kaldırma
Kullanabileceğiniz bir bulut hizmeti tanılama devre dışı bırakmak üzere [Remove-AzureServiceDiagnosticsExtension](/powershell/module/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'i.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Tanılama uzantısını kullanarak etkinleştirilirse *kümesi AzureServiceDiagnosticsExtension* veya *yeni AzureServiceDiagnosticsExtensionConfig* olmadan *rol*parametresi seçeneğini kaldırabilirsiniz uzantısını kullanarak *Remove-AzureServiceDiagnosticsExtension* olmadan *rol* parametresi. Varsa *rol* parametresi, uzantıyı daha sonra etkinleştirme de uzantı kaldırılırken kullanılmalıdır kullanıldı.

Tanılama uzantısını her bir rolden kaldırmak için:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Sonraki Adımlar
* Sorunlarını gidermek için Azure tanılama ve diğer teknikleri kullanma ile ilgili ek kılavuzlar için bkz: [Azure bulut Hizmetleri ve sanal Makineler'de tanılamayı etkinleştirme](cloud-services-dotnet-diagnostics.md).
* [Tanılama yapılandırma şeması](https://msdn.microsoft.com/library/azure/dn782207.aspx) tanılama uzantısı için çeşitli xml yapılandırma seçeneklerini açıklar.
* Sanal makineler için tanılama uzantısını etkinleştirme hakkında bilgi için bkz: [izleme ve tanılama Azure Resource Manager şablonu kullanarak bir Windows sanal makinesi oluşturma](../virtual-machines/windows/extensions-diagnostics-template.md)
