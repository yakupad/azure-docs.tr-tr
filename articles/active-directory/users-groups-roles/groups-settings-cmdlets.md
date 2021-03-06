---
title: PowerShell kullanarak Azure Active Directory'de Grup ayarlarını yapılandırma | Microsoft Docs
description: Azure Active Directory cmdlet'lerini kullanarak Grup ayarlarını yönetme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/13/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 9e065b04083cce958bc42f2efade0038bf137f8a
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145120"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri
Bu makale, grupları oluşturmak için Azure Active Directory (Azure AD) PowerShell cmdlet'lerini kullanmaya yönelik yönergeler içerir. Bu içerik yalnızca (birleştirilmiş grupları denir) Office 365 grupları için geçerlidir. 

> [!IMPORTANT]
> Bazı ayarlar, bir Azure Active Directory Premium P1 lisansı gerekir. Daha fazla bilgi için [şablon ayarlarını](#template-settings) tablo.

Yönetici olmayan kullanıcıların oluşturmasını önlemek nasıl daha fazla bilgi için *güvenlik* grupları, `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` açıklandığı [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Office 365 grupları ayarları, bir ayar nesnesi ve bir SettingsTemplate nesnesi kullanılarak yapılandırılır. Başlangıçta, dizininize varsayılan ayarlarla yapılandırıldığından dizininizde herhangi Ayarları nesnelerini görmüyorum. Varsayılan ayarları değiştirmek için ayarlar şablon kullanarak yeni bir ayar nesnesi oluşturmanız gerekir. Ayarları şablonları, Microsoft tarafından tanımlanır. Birkaç farklı ayarlar şablonu vardır. Dizininiz için Office 365 Grup ayarlarını yapılandırmak için "Group.Unified" adlı şablonu kullanın. Tek bir grup üzerinde Office 365 Grup ayarlarını yapılandırmak için "Group.Unified.Guest" adlı şablonu kullanın. Bu şablon, Office 365 grubu Konuk erişimi yönetmek için kullanılır. 

Cmdlet'ler, Azure Active Directory PowerShell V2 modülü bir parçasıdır. Yönergeler için nasıl indirin ve bilgisayarınıza modülü yüklemek, makaleye göz atın [Azure Active Directory PowerShell sürüm 2](https://docs.microsoft.com/powershell/azuread/). Modülü sürüm 2 sürümünü yükleyebilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>Belirli ayarları değeri alınamıyor
Almak istediğiniz ayar adını biliyorsanız, kullanabileceğiniz aşağıdaki geçerli ayarları değerini almak için cmdlet'i. Bu örnekte, biz "UsageGuidelinesUrl." adlı bir ayarın değerini alma Dizin ayarları ve adları hakkında daha fazla aşağı bu makalede daha fazla bilgi edinebilirsiniz.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a>Dizin düzeyinde ayarları oluşturma
Bu adımları ayarları dizin düzeyinde dizindeki tüm Office 365 grupları için geçerli oluşturun. Yalnızca Get-AzureADDirectorySettingTemplate cmdlet kullanılabilir [graf için Azure AD PowerShell Önizleme modülünün](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

1. DirectorySettings Cmdlet'lerde, kullanmak istediğiniz SettingsTemplate Kimliğini belirtmeniz gerekir. Bu cmdlet, bu kimliği bilmiyorsanız, tüm ayarları şablonları listesini döndürür:
  
  ```
  PS C:> Get-AzureADDirectorySettingTemplate
  ```
  Bu cmdlet çağrısı kullanılabilir olan tüm şablonları döndürür:
  
  ```
  Id                                   DisplayName         Description
  --                                   -----------         -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
  16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
  ```
2. Kullanım Kılavuzu URL'si eklemek için ilk kullanım kılavuzu URL değeri tanımlayan SettingsTemplate nesnesini almak gerekir; diğer bir deyişle, Group.Unified şablonu:
  
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
  ```
3. Ardından, o şablonu temel alan yeni bir ayar nesnesi oluşturun:
  
  ```
  $Setting = $template.CreateDirectorySetting()
  ```  
4. Ardından Kullanım Kılavuzu değeri güncelleştirin:
  
  ```
  $setting["UsageGuidelinesUrl"] = "https://guideline.example.com"

  ```  
5. Son olarak, ayarlarını uygulayın:
  
  ```
  New-AzureADDirectorySetting -DirectorySetting $setting
  ```

Cmdlet, işlemin başarıyla tamamlanmasından sonra yeni ayarları nesnesinin Kimliğini döndürür:
  ```
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```
## <a name="template-settings"></a>Şablon ayarları
Group.Unified SettingsTemplate içinde tanımlanan ayarlar aşağıda verilmiştir. Aksi belirtilmediği sürece, bu özellikler bir Azure Active Directory Premium P1 lisansı gerektirir. 

| **Ayar** | **Açıklama** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Türü: Boole<li>Varsayılan: True |Office 365 grubu oluşturma dizinde yönetici olmayan kullanıcılar tarafından izin verilip verilmeyeceğini belirten bayrak. Bu ayar, bir Azure Active Directory Premium P1 lisansı gerektirmez.|
|  <ul><li>GroupCreationAllowedGroupId<li>Türü: Dize<li>Varsayılan: "" |Kendisi için üyeleri Office 365 grupları oluşturmasına izin güvenlik grubunun GUID bile EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Türü: Dize<li>Varsayılan: "" |Grup kullanım kılavuzları bağlantısı. |
|  <ul><li>ClassificationDescriptions<li>Türü: Dize<li>Varsayılan: "" | Sınıflandırma açıklamaları virgülle ayrılmış listesi. ClassificationDescriptions yalnızca şu biçimde geçerli değeri:
  $setting ["ClassificationDescriptions"] "Sınıflandırma: açıklaması, Sınıflandırması: Description", sınıflandırma ClassificationList dizelerde eşleştiği =.|
|  <ul><li>DefaultClassification<li>Türü: Dize<li>Varsayılan: "" | Hiçbiri belirtilmemişse varsayılan sınıflandırma bir grup için kullanılacak olan sınıflandırması.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Türü: Dize<li>Varsayılan: "" | Office 365 grupları için yapılandırılmış adlandırma kuralı tanımlayan bir en fazla 64 karakter uzunluğunda dize. Daha fazla bilgi için [Office 365 grupları için bir adlandırma ilkesini zorlama](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Türü: Dize<li>Varsayılan: "" | Kullanıcı grubu adı veya diğer adı kullanmak için izin verilmez tümcecikleri virgülle ayrılmış dizesi. Daha fazla bilgi için [Office 365 grupları için bir adlandırma ilkesini zorlama](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Türü: Boole<li>Varsayılan: "False" | Kullanmayın
|  <ul><li>AllowGuestsToBeGroupOwner<li>Türü: Boole<li>Varsayılan: False | Konuk kullanıcı Grup sahibi olabilir olup olmadığını belirten bir Boole değeri. |
|  <ul><li>AllowGuestsToAccessGroups<li>Türü: Boole<li>Varsayılan: True | Konuk kullanıcı erişim için Office 365 grupları içeriğe sahip olup olmadığını belirten bir Boole değeri.  Bu ayar, bir Azure Active Directory Premium P1 lisansı gerektirmez.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Türü: Dize<li>Varsayılan: "" | Konuk kullanım yönergeleri için bir bağlantı URL'si. |
|  <ul><li>AllowToAddGuests<li>Türü: Boole<li>Varsayılan: True | Boole Konukları bu dizine eklemek için kullanılabilir olup olmadığını belirten bir.|
|  <ul><li>ClassificationList<li>Türü: Dize<li>Varsayılan: "" |Office 365 grupları için uygulanabilir geçerli sınıflandırma değerleri virgülle ayrılmış listesi. |

## <a name="read-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlarını okuma
Bu adımlar, dizindeki tüm Office grupları için geçerli olan ayarları dizin düzeyinde okuyun.

1. Tüm mevcut dizin ayarları okuyun:
  ```
  Get-AzureADDirectorySetting -All $True
  ```
  Bu cmdlet, tüm dizin ayarları listesini döndürür:
  ```
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

2. Belirli bir grup için tüm ayarların okuyun:
  ```
  Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
  ```

3. Tüm dizin ayarları değerlerini kimliği GUID ayarlarını kullanarak özel dizin ayarları nesnesinin okuyun:
  ```
  (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
  ```
  Bu cmdlet, bu belirli bir grup için bu ayarları nesnesinde adları ve değerleri döndürür:
  ```
  Name                          Value
  ----                          -----
  ClassificationDescriptions
  DefaultClassification
  PrefixSuffixNamingRequirement
  CustomBlockedWordsList        
  AllowGuestsToBeGroupOwner     False 
  AllowGuestsToAccessGroups     True
  GuestUsageGuidelinesUrl
  GroupCreationAllowedGroupId
  AllowToAddGuests              True
  UsageGuidelinesUrl            https://guideline.example.com
  ClassificationList
  EnableGroupCreation           True
  ```

## <a name="update-settings-for-a-specific-group"></a>Belirli bir grup için ayarları güncelleştir

1. "Groups.Unified.Guest" adlı ayarları şablonunu Ara
  ```
  Get-AzureADDirectorySettingTemplate
  
  Id                                   DisplayName            Description
  --                                   -----------            -----------
  62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
  08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
  4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
  898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
  5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
  ```
2. Groups.Unified.Guest şablon için şablon nesnesi Al:
  ```
  $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
  ```
3. Şablondan Yeni bir ayar nesnesi oluşturun:
  ```
  $Setting = $Template.CreateDirectorySetting()
  ```

4. Ayarını, gerekli değeri ayarlayın:
  ```
  $Setting["AllowToAddGuests"]=$False
  ```
5. Gerekli grubu için yeni ayar dizinde oluşturun:
  ```
  New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
  ```

## <a name="update-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlarını güncelleştirme

Bu adımlar, dizindeki tüm Office 365 grupları için geçerli olan ayarları dizin düzeyinde güncelleştirin. Bu örnekler, dizininizde zaten bir ayar nesnesi var. varsayar.

1. Var olan ayarları nesnesini Bul:
  ```
  Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ
  
  Id                                   DisplayName   TemplateId                           Values
  --                                   -----------   ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  
  $setting = Get-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323
  ```
2. Değeri güncelleştirin:
  
  ```
  $Setting["AllowToAddGuests"] = "false"
  ```
3. Güncelleştirme ayarı:
  
  ```
  Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
  ```

## <a name="remove-settings-at-the-directory-level"></a>Dizin düzeyinde ayarlarını Kaldır
Bu adım, dizindeki tüm Office grupları için geçerli olan ayarları dizin düzeyinde kaldırır.
  ```
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Cmdlet'in söz dizimi başvurusu
Daha fazla Azure Active Directory PowerShell belgeleri bulabilirsiniz [Azure Active Directory cmdlet'leri](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Ek okuma

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md)
