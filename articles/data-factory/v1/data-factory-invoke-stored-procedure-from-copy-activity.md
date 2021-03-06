---
title: Azure Data Factory kopyalama etkinliği saklı yordam çağırma | Microsoft Docs
description: Bir Azure Data Factory kopyalama etkinliği Azure SQL Database veya SQL Server saklı bir yordam çağırma öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e75573513f107977e1d5fe62fbae89cb4439e0e9
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048960"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Kopya etkinliği Azure Data Factory içinde depolanan yordamı çağırma
> [!NOTE]
> Bu makale, veri fabrikası 1 sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [dönüştürme veri fabrikasında saklı yordam etkinliği kullanarak verileri](../transform-data-using-stored-procedure.md).


Veri kopyalama işlemi sırasında [SQL Server](data-factory-sqlserver-connector.md) veya [Azure SQL veritabanı](data-factory-azure-sql-connector.md), yapılandırabileceğiniz **SqlSink** bir saklı yordam çağrılacak kopyalama etkinliğinde. Saklı kullanmak isteyebilirsiniz (sütun değerleri, birden çok tablo, vb. ekleme bakarak, birleştirme) herhangi bir ek işlem gerçekleştirmek için yordamı verileri hedef tabloyla eklemeden önce gereklidir. Bu özellik yararlandığı [tablo değerli parametreleri](https://msdn.microsoft.com/library/bb675163.aspx). 

Aşağıdaki örnek, bir Data Factory işlem hattı (kopyalama etkinliği) SQL Server veritabanından içinde saklı yordamı çağırma gösterilmektedir:  

## <a name="output-dataset-json"></a>Çıktı veri kümesi JSON
Çıkış dataset JSON'da, ayarlayın **türü** için: **SqlServerTable**. Ayarlamak **AzureSqlTable** bir Azure SQL veritabanı ile kullanmak için. Değeri **tableName** özelliği, saklı yordamın ilk parametresinin adı eşleşmelidir.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Kopyalama etkinliği JSON SqlSink bölümünde
Tanımlamak **SqlSink** kopyalama etkinliği JSON gibi bölüm. Havuz/hedef veritabanına veri eklerken bir saklı yordam çağırmak için değerleri için her ikisini de belirtin **SqlWriterStoredProcedureName** ve **SqlWriterTableType** özellikleri. Bu özelliklerin açıklamaları için bkz: [SQL Server Bağlayıcısı makaledeki SqlSink bölümüne](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Saklı yordam tanımı 
Aynı ada sahip saklı yordam veritabanınızdaki tanımlamak **SqlWriterStoredProcedureName**. Saklı yordam kaynak veri deposu gelen giriş verilerinin işler ve hedef veritabanı tablosunda veri ekler. Saklı yordam ilk parametresinin adı JSON (pazarlama) kümesinde tanımlanan tableName eşleşmesi gerekir.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Tablo türü tanımı
Aynı ada sahip bir tablo türü veritabanınızdaki tanımlamak **SqlWriterTableType**. Tablo türü şeması girdi veri kümesi şeması ile eşleşmesi gerekir.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Sonraki adımlar
Tam JSON örnekler için makaleler aşağıdaki Bağlayıcısı'nı gözden geçirin: 

- [Azure SQL Veritabanı](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
