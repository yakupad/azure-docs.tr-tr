---
title: Azure Log Analytics ile kaynakları genelinde arama yapma | Microsoft Docs
description: Bu makalede nasıl birden çok çalışma alanları ve App Insights uygulama kaynaklarda aboneliğinizde Sorgulayabileceğiniz açıklanır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: e7ca3bcb3c3322c0eba12d7f9eb2ee2bc7b7600c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049856"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Log Analytics'te kaynaklar arası günlük aramaları gerçekleştirme  

Daha önce Azure Log Analytics ile yalnızca geçerli çalışma alanındaki verileri çözümleyebilir ve aboneliğinizde tanımlanmış birden çok çalışma alanında sorgu yapabilmenizi sınırlıdır.  Ayrıca, Application ınsights'ta doğrudan Application Insights ile web tabanlı uygulamanızın içinden veya Visual Studio'dan toplanan telemetri öğelerinin yalnızca arama yapabilirsiniz.  Bu ayrıca yerel olarak operasyonel analiz etmek için sınama ve uygulama verileri birlikte hale.   

Artık yalnızca birden fazla Log Analytics çalışma alanları, aynı zamanda aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından verileriniz üzerinde sorgulama yapabilirsiniz. İle verilerinizin sistem genelinde bir görünüm sağlar.  Yalnızca bu tür bir sorgu gerçekleştirebileceğiniz [Gelişmiş portal](log-analytics-log-search-portals.md#advanced-analytics-portal), Azure Portalı'nda. 100'e (Log Analytics çalışma alanları ve Application Insights uygulaması), tek bir sorguda dahil edebileceğiniz kaynak sayısı sınırlıdır. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Application ınsights ve Log Analytics çalışma alanları arasında sorgulama
Sorgunuzu başka bir çalışma alanı başvurmak için kullanma [ *çalışma* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) tanımlayıcısı ve Application ınsights'tan bir uygulama için [ *uygulama* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())tanımlayıcısı.  

### <a name="identifying-workspace-resources"></a>Çalışma alanı kaynakları tanımlama
Aşağıdaki örnekler, günlükleri özetlenen sayısını güncelleştirme tablosundan adlı bir çalışma alanına dönmek için Log Analytics çalışma alanları arasında sorguları gösterir. *contosoretail BT*. 

Bir çalışma alanı tanımlama yollarından biri kullanılarak gerçekleştirilebilir olabilir:

* Kaynak adı - bazen denir, çalışma alanının okunabilir bir ad olduğu *bileşen adı*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Bir çalışma alanı adına göre tanımlayan benzersizlik tüm erişilebilir aboneliklerindeki varsayar. Belirtilen ada sahip birden çok uygulamalarınız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda, diğer tanımlayıcılarla birini kullanmanız gerekir.

* Tam adı - abonelik adı, kaynak grubu ve bileşen adı şu biçimde oluşan çalışma alanında, "tam adı" olduğunu: *resourceGroup/subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count `

    >[!NOTE]
    >Azure aboneliği adları benzersiz olmuyor çünkü bu tanımlayıcısı belirsiz olabilir. 
    >

* Çalışma alanı kimliği - atanan genel benzersiz tanıtıcısı (GUID) temsil edilen her bir çalışma alanı için benzersiz, sabit, tanımlayıcı bir çalışma alanı kimliğidir.

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure Resource ID: çalışma alanının Azure tanımlı benzersiz kimliği. Kaynak adı belirsiz olduğunda kaynak Kimliğini kullanın.  Çalışma alanları için biçimi şu şekildedir: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Çalışma alanları/Operationalınsights/componentName*.  

    Örneğin:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Update | count
    ```

### <a name="identifying-an-application"></a>Bir uygulama tanımlama
Aşağıdaki örnekte adlı bir uygulama karşı yapılan istekleri özetlenmiş bir sayısını döndürmek *fabrikamapp* Application ınsights. 

İle bir Application Insights uygulamasında tanımlama gerçekleştirilebilir *app(Identifier)* ifade.  *Tanımlayıcı* bağımsız değişkeni, aşağıdakilerden birini kullanarak uygulamayı belirtir:

* -Kaynak adı olduğundan bazen denir, app insan tarafından okunabilir adını *bileşen adı*.  

    `app("fabrikamapp")`

* Tam adı - addır "tam" uygulamasının abonelik adı, kaynak grubu ve bileşen adı şu biçimde oluşur: *resourceGroup/subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Azure aboneliği adları benzersiz olmuyor çünkü bu tanımlayıcısı belirsiz olabilir. 
    >

* Kimliği - uygulama uygulamasının GUID.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure kaynak kimliği - uygulamayı Azure tanımlı benzersiz kimliği. Kaynak adı belirsiz olduğunda kaynak Kimliğini kullanın. Biçim: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Operationalınsights/bileşenleri/componentName*.  

    Örneğin:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>Birden fazla kaynak arasında sorgu gerçekleştirme
Birden çok kaynak kaynak örnekleriniz hiçbirini sorgulayabilir, bunlar çalışma alanları ve uygulamaları olabilir birleştirilmiş.
    
Örneğin, iki çalışma alanları arasında sorgu:    
    ```
    union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
    | where TimeGenerated >= ago(1h)
    | where UpdateState == "Needed"
    | summarize dcount(Computer) by Classification
    ```

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [Log Analytics günlük araması başvuru](https://docs.loganalytics.io/docs/Language-Reference) tüm kullanılabilir Log Analytics'te sorgu söz dizimi seçeneklerini görüntüleyin.    
