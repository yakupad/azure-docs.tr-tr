---
title: Azure Data Lake Analytics U-SQL kataloğunda kullanmaya başlama
description: U-SQL kataloğunu kod ve veri paylaşmak için nasıl kullanılacağını öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: saveenr
editor: jasonwhowell
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.topic: conceptual
ms.date: 05/09/2017
ms.openlocfilehash: 35a39733987eba7060049db7005c1f6bc0058e63
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34624361"
---
# <a name="get-started-with-the-u-sql-catalog-in-azure-data-lake-analytics"></a>Azure Data Lake Analytics U-SQL kataloğunda kullanmaya başlama

## <a name="create-a-tvf"></a>Bir TVF oluşturma

Önceki U-SQL komut dosyası aynı kaynak dosyasını okumaya EXTRACT kullanımını yinelenir. U-SQL tablo değerli işlev (TVF), gelecekte tekrar kullanmak için veri yerleştirebilirsiniz.  

Aşağıdaki komut dosyası olarak adlandırılan bir TVF oluşturur `Searchlog()` şema ve varsayılan veritabanı içinde:

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

Aşağıdaki komut dosyası önceki betik tanımlandı TVF kullanmayı gösterir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SerachLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Görünümler oluşturma

Bir tek sorgu ifadesi varsa, TVF yerine, U-SQL görünümü ifade kapsüllemek için kullanabilirsiniz.

Aşağıdaki komut dosyası adlı bir görünüm oluşturur `SearchlogView` şema ve varsayılan veritabanı içinde:

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

Aşağıdaki komut dosyası tanımlı görünüm kullanımını göstermektedir:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a>Tablo oluşturma
İlişkisel veritabanı tablolarıyla U-SQL ile bir tablo ile önceden tanımlanmış bir şema oluşturabilir veya şema sorgusu oluşturur bir tablo oluşturmak gibi tablonun (olarak da bilinen CREATE TABLE AS SELECT veya CTAS) doldurur.

Aşağıdaki komut dosyası kullanarak bir veritabanı ve iki tablo oluşturun:

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a>Sorgu tabloları
Tablolar, veri dosyalarını sorgu aynı şekilde önceki komut dosyasındaki oluşturulanlar gibi sorgulayabilirsiniz. EXTRACT kullanarak bir satır kümesi oluşturmak yerine, şimdi tablo adına başvurabilir.

Tablodan okumak için daha önce kullanılan dönüştürme komut dosyasını değiştirin:

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    TO "/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 >Şu anda, bir SELECT aynı komut dosyasını bir tabloda tablonun oluşturulduğu çalıştıramazsınız.

## <a name="next-steps"></a>Sonraki Adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
