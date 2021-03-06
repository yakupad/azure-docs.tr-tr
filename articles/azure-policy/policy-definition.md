---
title: Azure İlkesi tanım yapısı
description: Kaynak ilke tanımı hangi etkili olması için zaman ilkelerin hiçbiri uygulanmaz ve açıklayarak, kuruluşunuzdaki kaynaklar için kuralları oluşturmak için Azure İlkesi tarafından nasıl kullanıldığını açıklar.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 7f01464c4b9063f20a83c3626d7f92a5e0524f7a
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989134"
---
# <a name="azure-policy-definition-structure"></a>Azure İlkesi tanım yapısı

Azure İlkesi tarafından kullanılan kaynak ilke tanımı hangi etkili olması için zaman ilkelerin hiçbiri uygulanmaz ve açıklayarak, kuruluşunuzdaki kaynaklar için kuralları kurmanızı sağlar. Kuralları tanımlayarak daha kolayca kaynaklarınızı yönetmek ve maliyetleri denetleyebilirsiniz. Örneğin, yalnızca belirli türlerdeki sanal makinelere izin verildiğini belirtebilirsiniz. Veya tüm kaynakların belirli bir etikete sahip gerektirebilir. İlkeleri, tüm alt kaynaklar tarafından devralınır. Bu nedenle, bir kaynak grubu için bir ilke uygulanırsa, bu kaynak grubundaki tüm kaynaklar için geçerlidir.

Azure İlkesi tarafından kullanılan şema burada bulunabilir: [https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json)

Bir ilke tanımı oluşturmak için JSON kullanın. İlke tanımı yönelik öğeleri içerir:

- mode
- parametreler
- Görünen ad
- açıklama
- ilke kuralı
  - mantıksal değerlendirme
  - Etkin

Örneğin, aşağıdaki JSON kaynakların dağıtıldığı sınırlayan ilke gösterilmektedir:

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

Tüm Azure ilkesi örnekleri altındadır [ilkesi örnekleri](json-samples.md).

## <a name="mode"></a>Mod

**Modu** hangi kaynak türlerinin bir ilke değerlendirileceğini belirler. İlişkin desteklenen modları şunlardır:

- `all`: kaynak grupları ve tüm kaynak türleri değerlendirin
- `indexed`: etiketlerini ve konumunu destekleyen kaynak türleri yalnızca değerlendirme

Ayarlamanızı öneririz **modu** için `all` çoğu durumda. Portalı kullanarak oluşturulan tüm ilke tanımlarını `all` modu. PowerShell veya Azure CLI kullanıyorsanız, belirtebilmeniz için **modu** parametresi el ile. İlke tanımı içermiyorsa bir **modu** varsayılan değer `all` Azure PowerShell ve çok `null` Azure CLI'da olduğu eşdeğer `indexed`, için geriye dönük uyumluluk.

`indexed` ilkeleri oluşturma etiketleri veya konumları zorlar olduğunda kullanılmalıdır. Bu gerekli değildir, ancak etiketleri ve konumları olarak uyumluluk sonuçları uyumlu olmayan gösteren gelen desteklemeyen kaynakları engeller. Bir özel durum **kaynak grupları**. Konum veya bir kaynak grubu etiketleri uygulamak çalıştığınız ilkeleri ayarlamalıdır **modu** için `all` ve özellikle hedef `Microsoft.Resources/subscriptions/resourceGroup` türü. Bir örnek için bkz. [kaynak grubu etiketleri zorunlu](scripts/enforce-tag-rg.md).

## <a name="parameters"></a>Parametreler

Parametreleri, ilke tanımlarının sayısını azaltarak ilke yönetiminizi basitleştirmeye yardımcı olur. Düşünme – form üzerinde alanları gibi parametrelerin `name`, `address`, `city`, `state`. Bu parametreleri her zaman aynı kalır, tek tek formu dolduran üzerinde tabanlı ancak bunların değerlerini değiştirin. Parametreler, ilkeleri oluştururken aynı şekilde çalışır. İlke tanımında parametreler ekleyerek, farklı değerler kullanarak farklı senaryolar için bu ilkeyi yeniden kullanabilirsiniz.

Örneğin, kaynakların dağıtıldığı konumları sınırlamak bir kaynak özelliği için bir ilke tanımlayabilirsiniz. Bu durumda, ilkenizi oluşturduğunuzda aşağıdaki parametreleri bildirmek:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        }
    }
}
```

Bir parametrenin türü dize veya dizi olabilir. Meta veri özelliği, Azure portalı gibi araçları için kullanıcı dostu bilgileri görüntülemek için kullanılır.

Meta veri özelliği içinde kullanabileceğiniz **strongType** çoklu seçim listesini Azure portalındaki seçenekleri sağlamak için.  İzin verilen değerler için **strongType** şu anda içerir:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

İlke kuralında aşağıdaki söz dizimini parametrelerle başvuru:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Tanım konumu

Bir girişim veya tanımını oluşturulurken tanım konumunu belirtmek önemlidir.

Tanım konumu için girişim veya tanımını atanabilir kapsamı belirler. Konum, bir yönetim grubu veya abonelik belirtilebilir.

> [!NOTE]
> Bu ilke tanımı için birden çok abonelik uygulamayı planlıyorsanız, konumu için girişim veya atar abonelikleri içeren yönetim grubu olması gerekir.

## <a name="display-name-and-description"></a>Görünen ad ve açıklama

Kullanabileceğiniz **displayName** ve **açıklama** ilke tanımını ve kullanıldığında için bağlam sağlar.

## <a name="policy-rule"></a>İlke kuralı

İlke kuralı oluşan **varsa** ve **ardından** engeller. İçinde **varsa** bloğu ne zaman ilkelerin hiçbiri uygulanmaz belirten bir veya daha fazla koşulları tanımlayın. Senaryo ilkesi için tam olarak tanımlamak için bu koşullar için mantıksal işleçler uygulayabilirsiniz.

İçinde **ardından** bloğu gerçekleşen etkisi tanımladığınız zaman **varsa** koşullar yerine.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
    }
}
```

### <a name="logical-operators"></a>Mantıksal işleçler

Desteklenen mantıksal işleçler şunlardır:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

**Değil** söz dizimi koşul sonucunu tersine çevirir. **Tümü** sözdizimi (benzer şekilde mantıksal **ve** işlemi) tüm koşulların true olmasını gerektirir. **Herhangi** sözdizimi (benzer şekilde mantıksal **veya** işlemi) bir veya daha fazla koşul true olmasını gerektirir.

Mantıksal işleçler iç içe yerleştirebilirsiniz. Aşağıdaki örnekte gösterildiği bir **değil** içinde iç içe işlem bir **tümü** işlemi.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Koşullar

Bir koşulu değerlendirir olup olmadığını bir **alan** belirli kriterlere uyan. Desteklenen koşullar şunlardır:

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"notMatch": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"exists": "bool"`

Kullanırken **gibi** ve **notLike** koşullar, joker karakter sağlayabilir `*` değerindeki değişikliği belirtir.
Değer, birden fazla joker karakter içermemelidir `*`.

Kullanırken **eşleşen** ve **notMatch** koşullar sağlamak `#` bir basamak gösterecek `?` harf ve gerçek o karakteri temsil etmesi için başka bir karakter. Örnekler için bkz [birden çok adı desenlerinin izin](scripts/allow-multiple-name-patterns.md).

### <a name="fields"></a>Alanlar

Koşullara alanları kullanılarak oluşturulur. Bir alan kaynak durumunu tanımlamak için kullanılan kaynak isteği yükü özelliklerinde temsil eder.  

Aşağıdaki alanları desteklenir:

- `name`
- `fullName`
  - Kaynağın tam adını döndürür. Bir kaynağın tam adını, tüm üst kaynak adları (örneğin "myServer/Veritabanım") tarafından başına kaynak addır.
- `kind`
- `type`
- `location`
- `tags`
- `tags.<tagName>`
  - Burada **\<tagName\>** doğrulamak için bir koşul için etiket adıdır.
  - Örnek: `tags.CostCenter` burada **CostCenter** etiketin adı.
- `tags[<tagName>]`
  - Tane nokta içeren etiket adları bu parantez sözdizimini destekler.
  - Burada **\<tagName\>** doğrulamak için bir koşul için etiket adıdır.
  - Örnek: `tags.[Acct.CostCenter]` burada **Acct.CostCenter** etiketin adı.
- özellik diğer adları - bir listesi için bkz [diğer adlar](#aliases).

### <a name="alternative-accessors"></a>Alternatif erişimcileri

**Alan** olan ilke kurallarında kullanılan birincil erişimcisi. Doğrudan değerlendirilen kaynak olup olmadığını denetler. Ancak, başka bir erişimci İlkesi destekler **kaynak**.

```json
"source": "action",
"equals": "Microsoft.Compute/virtualMachines/write"
```

**Kaynak** yalnızca bir değer destekler **eylem**. Eylem değerlendirilen isteğin yetkilendirme eylem döndürür. Yetkilendirme eylemleri yetkilendirme bölümünde gösterilen [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Arka planda mevcut kaynak İlkesi değerlendirirken, bu ayarlar **eylem** için bir `/write` kaynak türünün yetkilendirme eylem.

### <a name="effect"></a>Etki

İlke etkisi aşağıdaki türlerini destekler:

- **Reddetme**: Denetim günlüğüne bir olay oluşturur ve istek başarısız olur
- **Denetim**: denetim günlüğünde uyarı olayı oluşturur, ancak istek başarısız olmaz
- **Append**: dizi alanları isteği ekler
- **AuditIfNotExists**: bir kaynak mevcut değilse denetim sağlar
- **Deployıfnotexists**: zaten yoksa, bir kaynak dağıtır. Şu anda bu etkiyi yalnızca yerleşik ilkeler aracılığıyla desteklenir.

İçin **ekleme**, aşağıdaki ayrıntıları sağlamanız gerekir:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

Değer bir dize veya bir JSON biçimi nesnesi olabilir.

İle **AuditIfNotExists** ve **Deployıfnotexists** ilişkili bir kaynak varlığı değerlendirin ve bu kaynak mevcut değil, bir kural ve karşılık gelen bir efekt uygulayın. Örneğin, tüm sanal ağları için Ağ İzleyicisi dağıtıldığını gerektirebilir.
Sanal makine uzantısı olmayan dağıtıldığında denetim örneği için bkz: [uzantı mevcut değilse denetim](scripts/audit-ext-not-exist.md).

Değerlendirme, özellikler ve örnekler de sırasını her etkisi hakkında tüm ayrıntılar için bkz. [anlama ilke etkileri](policy-effects.md).

## <a name="aliases"></a>Diğer adlar

Bir kaynak türü için belirli özelliklerine erişmek için özelliği diğer adları kullanın. Diğer adlar, kaynak üzerinde bir özellik için hangi değerleri veya koşullara izin sınırlamak sağlar. Belirtilen kaynak türü için farklı bir API sürümlerinde yolları her diğer adın eşlenir. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

Diğer adlar listesini her zaman artmaktadır. Hangi diğer adlar şu anda Azure İlkesi tarafından desteklenen bulmak için aşağıdaki yöntemlerden birini kullanın:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  $azContext = Get-AzureRmContext
  $azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
  $profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
  $token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
  $authHeader = @{
      'Content-Type'='application/json'
      'Authorization'='Bearer ' + $token.AccessToken
  }

  # Invoke the REST API
  $response = Invoke-RestMethod -Uri 'https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases' -Method Get -Headers $authHeader

  # Create an Array List to hold discovered aliases
  $aliases = New-Object System.Collections.ArrayList

  foreach ($ns in $response.value) {
      foreach ($rT in $ns.resourceTypes) {
          if ($rT.aliases) {
              foreach ($obj in $rT.aliases) {
                  $alias = [PSCustomObject]@{
                      Namespace       = $ns.namespace
                      resourceType    = $rT.resourceType
                      alias           = $obj.name
                  }
                  $aliases.Add($alias) | Out-Null
              }
          }
      }
  }

  # Output the list, sort, and format. You can customize with Where-Object to limit as desired.
  $aliases | Sort-Object -Property Namespace, resourceType, alias | Format-Table
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # Get Azure Policy aliases for a specific Namespace
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

## <a name="initiatives"></a>Girişimler

Girişim atamaları ve yönetim grubu tek bir öğe olarak çalışmak için basitleştirmek için çeşitli ilgili ilke tanımlarını olanak sağlar. Örneğin, tek bir girişim etiketleme ilgili tüm ilke tanımlarını gruplandırabilirsiniz. Tek tek her ilke atamak yerine, girişim uygulayın.

Aşağıdaki örnek iki etiketi işlemeye yönelik bir girişim oluşturma işlemini gösterir: `costCenter` ve `productName`. Varsayılan etiket değeri uygulamak için iki yerleşik ilkeleri kullanır.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](json-samples.md) sayfasındaki diğer örnekleri inceleyin.
