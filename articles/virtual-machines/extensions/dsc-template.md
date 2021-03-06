---
title: Azure Resource Manager şablonları ile Desired State Configuration uzantısı
description: Azure Desired State Configuration ' nı (DSC) uzantısı için Resource Manager şablon tanımı hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: DSC
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: 1dcbc8e0221689a6ece7e061d4b1a2632986ae84
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224383"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile Desired State Configuration uzantısı

Bu makalede Azure Resource Manager şablonu [Desired State Configuration ' nı (DSC) uzantısı işleyicisi](dsc-overview.md).

> [!NOTE]
> Biraz farklı şema örneklerle karşılaşabilirsiniz. Şema değişikliği Ekim 2016 sürümündeki oluştu. Ayrıntılar için bkz [güncelleştirme Önceki biçimden](#update-from-a-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Bir Windows VM için şablon örneği

Aşağıdaki kod parçacığı gider **kaynak** şablon bölümü.
DSC uzantısı varsayılan uzantı özellikleri devralır.
Daha fazla bilgi için [VirtualMachineExtension sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.).

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
    "apiVersion": "2017-12-01",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.76",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "protectedSettings": {
                "Items": {
                    "registrationKeyPrivate": "registrationKey"
                }
            },
            "publicSettings": {
                "configurationArguments": [{
                        "Name": "RegistrationKey",
                        "Value": {
                            "UserName": "PLACEHOLDER_DONOTUSE",
                            "Password": "PrivateSettingsRef:registrationKeyPrivate"
                        }
                    },
                    {
                        "RegistrationUrl": "registrationUrl"
                    },
                    {
                        "NodeConfigurationName": "nodeConfigurationName"
                    }
                ]
            }
        }
    }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Şablon örneği Windows sanal makine ölçek için ayarlar

Bir sanal makine ölçek kümesi düğümü olan bir **özellikleri** olan bölüm bir **VirtualMachineProfile, extensionprofile öğesine** özniteliği.
Altında **uzantıları**, DSC uzantısı ayrıntılarını ekleyin.

DSC uzantısı varsayılan uzantı özellikleri devralır.
Daha fazla bilgi için [VirtualMachineScaleSetExtension sınıfı](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
    "extensions": [{
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
        "apiVersion": "2017-12-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.Powershell",
            "type": "DSC",
            "typeHandlerVersion": "2.76",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "protectedSettings": {
                    "Items": {
                        "registrationKeyPrivate": "registrationKey"
                    }
                },
                "publicSettings": {
                    "configurationArguments": [{
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        {
                            "RegistrationUrl": "registrationUrl"
                        },
                        {
                            "NodeConfigurationName": "nodeConfigurationName"
                        }
                    ]
                }
            },
        }
    }]
}
```

## <a name="detailed-settings-information"></a>Ayrıntılı ayarları bilgileri

Aşağıdaki şemada kullanın **ayarları** Resource Manager şablonu Azure DSC uzantı bölümü.

Varsayılan yapılandırma betiğini için kullanılabilir olan bağımsız değişkenler listesi için bkz. [varsayılan yapılandırma betiğini](#default-configuration-script).

```json
"settings": {
    "wmfVersion": "latest",
    "configuration": {
        "url": "http://validURLToConfigLocation",
        "script": "ConfigurationScript.ps1",
        "function": "ConfigurationFunction"
    },
    "configurationArguments": {
        "argument1": "Value1",
        "argument2": "Value2"
    },
    "configurationData": {
        "url": "https://foo.psd1"
    },
    "privacy": {
        "dataCollection": "enable"
    },
    "advancedOptions": {
        "downloadMappings": {
            "customWmfLocation": "http://myWMFlocation"
        }
    }
},
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        },
        "parameterOfTypePSCredential2": {
            "userName": "UsernameValue2",
            "password": "PasswordValue2"
        }
    },
    "configurationUrlSasToken": "?g!bber1sht0k3n",
    "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}
```

## <a name="details"></a>Ayrıntılar

| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| settings.wmfVersion |dize |Sanal makinenizde yüklü Windows Management Framework (WMF) sürümünü belirtir. Bu özelliği ayarlamak **son** WMF'nin en son sürümünü yükler. Şu anda bu özellik için yalnızca olası değerler şunlardır: **4.0**, **5.0**, **5.0PP**, ve **son**. Bu olası değerler şunlardır: güncelleştirmeleri tabidir. Varsayılan değer **son**. |
| Settings.Configuration.URL |dize |DSC yapılandırması .zip dosyanızın indirileceği URL konumu belirtir. Sağlanan URL erişimi için bir SAS belirteci gerektiriyorsa, ayarlama **protectedSettings.configurationUrlSasToken** değeriniz SAS belirtecinizle değere. Bu özellik gereklidir **settings.configuration.script** veya **settings.configuration.function** tanımlanır. Bu özellikler için hiçbir değer belirtilmezse, uzantı konumu Configuration Manager'ı (LCM) meta verileri ayarlamak için varsayılan yapılandırma betiğini çağırır ve bağımsız değişkenleri iletilmelidir. |
| Settings.Configuration.Script |dize |DSC yapılandırma tanımı içeren komut dosyasının dosya adını belirtir. Bu betik tarafından belirtilen URL'den indirilen .zip dosyasının kök klasöründe olmalıdır **configuration.url** özelliği. Bu özellik gereklidir **settings.configuration.url** veya **settings.configuration.script** tanımlanır. Bu özellikler için hiçbir değer belirtilmezse, uzantı LCM meta verileri ayarlamak için varsayılan yapılandırma betiğini çağırır ve bağımsız değişkenler iletilmelidir. |
| Settings.Configuration.Function |dize |DSC yapılandırma adını belirtir. Betikte adlı yapılandırmanın eklenmelidir, **configuration.script** tanımlar. Bu özellik gereklidir **settings.configuration.url** veya **settings.configuration.function** tanımlanır. Bu özellikler için hiçbir değer belirtilmezse, uzantı LCM meta verileri ayarlamak için varsayılan yapılandırma betiğini çağırır ve bağımsız değişkenler iletilmelidir. |
| settings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenmez. |
| settings.configurationData.url |dize |DSC yapılandırma için giriş olarak kullanmak için yapılandırma verileri (.psd1) dosyasını indirileceği URL'sini belirtir. Sağlanan URL erişimi için bir SAS belirteci gerektiriyorsa, ayarlama **protectedSettings.configurationDataUrlSasToken** değeriniz SAS belirtecinizle değere. |
| settings.privacy.dataEnabled |dize |Etkinleştirir veya telemetri koleksiyonunu devre dışı bırakır. Bu özellik için yalnızca olası değerler şunlardır: **etkinleştirme**, **devre dışı**, **''**, veya **$null**. Bu özellik boş ya da boş bırakarak telemetri sağlar. Varsayılan değer **''**. Daha fazla bilgi için [Azure DSC uzantı veri koleksiyonu](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/). |
| settings.advancedOptions.downloadMappings |Koleksiyon |WMF indirileceği alternatif konumlar tanımlar. Daha fazla bilgi için [Azure DSC uzantı 2.8 ve yüklemeleri uzantısı bağımlılıkları kendi konumuyla eşleşen nasıl](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx). |
| protectedSettings.configurationArguments |Koleksiyon |DSC yapılandırmanızı geçirmek istediğiniz herhangi bir parametre tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationUrlSasToken |dize |URL erişmek için SAS belirteci belirtir, **configuration.url** tanımlar. Bu özellik şifrelenir. |
| protectedSettings.configurationDataUrlSasToken |dize |URL erişmek için SAS belirteci belirtir, **configurationData.url** tanımlar. Bu özellik şifrelenir. |

## <a name="default-configuration-script"></a>Varsayılan yapılandırma betiği

Aşağıdaki değerleri hakkında daha fazla bilgi için bkz: [yerel Configuration Manager temel ayarları](/powershell/dsc/metaconfig#basic-settings).
DSC uzantısı varsayılan yapılandırma betiği, aşağıdaki tabloda listelenen LCM özellikleri yapılandırmak için kullanabilirsiniz.

| Özellik adı | Tür | Açıklama |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Gerekli özellik. Bir düğüm için Azure Otomasyon hizmeti ile bir PowerShell kimlik bilgisi nesnesi bir parola olarak kaydetmek için kullanılan anahtarını belirtir. Bu değeri kullanarak otomatik olarak bulunabileceğini **listkeys'i** Otomasyon hesabına karşı yöntemi. Değer korumalı bir ayarı olarak güvenli hale getirilmelidir. |
| settings.configurationArguments.RegistrationUrl |dize |Gerekli özellik. Düğümü kaydetmek için girişimde bulunduğu Otomasyon uç noktası URL'sini belirtir. Bu değeri kullanarak otomatik olarak bulunabileceğini **başvuru** Otomasyon hesabına karşı yöntemi. |
| settings.configurationArguments.NodeConfigurationName |dize |Gerekli özellik. Düğüm yapılandırması düğüme atamak için Otomasyon hesabı belirtir. |
| settings.configurationArguments.ConfigurationMode |dize |LCM için modu belirtir. Geçerli seçenekler şunlardır **ApplyOnly**, **ApplyandMonitor**, ve **ApplyandAutoCorrect**.  Varsayılan değer **ApplyandMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Güncelleştirmeler için Otomasyon hesabı ile denetlemek LCM'ne sıklıkta denediğini belirler.  Varsayılan değer **30**.  Minimum değer **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Ne sıklıkta LCM geçerli yapılandırmasını doğrular belirtir. Varsayılan değer **15**. Minimum değer **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | boole | DSC işlemi isterse bir düğümü otomatik olarak yeniden başlatılabilir olup olmadığını belirtir. Varsayılan değer **false**. |
| settings.configurationArguments.ActionAfterReboot | dize | Bir yeniden başlatmadan sonra bir yapılandırma uygulanırken olacağını belirtir. Geçerli seçenekler **ContinueConfiguration** ve **StopConfiguration**. Varsayılan değer **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | boole | LCM düğümünde mevcut modüllerini yazıp yazmayacağını belirtir. Varsayılan değer **false**. |

## <a name="settings-vs-protectedsettings"></a>Ayarları vs. ProtectedSettings

Tüm ayarlar, sanal makine ayarlarını metin dosyasına kaydedilir.
Altında listelenen özellikleri **ayarları** ortak özellikleri.
Genel Özellikler ayarlarını metin dosyasına şifreli değildir.
Altında listelenen özellikleri **protectedSettings** bir sertifika ile şifrelenir ve sanal makine ayarları dosyasına düz metin olarak gösterilmez.

Yapılandırma, kimlik bilgileri gerekiyorsa, kimlik bilgileri içerebilir **protectedSettings**:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example-configuration-script"></a>Örnek yapılandırma betiği

Aşağıdaki örnek, LCM için meta veri ayarları sağlayın ve Automation DSC hizmetiyle kaydetmek için DSC uzantısı için varsayılan davranış gösterir.
Yapılandırma bağımsız değişkenleri gereklidir.
Yapılandırma değişkenleri LCM meta verileri ayarlamak için varsayılan yapılandırma betiğini geçirilir.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl": "[parameters('registrationUrl1')]",
        "NodeConfigurationName": "nodeConfigurationNameValue1"
    }
},
"protectedSettings": {
    "Items": {
        "registrationKeyPrivate": "[parameters('registrationKey1')]"
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Yapılandırma betiği Azure depolama kullanan örnek

Aşağıdaki örnek dandır [DSC uzantısı işleyicisine genel bakış](dsc-overview.md).
Bu örnek, uzantıyı dağıtmak için cmdlet'leri yerine Resource Manager şablonları kullanır.
Iısınstall.ps1 yapılandırmayı kaydedin, .zip dosyasındaki yerleştirin ve ardından erişilebilir bir URL dosyayı karşıya yükleyin.
Bu örnek, Azure Blob Depolama kullanır, ancak herhangi bir rastgele konumdan .zip dosyalarını indirebilirsiniz.

Resource Manager şablonunda VM doğru dosyayı indirin ve ardından uygun PowerShell işlevi çalıştırmak için aşağıdaki kodu bildirir:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="update-from-a-previous-format"></a>Önceki bir biçimden güncelleştir

Herhangi bir ayarı uzantısı'nın önceki bir biçimde (ve ortak özelliklerine sahip **ModulesUrl**, **ConfigurationFunction**, **SasToken**, veya  **Özellikleri**) uzantısının geçerli biçime otomatik olarak uyum sağlar.
Yalnızca bunlar önce yaptığınız gibi bunlar çalıştırın.

Aşağıdaki şema gibi görünüyordu hangi önceki ayarları şeması gösterir:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Önceki biçimden geçerli biçime nasıl uyum sağlayan şu şekildedir:

| Özellik adı | Önceki şema eşdeğer |
| --- | --- |
| settings.wmfVersion |Ayarlar. WMFVersion |
| Settings.Configuration.URL |Ayarlar. ModulesUrl |
| Settings.Configuration.Script |Ayarlarının ilk bölümü. ConfigurationFunction (önce \\ \\) |
| Settings.Configuration.Function |Ayarları ikinci bölümü. ConfigurationFunction (sonra \\ \\) |
| settings.configurationArguments |Ayarlar. Özellikleri |
| settings.configurationData.url |protectedSettings.DataBlobUri (olmadan bir SAS belirteci) |
| settings.privacy.dataEnabled |Ayarlar. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |Ayarlar. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |Ayarlar. SasToken |
| protectedSettings.configurationDataUrlSasToken |SAS belirteci protectedSettings.DataBlobUri gelen |

## <a name="troubleshooting---error-code-1100"></a>Sorun giderme - hata kodu 1100

Hata kodu 1100 DSC uzantısı kullanıcı girişi ile ilgili bir sorun gösterir.
Bu hataların metin değişir ve değişebilir.
Karşılaşabileceğiniz hatalar bazıları aşağıda verilmiştir ve bunları nasıl düzeltebilirsiniz.

### <a name="invalid-values"></a>Geçersiz değerler

"Privacy.dataCollection olan '{0}'.
Yalnızca olası değerler şunlardır: '' 'Enable' ve 'Disable' ".
"WmfVersion olan '{0}'.
Yalnızca olası değerler şunlardır:... ' Son ' ".

**Sorun**: sağlanan değer izin verilmiyor.

**Çözüm**: geçersiz bir değer geçerli bir değerle değiştirin.
Daha fazla bilgi için bkz: tablodaki [ayrıntıları](#details).

### <a name="invalid-url"></a>Geçersiz URL

"ConfigurationData.url olan '{0}'. Bu geçerli bir URL değil"" DataBlobUri olan '{0}'. Bu geçerli bir URL değil"" Configuration.url olan '{0}'. Bu geçerli bir URL değil"

**Sorun**: A sağlanan URL geçerli değil.

**Çözüm**: sağlanan tüm URL'leri denetleyin.
Tüm URL'leri uzantısı uzak makinede erişebilirsiniz geçerli konumlara çözmek emin olun.

### <a name="invalid-configurationargument-type"></a>ConfigurationArgument türü geçersiz

"Geçersiz configurationArguments türü {0}"

**Sorun**: *ConfigurationArguments* özelliği çözümleyemiyor bir **Hashtable** nesne.

**Çözüm**: olun, *ConfigurationArguments* özelliği bir **Hashtable**.
Önceki örnekte sağlanan biçim izleyin. Tırnak işareti, virgül ve küme ayraçları izleyin.

### <a name="duplicate-configurationarguments"></a>Yinelenen ConfigurationArguments

"Yinelenen bağımsız bulunan{0}' hem genel hem de korumalı configurationArguments içinde"

**Sorun**: *ConfigurationArguments* genel ayarları ve *ConfigurationArguments* korumalı ayarlarında özellikler ile aynı ada sahip.

**Çözüm**: Yinelenen özellikler birini kaldırın.

### <a name="missing-properties"></a>Eksik özellikleri

"Configuration.function configuration.url veya configuration.module belirtilmesini gerektirir"

"Bu configuration.script belirtilen Configuration.url gerektirir"

"Bu configuration.url belirtilen Configuration.script gerektirir"

"Bu configuration.function belirtilen Configuration.url gerektirir"

"Bu configuration.url belirtilen ConfigurationUrlSasToken gerektirir"

"Bu configurationData.url belirtilen ConfigurationDataUrlSasToken gerektirir"

**Sorun**: bir tanımlanan özellik eksik başka bir özellik olması gerekiyor.

**Çözümleri**:

- Eksik özellik sağlar.
- Eksik özellik olması gerekiyor özelliğini kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [sanal makine ölçek kümeleri ile Azure DSC uzantı](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
- Hakkında daha fazla ayrıntı bulmak [DSC'ın güvenli kimlik bilgileri yönetimi](dsc-credentials.md).
- Alma bir [giriş Azure DSC uzantısı işleyicisine](dsc-overview.md).
- PowerShell DSC hakkında daha fazla bilgi için Git [PowerShell Belge Merkezi](/powershell/dsc/overview).
