---
title: Azure PowerShell kullanarak özel roller oluşturma | Microsoft Docs
description: Azure PowerShell ile rol tabanlı erişim denetimi (RBAC) için özel roller oluşturmayı öğrenin. Bu liste, oluşturma, güncelleştirme ve özel roller silme nasıl içerir.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c8e5f34bb6b38a3f187d86a1ebc0c7019c7f1046
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437027"
---
# <a name="create-custom-roles-using-azure-powershell"></a>Azure PowerShell kullanarak özel roller oluşturma

[Yerleşik roller](built-in-roles.md) kuruluşunuzun ihtiyaçlarını karşılamıyorsa kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure PowerShell kullanarak özel roller oluşturmak ve yönetmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Özel roller oluşturmanızı da ihtiyacınız vardır:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- Yerel ortamda [Azure PowerShell](/powershell/azure/install-azurerm-ps)

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Bir kapsamda atama için uygun rolü listelemek için kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) komutu. Aşağıdaki örnek, seçili Abonelikteki atama için uygun olan tüm rolleri listeler.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

Aşağıdaki örnek yalnızca seçili Abonelikteki atama için uygun olan özel rolleri listeler.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Seçili abonelik içinde değilse `AssignableScopes` rolünü, özel rol listelenmez.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için kullanın [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) komutu. Rol yapılandırma için iki yöntem vardır kullanarak `PSRoleDefinition` nesnesi veya bir JSON şablonu. 

### <a name="get-operations-for-a-resource-provider"></a>Bir kaynak sağlayıcısı işlemleri Al

Özel rol oluşturduğunuzda, olası tüm işlemler kaynak sağlayıcılarından bilmeniz önemlidir.
Listesini görüntüleyebileceğiniz [kaynak sağlayıcısı işlemleri](resource-provider-operations.md) veya [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) bu bilgileri almak için komutu.
Örneğin, sanal makineler için kullanılabilir tüm işlemleri denetlemek istiyorsanız, bu komutu kullanın:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Özel bir rol ile PSRoleDefinition nesnesi oluşturun

Özel bir rol oluşturmak için PowerShell kullanırken, aşağıdakilerden birini kullanabilirsiniz [yerleşik roller](built-in-roles.md) gibi bir başlangıç noktası veya sıfırdan başlayabilirsiniz. Bu bölümdeki ilk örnekte, yerleşik bir rol ile başlar ve ardından daha fazla izinlerle özelleştirir. Öznitelikleri eklemek için Düzenle `Actions`, `NotActions`, veya `AssignableScopes` ve sonra değişiklikleri yeni bir rolü olarak kaydedebilirsiniz.

Aşağıdaki örnek ile başlayan [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) adlı özel bir rol oluşturmak için yerleşik rol *sanal makine işleci*. Yeni rol tüm okuma işlemleri için erişim verir *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* başlatmak için kaynak sağlayıcıları ve izinler erişimi , yeniden başlatın ve sanal makineleri izleyin. Özel rolü iki abonelik kullanılabilir.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

Aşağıdaki örnek oluşturmak için başka bir yolunu gösterir *sanal makine işleci* özel rol. Yeni bir oluşturarak başlar `PSRoleDefinition` nesne. Belirtilen eylem işlemleri `perms` değişken ve ayarlanmış `Actions` özelliği. `NotActions` Özelliği ayarlanmış okuyarak `NotActions` gelen [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) yerleşik rolü. Bu yana [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) yok `NotActions`, bu satırı gerekli değildir, ancak başka bir rolden bilgiler nasıl alınabilir gösterir.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>JSON şablonu ile bir özel rol oluşturma

Bir JSON şablonu kaynak tanımını özel rolü için kullanılabilir. Aşağıdaki örnek, depolama ve işlem kaynaklarına erişimi desteklemek için okuma erişimine izin veren özel bir rol oluşturur ve bu rol için iki abonelik ekler. Yeni bir dosya oluşturun `C:\CustomRoles\customrole1.json` aşağıdaki örnekle. Kimliği ayarlanmalıdır `null` yeni bir kimliği olarak ilk rol oluşturma sırasında otomatik olarak oluşturulur. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Abonelikleri rolü eklemek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Kullanarak mevcut bir özel rol değiştirebilirsiniz benzer şekilde, özel bir rol oluşturmadan, `PSRoleDefinition` nesnesi veya bir JSON şablonu.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Özel bir rol PSRoleDefinition nesnesi ile güncelleştirme

Özel bir rol değiştirmek için önce kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) rol tanımı almak için komutu. İkinci olarak, rol tanımı için istediğiniz değişiklikleri yapın. Son olarak, [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) değiştirilmiş rol tanımı kaydetmek için komutu.

Aşağıdaki örnek ekler `Microsoft.Insights/diagnosticSettings/*` işlemi *sanal makine işleci* özel rol.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

Aşağıdaki örnek, bir Azure aboneliğine atanabilir kapsamlar ekler *sanal makine işleci* özel rol.

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="update-a-custom-role-with-a-json-template"></a>Özel bir rol ile bir JSON şablonunu güncelleştirme

Önceki JSON şablonunu kullanarak, mevcut bir özel rolü ekleme veya kaldırma eylemleri kolayca değiştirebilirsiniz. JSON şablonunu güncelleştirin ve aşağıdaki örnekte gösterildiği gibi ağ okuma eylemi ekleyin. Şablonda listelenen tanımları, şablonda tam olarak belirttiğiniz şekilde rol göründüğünü anlamına gelir, mevcut bir tanımı için üst üste uygulanmaz. Ayrıca, rol kimliği ile kimlik alanı güncelleştirmeniz gerekiyor. Bu değer nedir emin değilseniz, kullanabileceğiniz [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) bu bilgileri almak için cmdlet.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Mevcut rol güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Özel rolü silme

Bir özel rolü silmek için kullanın [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition) komutu.

Aşağıdaki örnek kaldırır *sanal makine işleci* özel rol.

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure PowerShell kullanarak özel bir rol oluşturma](tutorial-custom-role-powershell.md)
- [Azure'da özel roller](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)
