---
title: Azure Uyarıları'te günlük uyarıları için Web kancası eylemleri
description: Bu makalede, nasıl bir günlük uyarı kuralı için günlük analizi veya uygulama Öngörüler push kullanılarak veri HTTP Web kancası ve farklı özelleştirmeler ayrıntılarını olası açıklanmaktadır.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 304476e2d6862fbb6a859ae6fefe96d177b1111b
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264264"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Günlük uyarı kuralları için Web kancası eylemleri
Zaman bir [uyarı Azure içinde oluşturulan ](monitor-alerts-unified-usage.md), seçeneğiniz vardır [Eylem grupları kullanarak yapılandırma](monitoring-action-groups.md) bir veya daha fazla eylemleri gerçekleştirmek için.  Bu makalede, özel JSON tabanlı Web kancası yapılandırma hakkında ayrıntılar ve kullanılabilir farklı Web kancası eylemleri açıklanmaktadır.


## <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir dış işlem tek bir HTTP POST isteği üzerinden çağırma olanak tanır.  Çağrılan Hizmet Web kancalarını destekleyen ve nasıl herhangi yükü kullandığını belirlemek aldığı.   Bir Web kancası yanıt olarak bir uyarı kullanma örnekleri bir ileti gönderiyorsunuz [Slack'e](http://slack.com) veya bir olay oluşturma [PagerDuty](http://pagerduty.com/).  

Web kancası eylemleri özellikler aşağıdaki tabloda gerektirir:

| Özellik | Açıklama |
|:--- |:--- |
| Web kancası URL'si |Web kancası URL'si. |
| Özel JSON yükü |Uyarı oluşturulurken bu seçenek seçildiğinde Web kancası ile göndermek için özel yükü. Ayrıntılar adresinde [Azure uyarıları kullanarak Uyarıları yönetme ](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Web kancası düğmesi yanında test *INCLUDE özel JSON yükü Web kancası için* seçeneği günlük uyarı için Web kancası URL'si test etmek için sahte çağrısı tetikler. Gerçek veri ve günlük uyarılar için kullanılan JSON şeması temsilcisi içermiyor. 

Web kancası bir URL ve dış hizmete gönderilen veriler JSON biçimli bir yükü içerir.  Varsayılan olarak, yükü değerleri aşağıdaki tabloda içerir: özel bir kendi bu yükü tarihle seçebilirsiniz.  Bu durumda, değişkenleri tabloda her parametre için değer özel yükünüzü dahil etmek için kullanabilirsiniz.


| Parametre | Değişken | Açıklama |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Uyarı kuralı adı. |
| Severity |#severity |Önem derecesi için Mazotlu günlük uyarı ayarlanmıştır. |
| AlertThresholdOperator |#thresholdoperator |Uyarı kuralı için eşik işleci.  *Büyük* veya *değerinden*. |
| AlertThresholdValue |#thresholdvalue |Uyarı kuralı için eşik değer. |
| LinkToSearchResults |#linktosearchresults |Uyarı oluşturulan sorgudan kayıtları döndürür Analytics portalı bağlayın. |
| ResultCount |#searchresultcount |Arama sonuçlarında kayıt sayısı. |
| Arama aralığı bitiş saati |#searchintervalendtimeutc |Bitiş zamanı, UTC sorguda biçimi - aa/gg/yyyy ss: dd: ss AM/PM. |
| Arama aralığı |#searchinterval |Zaman penceresi için uyarı kuralı, biçimi - SS: dd:. |
| Arama aralığı başlangıç saati |#searchintervalstarttimeutc |UTC olarak başlangıç zamanı sorgu için biçimi - aa/gg/yyyy ss: dd: ss AM/PM.. 
| SearchQuery |#searchquery |Uyarı kuralı tarafından kullanılan günlük arama sorgusu. |
| SearchResults |"IncludeSearchResults": true|Bir JSON tablosu olarak ilk 1.000 kayıtları sınırlı sorgu tarafından döndürülen kayıt; varsa "IncludeSearchResults": true, özel JSON Web kancası tanımında en üst düzey bir özellik olarak eklenir. |
| Workspaceıd |#workspaceid |Günlük analizi çalışma alanı kimliği. |
| Uygulama Kimliği |#applicationid |Uygulama Insight Kimliğini uygulama. |
| Abonelik Kimliği |#subscriptionid |Application Insights ile kullanılan Azure aboneliğinizi kimliği. 


Örneğin, adlı tek bir parametre içeren aşağıdaki özel yükü belirtebilir *metin*.  Bu Web kancası çağırır hizmet, bu parametre bekleniyor.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Bu örnek yükü için Web kancası gönderildiğinde aşağıdaki gibi bir şey çözümlemek.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
"#Searchinterval" gibi JSON muhafaza içinde belirtilen için özel bir Web kancası tüm değişkenler olması gibi sonuç Web kancası gibi muhafaza içinde değişken veri da sahip olur "00: 05:00".

Özel bir yükte arama sonuçlarında için emin **IncudeSearchResults** json yükü en üst düzey özelliği olarak ayarlayın. 

## <a name="sample-payloads"></a>Örnek yükü
Bu bölümde, günlük için uyarıları, yükü standart olduğunda ve ne zaman gibi Web kancası için örnek yükü gösterir kendi özel.

> [!NOTE]
> Geriye dönük uyumluluk sağlamak üzere Azure günlük analizi kullanarak uyarıları için standart Web kancası yükü aynı [günlük analizi uyarı Yönetim](../log-analytics/log-analytics-alerts-creating.md). Ancak kullanarak günlük uyarılar için [Application Insights](../application-insights/app-insights-analytics.md), standart Web kancası yükü eylem Grup şemasını temel alan.

### <a name="standard-webhook-for-log-alerts"></a>Standart Web kancası günlük uyarılar için 
Bu örneklerin her ikisi de yalnızca iki sütun ve iki satır bir kukla yükü açıklamıştır.

#### <a name="log-alert-for-azure-log-analytics"></a>Azure günlük analizi için günlük uyarı
Standart Web kancası eylemi için örnek yükü aşağıdadır *özel Json seçeneği olmadan* günlük analizi tabanlı uyarılar için kullanılan.

```json
{
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
 }
 ```   

#### <a name="log-alert-for-azure-application-insights"></a>Azure Application Insights için günlük uyarı
Standart bir Web kancası için örnek yükü aşağıdadır *özel Json seçeneği olmadan* uygulama Öngörüler tabanlı günlük-uyarıları için kullanıldığında.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Error",
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Özel JSON yükü olan günlük uyarı
Örneğin, yalnızca uyarı adı ve arama sonuçlarını içeren özel bir yükü oluşturmak için aşağıdakileri kullanabilirsiniz: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Herhangi bir günlük uyarı için bir özel Web kancası eylemi için örnek yükü aşağıdadır.
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [uyarıları Azure Uyarıları'nda oturum açın ](monitor-alerts-unified-log.md)
- Oluşturma ve yönetme [azure'da Eylem grupları](monitoring-action-groups.md)
- Daha fazla bilgi edinmek [Application Insights](../application-insights/app-insights-analytics.md)
- Daha fazla bilgi edinmek [günlük analizi](../log-analytics/log-analytics-overview.md). 