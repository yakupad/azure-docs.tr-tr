---
title: Azure Resource Manager şablon yapısını ve söz dizimi | Microsoft Docs
description: Yapısını ve bildirim temelli JSON söz dizimini kullanarak Azure Resource Manager şablonları özelliklerini açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2018
ms.author: tomfitz
ms.openlocfilehash: e1964b7f46259e54c65aeb46aa795713922c3504
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114621"
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonları, söz dizimi ve yapısı anlama
Bu makalede, Azure Resource Manager şablon yapısını açıklar. Bu, bir şablon ve bu bölümlerdeki kullanılabilir olan özellikleri farklı bölümlerini sayısını gösterir. Şablonda, JSON ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler bulunur. Şablon oluşturmanın adım adım öğretici için bkz: [ilk Azure Resource Manager şablonunuzu oluşturma](resource-manager-create-first-template.md).

## <a name="template-format"></a>Şablon biçimi
En basit yapısına bir şablon aşağıdaki öğelere sahiptir:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| $schema |Evet |Şablon dil sürümünü tanımlayan JSON şema dosyasının konumu. Önceki örnekte gösterilen URL'yi kullanın. |
| contentVersion |Evet |Şablon (örneğin, 1.0.0.0) sürümü. Bu öğe için herhangi bir değer sağlayabilirsiniz. Şablonunuzda önemli değişiklikleri belgelemek için bu değeri kullanın. Şablon kullanarak kaynakları dağıtırken, bu değer, en uygun şablonu kullanıldığından emin emin olmak için kullanılabilir. |
| parametreler |Hayır |Kaynak bir dağıtımı özelleştirmek için dağıtım çalıştırıldığında, sağlanan değerler. |
| Değişkenleri |Hayır |Şablonda, JSON parçaları olarak şablon dili ifadeleri basitleştirmek için kullanılan değerleri. |
| işlevler |Hayır |Şablonda kullanılabilir olan kullanıcı tanımlı işlevler. |
| kaynaklar |Evet |Dağıtılan ya da bir kaynak grubunda güncelleştirilmiş kaynak türleri. |
| çıkışlar |Hayır |Dağıtımdan sonra döndürülen değerleri. |

Her öğesinin özellikleri ayarlayabilirsiniz. Aşağıdaki örnek, bir şablon için tam sözdizimini gösterir:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-the parameter>" 
            }
        }
    },
    "variables": {
        "<variable-name>": "<variable-value>",
        "<variable-object-name>": {
            <variable-complex-type-value>
        },
        "<variable-object-name>": {
            "copy": [
                {
                    "name": "<name-of-array-property>",
                    "count": <number-of-iterations>,
                    "input": {
                        <properties-to-repeat>
                    }
                }
            ]
        },
        "copy": [
            {
                "name": "<variable-array-name>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "functions": [
      {
        "namespace": "<namespace-for-your-function>",
        "members": {
          "<function-name>": {
            "parameters": [
              {
                "name": "<parameter-name>",
                "type": "<type-of-parameter-value>"
              }
            ],
            "output": {
              "type": "<type-of-output-value>",
              "value": "<function-expression>"
            }
          }
        }
      }
    ],
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Bu makalede daha ayrıntılı şablon bölümlerini açıklar.

## <a name="syntax"></a>Sözdizimi
Temel söz dizimi şablonu JSON ' dir. Ancak, ifadeler ve İşlevler, şablonda kullanılabilir JSON değerlerinin genişletin.  İfadeleri JSON dize değişmez değerleri içinde ayarlanmış ilk yazılır ve son karakter olan ayraçlar: `[` ve `]`sırasıyla. Şablon dağıtıldığında ifade değeri değerlendirilir. Dize sabit değeri olarak yazılmış olsa da ifade değerlendirme sonucu gibi bir dizi ya da tamsayı, gerçek ifade bağlı olarak farklı bir JSON türünde olabilir.  Bir sabit dize köşeli ayraç ile başlatmak için `[`, ancak değil bir ifade olarak yorumlanır olması, dizesiyle başlatmak için ek bir köşeli ayraç Ekle `[[`.

Genellikle, ifadeleri işlevleri ile dağıtım yapılandırma işlemleri gerçekleştirmek için kullanırsınız. JavaScript'te, işlev çağrıları olarak biçimlendirilmiş gibi yalnızca `functionName(arg1,arg2,arg3)`. Özellikler, nokta ve [dizin] işleçleri kullanarak başvuru.

Aşağıdaki örnek, bir değer oluştururken çeşitli işlevler kullanma işlemini gösterir:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Şablon işlevlerinin tam listesi için bkz. [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametreler
Şablon parametreleri bölümünde kaynakları dağıtırken giriş değerleri belirtin. Bu parametre değerleri (örneğin, geliştirme, test ve üretim) belirli bir ortam için uygun değerleri sağlayarak bir dağıtımı özelleştirmek etkinleştirin. Şablonunuzdaki parametrelerle sağlamanıza gerek yoktur, ancak parametre olmadan, şablonunuzu her zaman aynı adları, konumları ve özellikleri ile aynı kaynakları dağıtmak için kullanacağınız.

Aşağıdaki örnek, bir basit parametre tanımı gösterilmektedir:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Parametreleri tanımlama hakkında daha fazla bilgi için bkz: [parametreleri bölümünde Azure Resource Manager şablonları](resource-manager-templates-parameters.md).

## <a name="variables"></a>Değişkenler
Değişkenler bölümünde kullanılabilir değerler, şablonun tamamında oluşturun. Değişkenleri tanımlamanız gerekmez, ancak bunlar karmaşık ifadeleri azaltarak genellikle şablonunuzu basitleştirin.

Aşağıdaki örnek, basit bir değişken tanımı gösterilmektedir:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Değişkenleri tanımlama hakkında daha fazla bilgi için bkz: [değişkenler bölümü Azure Resource Manager şablonlarının](resource-manager-templates-variables.md).

## <a name="functions"></a>İşlevler

Şablonunuzun içindeki işlevleri kendi oluşturabilirsiniz. Bu işlevler şablonunuzdaki kullanılabilir. Genellikle, şablonunuzu yinelemek için istemediğiniz karmaşık bir ifadeyi tanımlar. Gelen ifadeleri kullanıcı tanımlı işlevler oluşturma ve [işlevleri](resource-group-template-functions.md) şablonlarında desteklenir.

Bir kullanıcı işlevi tanımlanırken, bazı kısıtlamalar vardır:

* İşlev değişkenleri erişemez.
* İşlev şablonu parametreleri erişemez. Diğer bir deyişle, [parametreleri işlevi](resource-group-template-functions-deployment.md#parameters) işlev parametrelerini sınırlıdır.
* İşlev diğer kullanıcı tanımlı işlevleri çağrılamaz.
* İşlev kullanamazsınız [başvuru işlevi](resource-group-template-functions-resource.md#reference).
* İşlev parametrelerini varsayılan değerlere sahip olamaz.

İşlevlerinizi adlandırma şablon işlevleri ile çakışmalarını önlemek için bir ad alanı değeri gerektirir. Aşağıdaki örnek, bir depolama hesabı adı döndüren bir işlev gösterir:

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

İşlevini kullanarak çağırırsınız:

```json
"resources": [
  {
    "name": "[contoso.uniqueName(parameters('storageNamePrefix'))]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "location": "South Central US",
    "tags": {},
    "properties": {}
  }
]
```

## <a name="resources"></a>Kaynaklar
Kaynaklar bölümünde, dağıtılan veya güncelleştirilen kaynakları tanımlayın. Bu bölümde, doğru değerleri sağlamak üzere dağıtıyorsanız türlerini anlamanız gerekir çünkü karmaşık alabilirsiniz.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Daha fazla bilgi için [Azure Resource Manager şablonları, kaynaklar bölümü](resource-manager-templates-resources.md).

## <a name="outputs"></a>Çıkışlar
Çıkış bölümünde dağıtımdan döndürülen değerlerini belirtin. Örneğin, dağıtılmış bir kaynağa erişmek için URI döndürebilir.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Daha fazla bilgi için [çıktısını alır, Azure Resource Manager şablonları bölümünü](resource-manager-templates-outputs.md).

## <a name="template-limits"></a>Şablon sınırları

Şablonunuza 1 MB ve her 64 KB parametre dosyası boyutunu sınırlama. Yinelemeli Kaynak tanımları ve değişkenler ve parametreler için değerleri genişletildi sonra şablonunun son duruma 1 MB'lık sınırı uygular. 

İçin sınırlama getirilir:

* 256 parametreleri
* 256 değişkenleri
* 800 kaynakları (kopya sayısı da dahil olmak üzere)
* 64 çıkış değerleri
* bir şablon ifadesi 24.576 karakter

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırlar aşabilir. Daha fazla bilgi için [Azure kaynakları dağıtılırken bağlı şablonları kullanma](resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkış sayısını azaltmak için değerlerden bir nesnesi olarak birleştirebilirsiniz. Daha fazla bilgi için [parametre olarak nesnelerin](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Birden fazla şablon dağıtımı sırasında birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Küresel Azure, Azure bağımsız bulutlarda ve Azure Stack arasında kullanabileceğiniz Resource Manager şablonları oluşturmaya ilişkin öneriler için bkz. [bulut tutarlılık için geliştirme Azure Resource Manager şablonları](templates-cloud-consistency.md).
