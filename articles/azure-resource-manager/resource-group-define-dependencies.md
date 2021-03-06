---
title: Azure kaynakları için dağıtım sırasını Ayarla | Microsoft Docs
description: Bir kaynak başka bir kaynağa bağlı olarak doğru sırayla dağıtılan kaynakları sağlamak için dağıtım sırasında nasıl ayarlanacağını açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: ''
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2018
ms.author: tomfitz
ms.openlocfilehash: 475e1f0d481678f53c191a887c7cc56c28c4b361
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887438"
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarında kaynak dağıtmaya sırasını tanımlayın
Belirli bir kaynak için kaynak dağıtılmadan önce mevcut olmalıdır diğer kaynakları olabilir. Örneğin, bir SQL server, SQL veritabanı dağıtmaya çalışmadan önce mevcut olması gerekir. Bu ilişki, diğer kaynağına bağlı olarak bir kaynak olarak işaretleyerek tanımlayın. Bir bağımlılık ile tanımladığınız **dependsOn** öğesini kullanarak veya **başvuru** işlevi. 

Resource Manager, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları bağımlı sıralamalarını dağıtır. Resource Manager kaynaklarını birbirine bağımlı olmayan, paralel olarak bunları dağıtır. Yalnızca aynı şablonda dağıtılan kaynaklar için bağımlıkları tanımlama gerekir. 

## <a name="dependson"></a>dependsOn
Şablonunuzun içindeki dependsOn öğesi bir kaynak olarak bir veya daha fazla kaynak bağlıdır tanımlamanızı sağlar. Değerini kaynak adlarının virgülle ayrılmış bir listesini olabilir. 

Aşağıdaki örnek, bir yük dengeleyici, sanal ağ ve birden çok depolama hesabında oluşturan bir döngü olduğu bir sanal makine ölçek kümesi gösterir. Aşağıdaki örnekte bu diğer kaynaklara gösterilmez, ancak başka bir yerde şablonda mevcut gerekir.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

Önceki örnekte adlı bir kopya döngü oluşturulan kaynakların bir bağımlılık içindedir **storageLoop**. Bir örnek için bkz. [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).

Bağımlılıkları tanımlarken, kaynak sağlayıcısı ad alanı ve belirsizlik önlemek için kaynak türü içerebilir. Örneğin, bir yük dengeleyici ve diğer kaynakların aynı adları olabilir, sanal ağ'ı açıklamak için aşağıdaki biçimi kullanın:

```json
"dependsOn": [
  "[resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName'))]",
  "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]"
]
``` 

DependsOn kaynaklarınız arasındaki ilişkileri eşleştirmek için kullanılacak yazmaya, ancak neden yaptığınızı anlamak önemlidir. Örneğin, kaynakları nasıl bağlandığına belgeye dependsOn aldığı doğru yaklaşım değildir. Dağıtımdan sonra hangi kaynakların dependsOn öğesinde tanımlanan sorgulayamaz. Bağımlılığı olan paralel iki kaynakları Resource Manager dağıtmaz çünkü dependsOn kullanarak, büyük olasılıkla dağıtım süresini etkiler. 

## <a name="child-resources"></a>Alt kaynakları
Kaynaklar özellik tanımlanan kaynakla ilgili alt kaynakları belirtmenizi sağlar. Alt kaynakları yalnızca tanımlanmış beş düzey derinliğinde olabilir. Örtük bir bağımlılık üst kaynak alt kaynak arasında oluşturulan değil dikkat edin önemlidir. Alt kaynak sonra üst kaynak dağıtılması için gerekiyorsa, bu bağımlılık dependsOn özelliği ile açıkça belirtmelidir. 

Her bir üst kaynak alt kaynakları yalnızca belirli kaynak türleri kabul eder. Kabul edilen kaynak türleri belirtilen [şablon şeması](https://github.com/Azure/azure-resource-manager-schemas) üst kaynak. Alt kaynak türünün adı gibi üst kaynak türü adı içerir **Microsoft.Web/sites/config** ve **Microsoft.Web/sites/extensions** herikialtkaynaklarıolan**Microsoft.Web/sites**.

Aşağıdaki örnek, bir SQL server ve SQL veritabanı gösterir. Veritabanı sunucusunun bir alt olsa bile SQL veritabanı ve SQL server arasında açık bir bağımlılık tanımlanır dikkat edin.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-and-list-functions"></a>Başvuru ve liste işlevleri
[Başvuru işlevi](resource-group-template-functions-resource.md#reference) bir ifadenin, değerini diğer JSON ad ve değer çiftlerini ya da çalışma zamanı kaynakları alabilmesine olanak sağlar. [Listesi * işlevleri](resource-group-template-functions-resource.md#listkeys-listsecrets-and-list) dönüş değerleri bir kaynak için bir liste işlemi.  Başvuru ve liste ifadeleri örtük olarak bir kaynak başvurulan kaynak aynı şablon dağıtıldığında başka birinde bağlıdır ve adı (kaynak kimliği değil) tarafından başvurulan bildirin. Kaynak Kimliği başvuru veya liste işlevlerini geçirirseniz, örtük bir başvuru oluşturulmaz.

Başvuru işlevin genel biçimi şu şekildedir:

```json
reference('resourceName').propertyPath
```

Listkeys'i işlevin genel biçimi şu şekildedir:

```json
listKeys('resourceName', 'yyyy-mm-dd')
```

Aşağıdaki örnekte, bir CDN uç noktası açıkça CDN profile bağlıdır ve örtük olarak bir web uygulaması bağlıdır.

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

Bağımlılıklarını belirtmek için bu öğe veya öğe dependsOn kullanabilirsiniz, ancak her ikisi için aynı bağımlı kaynak kullanmanız gerekmez. Mümkün olduğunda, gereksiz bir bağımlılık ekleme önlemek için örtük bir başvuru kullanın.

Daha fazla bilgi için bkz. [başvuru işlevi](resource-group-template-functions-resource.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Bağımlılıkları ayarlamak için öneriler

Ayarlamak için hangi bağımlılıkların karar verirken aşağıdaki yönergeleri kullanın:

* Mümkün olduğu kadar az bağımlılıklar ayarlayın.
* Alt kaynak kendi üst kaynağına bağımlı olarak ayarlayın.
* Kullanım **başvuru** işlevi ve bir özellik paylaşmak için gereken kaynaklar arasında örtük bağımlılıkları ayarlamak üzere kaynak adı geçirin. Açık bir bağımlılık ekleme (**dependsOn**) ne zaman önceden tanımladığınız dolaylı bir bağımlılığı. Bu yaklaşım, gereksiz bağımlılıkları yaşama riskini azaltır. 
* Bir kaynak olamaz, bir bağımlılık ayarlayın **oluşturulan** başka bir kaynaktan işlevselliği olmadan. Kaynakları yalnızca dağıtımdan sonra etkileşim, bir bağımlılık ayarlamanız gerekmez.
* Açıkça ayarlamadan basamaklı bağımlılıkları sağlar. Örneğin, sanal makinenize bir sanal ağ arabiriminde bağlıdır ve sanal ağ arabirimi bir sanal ağ ve genel IP adresleri bağlıdır. Bu nedenle, dağıtılan tüm üç kaynak sanal makine olduğu halde tüm üç kaynaklara bağlı olarak sanal makine açıkça ayarlamanız gerekmez. Bu yaklaşım, bağımlılık sırası açıklar ve daha sonra şablonu değiştirmek kolaylaştırır.
* Dağıtımdan önce bir değer belirlenemezse olmayan kaynak dağıtma deneyin. Örneğin, bir yapılandırma değeri başka bir kaynak adı gerekiyorsa, bir bağımlılık gerekmeyebilir. Bu kılavuz, bazı kaynaklar diğer kaynak varlığını doğrulamak için her zaman çalışmaz. Bir hata alırsanız, bağımlılık ekleme. 

Resource Manager şablon doğrulaması sırasında döngüsel bağımlılıklar tanımlar. Döngüsel bağımlılık var olduğunu bildiren bir hata alırsanız, herhangi bir bağımlılığın gerekli değildir ve Kaldırılabilir görmek için şablonunuzun değerlendirin. Bağımlılıkları kaldırma işe yaramazsa, bazı dağıtım işlemlerini sonra döngüsel bağımlılığı olan kaynakların dağıtıldığı alt kaynakları içine taşıyarak döngüsel bağımlılıklar önleyebilirsiniz. Örneğin, iki sanal makineler dağıtırken, ancak diğer başvuran her birinde özelliklerini ayarlamalısınız varsayalım. Bunları şu sırayla dağıtabilirsiniz:

1. vm1
2. vm2
3. Vm1 uzantısı vm1 ve vm2 bağlıdır. Uzantı vm1, vm2 ' alır değerlerini ayarlar.
4. Vm1 ve vm2 vm2 uzantısı bağlıdır. Uzantı vm1 ' alır vm2 değerlerini ayarlar.

Dağıtım sırası değerlendirmek ve bağımlılık hatalarını çözme hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Sonraki adımlar
* Bağımlılıkları dağıtımı sırasında sorun giderme hakkında bilgi edinmek için [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için [şablonları yazma](resource-group-authoring-templates.md). 
* Bir şablonda kullanabileceğiniz işlevler listesi için bkz. [şablon işlevleri](resource-group-template-functions.md).

