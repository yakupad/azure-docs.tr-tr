---
title: Azure Data Factory kullanarak müşteri için/SAP bulut için veri kopyalama | Microsoft Docs
description: Veri müşteri desteklenen havuz veri depoları için SAP buluttan (veya) desteklenen kaynak veri depolarına SAP buluta müşteri için Data Factory kullanarak kopyalamak öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: jingwang
ms.openlocfilehash: df45613105c8fb005fc8ba0c796ef768e293c57e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052441"
---
# <a name="copy-data-from-sap-cloud-for-customer-c4c-using-azure-data-factory"></a>SAP bulut müşteri (C4C) için Azure Data Factory kullanarak verilerden kopyalama

Bu makalede kopya etkinliği Azure Data Factory'de (C4C) müşteri için başlangıç/bitiş SAP bulut veri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri SAP Buluttan müşteri için herhangi bir desteklenen havuz veri deposuna kopyalamak veya veri tüm desteklenen kaynak veri deposundan SAP buluta müşteri için kopyalayın. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu bağlayıcı, satış, SAP bulut hizmeti için ve SAP bulut sosyal katılım çözümleri için SAP bulut dahil olmak üzere müşteri için başlangıç/bitiş SAP bulut veri kopyalamak Azure Data Factory sağlar.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler için müşteri bağlayıcısı SAP buluta Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler için SAP bulut bağlı müşteri hizmetleri için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapCloudForCustomer**. | Evet |
| url | SAP C4C OData hizmeti URL'si. | Evet |
| kullanıcı adı | SAP C4C bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirtilen kullanıcı hesabı için parola belirtin. Bu alan veri fabrikasında güvenli bir şekilde depolamak için bir SecureString olarak işaretle veya [Azure anahtar kasasında depolanan gizli başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Kaynak havuzu için Evet için Hayır'ı |

>[!IMPORTANT]
>Müşteri, SAP bulutunu verileri açıkça kopyalamak için [Azure IR oluşturmak](create-azure-integration-runtime.md#create-azure-ir) müşteri ve bağlantılı hizmet ilişkilendirme için SAP bulut yakın bir konum aşağıdaki örnekteki gibi:

**Örnek:**

```json
{
    "name": "SAPC4CLinkedService",
    "properties": {
        "type": "SapCloudForCustomer",
        "typeProperties": {
            "url": "https://<tenantname>.crm.ondemand.com/sap/c4c/odata/v1/c4codata/" ,
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, müşteri veri kümesi için SAP bulut tarafından desteklenen özellikler listesini sağlar.

Müşteri için SAP Buluttan verileri kopyalamak için veri kümesi türü özelliğini ayarlayın **SapCloudForCustomerResource**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **SapCloudForCustomerResource** |Evet |
| yol | SAP C4C OData varlık yolunu belirtin. |Evet |

**Örnek:**

```json
{
    "name": "SAPC4CDataset",
    "properties": {
        "type": "SapCloudForCustomerResource",
        "typePoperties": {
            "path": "<path e.g. LeadCollection>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP C4C linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölüm için müşteri kaynağı SAP bulut tarafından desteklenen özellikler listesini sağlar.

### <a name="sap-c4c-as-source"></a>Kaynak olarak SAP C4C

Müşteri için SAP Buluttan verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **SapCloudForCustomerSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapCloudForCustomerSource**  | Evet |
| sorgu | Verileri okumak için özel OData sorgu belirtin. | Hayır |

Belirli bir gün için veri almak için örnek sorgu: `"query": "$filter=CreatedOn ge datetimeoffset'2017-07-31T10:02:06.4202620Z' and CreatedOn le datetimeoffset'2017-08-01T10:02:06.4202620Z'"`

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPC4C",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP C4C input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapCloudForCustomerSource",
                "query": "<custom query e.g. $top=10>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sap-c4c-as-sink"></a>SAP C4C havuz olarak

Müşteri için SAP buluta verileri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SapCloudForCustomerSink**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **SapCloudForCustomerSink**  | Evet |
| WriteBehavior | İşlemi yazma davranışını. "Ekle", "Güncelleştir" olabilir. | Hayır. Varsayılan "Ekle". |
| writeBatchSize | Yazma işlemi toplu iş boyutu. En iyi performansı elde etmek için toplu iş boyutu farklı bir tablo veya sunucu için farklı olabilir. | Hayır. Varsayılan olarak 10. |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSapC4c",
        "type": "Copy",
        "inputs": [{
            "type": "DatasetReference",
            "referenceName": "<dataset type>"
        }],
        "outputs": [{
            "type": "DatasetReference",
            "referenceName": "SapC4cDataset"
        }],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SapCloudForCustomerSink",
                "writeBehavior": "Insert",
                "writeBatchSize": 30
            },
            "parallelCopies": 10,
            "dataIntegrationUnits": 4,
            "enableSkipIncompatibleRow": true,
            "redirectIncompatibleRowSettings": {
                "linkedServiceName": {
                    "referenceName": "ErrorLogBlobLinkedService",
                    "type": "LinkedServiceReference"
                },
                "path": "incompatiblerows"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-cloud-for-customer"></a>Müşteri SAP bulut için veri türü eşlemesi

Veriler müşteri için SAP Buluttan kopyalarken, aşağıdaki eşlemelerini SAP Buluttan Azure Data Factory geçici veri türleri için müşteri veri türleri için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) nasıl kopyalama etkinliği kaynak şema ve veri türü için havuz eşlemeleri hakkında bilgi edinmek için.

| SAP C4C OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Edm.Binary | Byte] |
| Edm.Boolean | bool |
| Edm.Byte | Byte] |
| Edm.DateTime | DateTime |
| Edm.Decimal | Ondalık |
| Edm.Double | çift |
| Edm.Single | Tek |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
