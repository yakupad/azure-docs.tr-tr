---
title: Azure'da Service Fabric üzerindeki Windows kapsayıcılarını izleme ve tanılama | Microsoft Docs
description: Bu öğreticide, Azure Service Fabric üzerindeki Windows kapsayıcıları için Log Analytics izleme ve tanılama özelliğini yapılandıracaksınız.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2018
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: b013627c5a0dc596c9897d7fa2c5bf2b2a79ee40
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114015"
---
# <a name="tutorial-monitor-windows-containers-on-service-fabric-using-log-analytics"></a>Öğretici: Log Analytics kullanarak Service Fabric’te Windows kapsayıcılarını izleme

Bu, bir öğreticinin ikinci bölümüdür ve Log Analytics’i Service Fabric’te düzenlenen Windows kapsayıcılarınızı izleyecek şekilde ayarlamanız konusunda size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Service Fabric kümeniz için Log Analytics’i yapılandırma
> * Kapsayıcılarınızdaki ve düğümlerinizdeki günlükleri görüntülemek ve sorgulamak için bir Log Analytics çalışma alanı kullanma
> * Log Analytics aracısını kapsayıcı ve düğüm ölçümlerini alacak şekilde yapılandırma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

* Azure’da bir kümeye sahip olmak veya [bu öğreticide küme oluşturmak](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Buna kapsayıcılı bir uygulama dağıtmak](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-log-analytics-with-your-cluster-in-the-resource-manager-template"></a>Kaynak Yöneticisi şablonunda kümenizle Log Analytics’i ayarlama

Bu öğreticinin ilk bölümünde [sağlanan şablonu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) kullandıysanız, genel bir Service Fabric Azure Resource Manager şablonuna aşağıdaki eklemeler zaten yapılmıştır. Kendinize ait bir kümeniz varsa ve bunu Log Analytics ile kapsayıcıları izlemek üzere ayarlamak istiyorsanız:

* Resource Manager şablonunuzda aşağıdaki değişiklikleri yapın.
* PowerShell ile [şablonu dağıtarak](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) kümenizi yükseltin. Azure Resource Manager, kaynağın mevcut olduğunu algılar ve yükseltme olarak kullanıma sunar.

### <a name="adding-log-analytics-to-your-cluster-template"></a>Log Analytics’i küme şablonunuza ekleme

*template.json* dosyanızda aşağıdaki değişiklikleri yapın:

1. *Parametreler* bölümünüze Log Analytics çalışma alanı konumunu ve adını ekleyin:

    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your Log Analytics workspace"
      }
    }
    ```

    Bunlardan birinde kullanılan değeri değiştirmek için *template.parameters.json* dosyanıza aynı parametreleri ekleyin ve orada kullanılan değerleri değiştirin.

2. Çözüm adını ve çözümü *değişkenlerinize* ekleyin:

    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. Microsoft Monitoring Agent’ı sanal makine uzantısı olarak ekleyin. Sanal makine ölçek kümeleri kaynağını bulun: *resources* > *"apiVersion": "[variables('vmssApiVersion')]"*. *properties* > *virtualMachineProfile* > *extensionProfile* > *extensions* yolunda, *ServiceFabricNode* uzantısının altına şu uzantı açıklamasını ekleyin: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Log Analytics çalışma alanını ayrı bir kaynak olarak ekleyin. *resources* kısmında, sanal makine ölçek kümeleri kaynağından sonra şunu ekleyin:

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[Burada](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json), gerektiğinde başvurmanız için bu değişikliklerin tümünü içeren (ve bu öğreticinin ilk bölümünde kullanılan) bir örnek şablon verilmiştir. Bu değişiklikler, kaynak grubunuza bir Log Analytics çalışma alanı ekler. Çalışma alanı, [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) aracısıyla yapılandırılan depolama tablolarından Service Fabric platform olaylarını alacak şekilde yapılandırılır. Log Analytics aracısı da (Microsoft Monitoring Agent) kümenizdeki her düğüme sanal makine uzantısı olarak eklenmiştir. Böylece siz kümenizi ölçekledikçe aracı her bir makinede otomatik olarak yapılandırılır ve aynı çalışma alanına bağlanır.

Geçerli kümenizi yükseltmek için şablonu yeni değişikliklerinizle dağıtın. Bu işlem tamamlandığında kaynak grubunuzda Log Analytics kaynaklarını görürsünüz. Küme hazır olduğunda kümeye kapsayıcılı uygulamanızı dağıtın. Sonraki adımda, kapsayıcıları izleme özelliğini ayarlayacağız.

## <a name="add-the-container-monitoring-solution-to-your-log-analytics-workspace"></a>Log Analytics çalışma alanınıza Kapsayıcı İzleme Çözümünü ekleme

Çalışma alanınızda Kapsayıcı çözümünü ayarlamak için *Kapsayıcı İzleme Çözümü* araması yapın ve İzleme ve Yönetim kategorisi altında Kapsayıcılar kaynağı oluşturun.

![Kapsayıcılar çözümü ekleme](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

*Log Analytics Çalışma Alanı* için istem görüntülendiğinde kaynak grubunuzda oluşturulan çalışma alanını seçin ve **Oluştur**’a tıklayın. Bu, çalışma alanınıza bir *Kapsayıcı İzleme Çözümü* ekler ve otomatik olarak, şablon tarafından dağıtılan Log Analytics aracısının docker günlüklerini ve istatistiklerini toplamaya başlamasını sağlar. 

*Kaynak grubunuza* geri dönün. Yeni eklenen izleme çözümünü burada görürsünüz. Bu çözüme tıklarsanız giriş sayfası, çalıştırdığınız kapsayıcı görüntülerinin sayısını size gösterir.

*Öğreticinin [ikinci bölümünde](service-fabric-host-app-in-a-container.md) fabrikam kapsayıcımın 5 örneğini çalıştırdığıma dikkat edin*

![Kapsayıcı çözümü giriş sayfası](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

**Kapsayıcı İzleme Çözümü**’ne tıkladığınızda, birden fazla panelde gezinmenin yanı sıra Log Analytics’te sorgu çalıştırmanızı sağlayan daha ayrıntılı bir panoya yönlendirilirsiniz.

*Eylül 2017 itibarıyla çözümde bazı güncelleştirmeler yapılacağını unutmayın. Birden fazla düzenleyiciyi aynı çözümle tümleştirme üzerinde çalıştığımız için Kubernetes olaylarıyla ilgili aldığınız hataları yoksayabilirsiniz.*

Aracı, docker günlüklerini aldığından varsayılan olarak *stdout* ve *stderr*’i gösterecek şekilde ayarlanır. Sağa kaydırdığınızda kapsayıcı görüntüsü envanterinin, durumun, ölçümlerin yanı sıra diğer faydalı verileri almak için çalıştırabileceğiniz örnek sorguları da görürsünüz.

![Kapsayıcı çözümü panosu](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Bu panellerden herhangi birine tıklamanız sizi görüntülenen değeri oluşturan Log Analytics sorgusuna yönlendirir. Alınmakta olan tüm farklı günlük türlerini görmek için sorguyu *\** olarak değiştirin. Buradan kapsayıcı performansını ve günlükleri sorgulayabilir, bunlar için filtreleme yapabilir veya Service Fabric platform olaylarına bakabilirsiniz. Aynı zamanda aracılarınız her düğümden sürekli olarak bir sinyal yayar. Böylece, küme yapılandırmanızın değişmesi durumunda verilerin tüm makinelerinizden toplanmaya devam ettiğinden emin olmak için bu sinyallere bakabilirsiniz.

![Kapsayıcı sorgusu](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-log-analytics-agent-to-pick-up-performance-counters"></a>Log Analytics aracısını performans sayaçlarını alacak şekilde yapılandırma

Log Analytics aracısını kullanmanın diğer bir avantajı da her seferinde Azure tanılama aracısını yapılandırmak ve Resource Manager şablonuna dayalı bir yükseltme gerçekleştirmek zorunda kalmadan Log Analytics arabirim deneyimi aracılığıyla almak istediğiniz performans sayaçlarını değiştirebilmenizdir. Bunu yapmak için Kapsayıcı İzleme (veya Service Fabric) çözümünüzün giriş sayfasında **OMS Çalışma Alanı**’na tıklayın.

Bu sizi Log Analytics Çalışma Alanınıza yönlendirir. Burada çözümlerinizi görüntüleyebilir, özel panolar oluşturabilir ve Log Analytics aracısını yapılandırabilirsiniz. 
* Gelişmiş Ayarlar menüsünü açmak için **Gelişmiş Ayarlar**’a tıklayın.
* **Bağlı Kaynaklar** > **Windows Sunucuları**’na tıklayarak *5 Windows Bilgisayarı Bağlı* ifadesini gördüğünüzden emin olun.
* Yeni performans sayaçlarını aramak ve eklemek için **Veriler** > **Windows Performans Sayaçları**’na tıklayın. Burada, toplayabileceğiniz performans sayaçlarına ilişkin Log Analytics önerilerinin listesinin yanı sıra başka sayaçları arama seçeneğini de görürsünüz. Doğrulayın **Processor(_Total)\% İşlemci Zamanı** ve **Memory(*)\Available MBytes** sayaçlarının toplandığını doğrulayın.

Kapsayıcı İzleme Çözümünüzü birkaç dakika içinde **yenileyin**. Bunu yaptığınızda *Bilgisayar Performansı* verilerinin geldiğini görmeye başlarsınız. Bu, kaynaklarınızın nasıl kullanıldığını anlamanıza yardımcı olur. Kümenizi ölçeklendirme konusunda doğru kararlar vermek veya bir kümenin yükünüzü beklenen şekilde dengeleyip dengelemediğini doğrulamak için de bu ölçümleri kullanabilirsiniz.

*Not: Zaman filtrelerinin bu ölçümleri kullanabilmeniz için uygun şekilde ayarlandığından emin olun.*

![Performans sayaçları 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Service Fabric kümeniz için Log Analytics’i yapılandırma
> * Kapsayıcılarınızdaki ve düğümlerinizdeki günlükleri görüntülemek ve sorgulamak için bir Log Analytics çalışma alanı kullanma
> * Log Analytics aracısını kapsayıcı ve düğüm ölçümlerini alacak şekilde yapılandırma

Kapsayıcılı uygulamanız için izlemeyi ayarladığınıza göre artık aşağıdaki deneyebilirsiniz:

* Yukarıdaki adımların aynısını uygulayarak bir Linux kümesi için Log Analytics’i ayarlayın. Kaynak Yöneticisi şablonunuzda değişiklik yapmak için [bu şablona](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) başvurun.
* Algılama ve tanılama konusunda yardımcı olması için Log Analytics’i [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) özelliğini ayarlayacak şekilde yapılandırın.
* Service Fabric'in kümeleriniz için yapılandırılacak [önerilen performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) listesini keşfedin.
* Log Analytics’in bir parçası olarak sunulan [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri hakkında bilgi sahibi olun.