---
title: Redis önbelleği ile Web uygulaması sağlama
description: Redis Cache ile web uygulamasını dağıtmak için Azure Resource Manager şablonu kullanın.
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 6e99c71f-ef8e-4570-a307-e4c059e60c35
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: wesmc
ms.openlocfilehash: b787b3f2caaff5535557c03c45b058e1bbb0c77d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38671410"
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a>Bir Web uygulaması ek bir şablon kullanarak Redis önbelleği oluşturma
Bu konu başlığında, Redis önbelleği ile bir Azure Web uygulaması dağıtır bir Azure Resource Manager şablonunun nasıl oluşturulacağını öğreneceksiniz. Nasıl tanımlamak için hangi kaynaklara dağıtılır ve parametrelerin nasıl dağıtıldığının ve dağıtım yürütülürken belirtilen öğreneceksiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

Şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Tam şablon için bkz: [Web uygulamasını Redis Cache şablonuyla](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Ne dağıtacaksınız
Bu şablonda dağıtacaksınız:

* Azure Web Uygulaması
* Azure Redis önbelleği.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye tıklayın:

[![Azure’a dağıtma](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Parametreleri belirtmek için
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Adları için değişkenleri
Bu şablon, kaynakların adlarını oluşturmak için değişkenleri kullanır. Kullandığı [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) işlev bir değer oluşturmak için temel kaynak grubu kimliği.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a>Redis Cache
Web uygulaması ile kullanılan Azure Redis önbelleği oluşturur. Önbellek adı belirtilen **cacheName** değişkeni.

Şablon Önbelleği kaynak grubu ile aynı konumda oluşturur.

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a>Web uygulaması
Belirtilen ada sahip web uygulaması oluşturur **webSiteName** değişkeni.

Web uygulamasını Redis önbellek ile çalışacak şekilde etkinleştirmek uygulama ayarı özellikleri ile yapılandırıldığını dikkat edin. Ayarları dinamik olarak oluşturulan bu uygulama dağıtımı sırasında sağlanan değerlere göre.

    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
