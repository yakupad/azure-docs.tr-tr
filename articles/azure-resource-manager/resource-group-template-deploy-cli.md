---
title: Azure CLI ve şablon kaynakları dağıtma | Microsoft Docs
description: Bir kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure CLI kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 5a6b227cee3765593adbda430d8c47312f996c18
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723843"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma

Bu makalede, Azure CLI'yı Resource Manager şablonları ile kaynakları Azure'a dağıtmak için nasıl kullanılacağını açıklanmaktadır. Dağıtma ile ilgili kavramları hakkında bilgi sahibi değilseniz ve Azure çözümlerinizi bkz [Azure Resource Manager'a genel bakış](resource-group-overview.md).  

Resource Manager şablonu makinenizde yerel bir dosya veya GitHub gibi bir depoda bulunan bir dış dosya olabilir dağıtın. Bu makalede dağıttığınız şablonu kullanılabilir [örnek şablonu](#sample-template) bölümünde veya farklı bir [depolama hesabı GitHub şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Azure CLI'yı yüklü değilse, kullanabileceğiniz [Cloud Shell](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Kaynakları Azure'a dağıtırken:

1. Azure hesabınızda oturum açma
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. Bu, en fazla 90 karakter olabilir. Bu nokta ile bitemez.
3. Kaynak grubunu oluşturmak için gereken kaynakları tanımlayan şablonu dağıtma

Şablon dağıtımı özelleştirmenize olanak sağlayan parametreler ekleyebilir. Örneğin, belirli bir ortama (örneğin, geliştirme, test ve üretim gibi) için uygun değerler sağlayabilirsiniz. Örnek şablonu, depolama hesabı SKU'su için parametre tanımlar. 

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren bir ileti görürsünüz:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Dış şablonu dağıtma

Resource Manager şablonları, yerel makinenizde depolamak yerine dış bir konumda depolanması tercih edebilirsiniz. Şablonları bir kaynak denetim deposu (örneğin GitHub) depolayabilirsiniz. Veya, bunları paylaşılan erişim için bir Azure depolama hesabında kuruluşunuzda depolayabilirsiniz.

Dış bir şablonu dağıtmak için **URI şablonu** parametresi. URI örnekte, github'dan örnek şablonu dağıtmak için kullanın.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

Yukarıdaki örnekte genel olarak erişilebilen bir URI şablonu için çoğu senaryo için şablonunuzu hassas bir veri içermemesi çünkü düşünülerek gerektirir. Hassas verileri (örneğin, bir yönetici parolası) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre geçirin. Ancak, şablonunuzu genel olarak erişilebilir olmasını istemiyorsanız, bir özel depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Cloud Shell'de aşağıdaki komutları kullanın:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az group deployment create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="deploy-to-more-than-one-resource-group-or-subscription"></a>Birden fazla kaynak grubuna veya aboneliğe dağıtma

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesini birlikte dağıtmak ancak farklı kaynak gruplarında ya da abonelik yerleştirmek istediğiniz senaryolar da vardır. Tek bir dağıtımda yalnızca beş kaynak gruplarına dağıtabilirsiniz. Daha fazla bilgi için [birden fazla abonelik veya kaynak grubu dağıtma Azure kaynaklarına](resource-manager-cross-resource-group-deployment.md).

## <a name="parameter-files"></a>Parametre dosyaları

Betiğinizde değerleri satır içi olarak parametre geçirme yerine parametre değerlerini içeren bir JSON dosyası kullanmak daha kolay. Parametre dosyasını şu biçimde olmalıdır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Parametreler bölümü (storageAccountType), şablonunuzda tanımlanan parametre eşleşen bir parametre adı içerdiğine dikkat edin. Parametre dosyasını, parametre için bir değer içerir. Bu değer, şablona dağıtım sırasında otomatik olarak geçirilir. Farklı dağıtım senaryoları için birden çok parametre dosyası oluşturun ve ardından uygun bir parametre dosyası geçirin. 

Önceki örneği kopyalayabilir ve adlı bir dosya kaydedin `storage.parameters.json`.

Bir yerel parametre dosyası geçirmek için kullanmak `@` storage.parameters.json adlı yerel bir dosya belirtmek için.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Şablon dağıtımı test etme

Şablonu ve parametre değerleriniz tüm kaynakları gerçekten dağıtmadan test etmek için [az grubu dağıtımını doğrula](/cli/azure/group/deployment#az_group_deployment_validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Hata algılanırsa, komut test dağıtım hakkında bilgi döndürür. Özellikle dikkat **hata** değeri NULL'dur.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Hata algılanırsa, komutu bir hata iletisi döndürür. Örneğin, depolama hesabının SKU, yanlış bir değer geçirmeye çalışırken şu hatayı döndürür:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Komut, şablonunuzun söz dizimi hatası varsa, şablon ayrıştırılamadı belirten bir hata döndürür. İleti satır numarasını ve ayrıştırma hatası konumunu gösterir.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Tam modda kullanmak için `mode` parametresi:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a>Örnek şablon

Bu makaledeki örneklerde aşağıdaki şablonu kullanılır. Kopyalayıp storage.json adlı bir dosya olarak kaydedin. Bu şablonu oluşturma hakkında bilgi almak için bkz. [ilk Azure Resource Manager şablonunuzu oluşturma](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Bu makaledeki örneklerde, varsayılan aboneliğinizde bir kaynak grubunda kaynak dağıtın. Farklı bir aboneliği kullanmak için bkz: [birden çok Azure aboneliklerini yönetme](/cli/azure/manage-azure-subscriptions-azure-cli).
* Bir şablon dağıtan bir tam örnek betik için bkz. [Resource Manager şablonu dağıtım betiği](resource-manager-samples-cli-deploy.md).
* Şablonunuzda parametreleri tanımlayan anlamak için bkz. [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).
* Sık karşılaşılan dağıtım hataları çözümleme hakkında daha fazla ipucu için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-cli-sas-token.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
