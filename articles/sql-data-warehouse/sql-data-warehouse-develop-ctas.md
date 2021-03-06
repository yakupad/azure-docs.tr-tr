---
title: TABLE AS SELECT (CTAS) Azure SQL Data Warehouse oluşturma | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse oluşturma tablo AS seçin (CTAS) deyiminde ile kodlamak için ipuçları.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 9bff6b1216ae826203b24a2cdf8a3d7fd0fd586f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31599093"
---
# <a name="using-create-table-as-select-ctas-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda CREATE TABLE AS SELECT (CTAS) kullanma
Çözümleri geliştirme için Azure SQL Data Warehouse oluşturma tablo AS seçin (CTAS) T-SQL deyiminde ile kodlamak için ipuçları.

## <a name="what-is-create-table-as-select-ctas"></a>OLUŞTURMA tablo AS seçin (CTAS) nedir?

[CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) veya CTAS deyimi en önemli T-SQL özelliklerini biridir. Bir SELECT deyimi çıktıya dayalı yeni bir tablo oluşturur paralel bir işlemdir. CTASD bir tablonun kopyasını oluşturmak için basit ve hızlı bir yoludur. 

## <a name="selectinto-vs-ctas"></a>SEÇİN... VS. CTAS
CTAS Süper kartınızdan bir sürümü olarak düşünebilirsiniz [seçin... İÇİNE](/sql/t-sql/queries/select-into-clause-transact-sql) deyimi.

Basit bir SELECT örneği aşağıdadır... İÇİNE:

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

Önceki örnekte `[dbo].[FactInternetSales_new]` olması oluşturulur ROUND_ROBIN dağıtılmış tablosu kümelenmiş COLUMNSTORE DİZİNİNE sahip olarak bu Azure SQL Data warehouse'da tablo Varsayılanları olduğundan.

SEÇİN... İÇİNE, ancak, dağıtım yöntemini veya dizin türü işleminin bir parçası değiştirmenize izin vermiyor. CTAS nereden geldiğini budur.

Önceki örneği CTAS dönüştürmek için oldukça düz ilet:

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
    DISTRIBUTION = ROUND_ROBIN
,   CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

İle CTAS, dağıtım, tablo türü yanı sıra tablo verileri değiştirme imkanınız olur. 

> [!NOTE]
> Yalnızca dizinde değiştirmeye çalışıyorsanız, `CTAS` işlemi ve kaynak tablosu olduğu dağıtılmış karma sonra `CTAS` işlemi aynı dağıtım sütun ve veri türü sahipseniz en iyi şekilde gerçekleştirir. Bu dağıtım veri taşıma daha etkilidir işlemi sırasında kaçının.
> 
> 

## <a name="using-ctas-to-copy-a-table"></a>CTAS bir tabloyu kopyalama için kullanma
Belki de en yaygın birini kullanır `CTAS` DDL geçiş yapabilmeniz sağlayan bir tablonun kopyasını oluşturma. Örneğin, tablo olarak başlangıçta oluşturduysanız `ROUND_ROBIN` ve değiştirmek istediğiniz artık, dağıtılmış bir sütuna, tabloya `CTAS` nasıl dağıtım sütun değiştirme olduğu. `CTAS` Ayrıca bölümlendirme, dizin oluşturma veya sütun türlerini değiştirmek için kullanılabilir.

Bu tablonun varsayılan dağıtım türünü kullanarak oluşturduğunuz düşünelim `ROUND_ROBIN` hiçbir dağıtım sütun belirtilmediğinden dağıtılmış `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Şimdi bu tabloda yeni bir kopyasını, ile kümelenmiş Columnstore dizini oluşturun, böylece kümelenmiş Columnstore tabloları performansını yararlanabilir istiyorsunuz. Ayrıca bu sütunda birleştirmeler bekleme beri bu tabloda ProductKey dağıtmak istediğiniz ve ProductKey üzerinde birleştirme sırasında veri taşıma önlemek istiyor. Son olarak da eski bölümleri bırakarak, eski verileri hızlı bir şekilde silebilirsiniz böylece OrderDateKey üzerinde bölümleme eklemek istediğiniz. Burada, eski tablonuz yeni bir tabloya kopyalarsınız CTAS deyimi verilmiştir.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Son olarak, yeni tablonuzda değiştirme ve eski tablonuz bırakma için tablolarınız adlandırabilirsiniz.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [!NOTE]
> Azure SQL Data Warehouse henüz istatistiklerin otomatik olarak oluşturulup güncelleştirilmesini desteklemiyor.  Sorgularınızdan en iyi performansı elde edebilmeniz için ilk yüklemeden veya verilerdeki önemli değişikliklerden sonra her tablonun her sütununa ilişkin istatistiklerin oluşturulması önemlidir.  İstatistiklerin ayrıntılı bir açıklama konu geliştirme grubunu [İstatistikler] [istatistikleri] konusuna bakın.
> 
> 

## <a name="using-ctas-to-work-around-unsupported-features"></a>Desteklenmeyen özellikler etrafında çalışmak için CTAS kullanma
CTAS, aşağıda listelenen desteklenmeyen özellikler çeşitli geçici olarak çözmek için de kullanılabilir. Bu genellikle yalnızca kodunuzu uyumlu olacaktır, ancak bunu genellikle daha hızlı SQL Data Warehouse yürütecek win/win durum olması için kanıtlayabilirsiniz. Tam olarak parallelized tasarımını sonucunda budur. Geçici CTAS ile çalışılabilecek senaryolar şunlardır:

* ANSI BİRLEŞTİRMELER güncelleştirmeleri
* ANSI birleştirmeler üzerinde siler
* MERGE deyimi

> [!NOTE]
> Düşünmek deneyin "CTAS ilk". Kullanarak bir sorunu çözebilir düşünüyorsanız `CTAS` , ise genellikle en iyi yolu, - yaklaşımını daha fazla veri sonuç olarak yazıyorsanız bile.
> 
> 

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI birleştirme değiştirme için güncelleştirme deyimleri
Birlikte sözdizimi birleştirme ANSI UPDATE veya DELETE kullanarak gerçekleştirmek için ikiden fazla tabloyu birleştiren karmaşık bir güncelleştirmeye sahip bulabilirsiniz.

Bu tablo güncelleştirmeniz gerekiyordu düşünün:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(    [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,    [CalendarYear]                    SMALLINT        NOT NULL
,    [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Özgün sorgu şöyle bir şey attıktan:

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT    [EnglishProductCategoryName]
        ,        [CalendarYear]
        ,        SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

ANSI SQL veri ambarı desteklemediği bu yana birleştirmeleri `FROM` yan tümcesinde bir `UPDATE` deyimi, biraz değiştirmeden üzerinden bu kodu kopyalayamıyor.

Bir bileşimini kullanabilirsiniz bir `CTAS` ve bu kodu değiştirmek için örtük bir birleşim:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,        ISNULL(CAST([CalendarYear] AS SMALLINT),0)                         AS [CalendarYear]
,        ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                        AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,        [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI birleştirme yerini silme deyimleri
Bazen veri silmek için en iyi yaklaşımı kullanmaktır `CTAS`. Verileri yalnızca silme yerine korumak istediğiniz verileri seçin. Bu özellikle true `DELETE` ANSI SQL veri ambarı ANSI desteklemediğinden katılma sözdizimi birleştirir kullan deyimleri `FROM` yan tümcesinde bir `DELETE` deyimi.

Dönüştürülen DELETE deyimi örneği aşağıda kullanılabilir:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Merge deyimlerinde değiştirin
Merge deyimlerinde, en az bölümünde, CTAS kullanılarak değiştirilebilir. Tek bir deyimde ekleme ve güncelleştirme birleştirebilir. Silinmiş kayıtları Kapat'ı ikinci bir deyimde kapatılması gerekir.

Bir UPSERT örneği verilmiştir:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS öneri: açıkça durumunda veri türü ve çıktı olabilirliği
Kod geçirirken bu tür bir kodlama düzeni çalıştırmak bulabilirsiniz:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

İnstinctively CTAS için bu kodu geçirmeniz gerekir ve doğru olacaktır düşünebilirsiniz. Ancak, burada gizli bir sorun yoktur.

Aşağıdaki kod, aynı sonucu vermez:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

"Sonuç" sütun İleri ifade veri türü ve null atanabilirlik değerleri taşıyan dikkat edin. Dikkatli değilseniz, bu değerleri Zarif Varyanslar neden olabilir.

Örnek olarak aşağıdakileri deneyin:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Sonuç için saklanan değer farklıdır. Sonuç sütunu kalıcı değerinde diğer ifadelerinde kullanıldıkça hata daha önemli hale gelir.

![CTAS sonuçları](media/sql-data-warehouse-develop-ctas/ctas-results.png)

Bu veri geçişler için özellikle önemlidir. İkinci sorguyu tartışmaya açık bir şekilde daha doğru olsa bile bir sorun yoktur. Veri kaynağı sisteme karşılaştırıldığında farklı olacaktır ve bütünlüğü geçiş sorular, yol açar. Bu, "yanlış" yanıt gerçekten doğru olanı olduğu bu nadir durumlarda biridir!

Biz bu farklılığa arasında iki sonuçları görmek için örtük tür atama nedenidir. İlk örnekte tablonun sütun tanımı tanımlar. Satır eklendiğinde, bir örtük tür dönüştürme oluşur. İkinci örnekte yoktur örtük tür dönüştürme gibi ifade sütunun veri türünü tanımlar. Ayrıca ilk örnekte, henüz ancak ikinci örnek sütununda bir boş değer atanabilir sütun olarak tanımlanmış dikkat edin. Tablonun ilk örnek sütun null atanabilirlik içinde oluşturulduğu açıkça tanımlandı. İkinci örneği, yalnızca bir ifadeye ve varsayılan olarak bu bırakıldı NULL tanımında neden olur.  

Bu sorunları gidermek için açıkça CTAS deyiminin SELECT bölümünde null atanabilirlik ve tür dönüştürmeleri ayarlamanız gerekir. Create table bölümünde bu özellikleri ayarlanamıyor.

Aşağıdaki örnek kod gidermeye yönelik gösterir.

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Şunlara dikkat edin:

* CAST veya CONVERT kullanılan
* IsNull null Atanabilirlik olmayan birleşim zorlamak için kullanılır
* IsNull en dıştaki işlevidir
* Bir sabit IsNull ikinci parçası olan yani 0

> [!NOTE]
> IsNull ve değil birleşim kullanmak üzere doğru şekilde ayarlanması için null atanabilirlik önemlidir. BİRLEŞİM belirleyici bir işlev değil ve bu nedenle ifade sonucu null atanabilir her zaman olacaktır. IsNull farklıdır. Belirleyici. Bu nedenle zaman IsNull işlevi ikinci bölümü bir sabit ya da bir hazır değer sonra sonuçta elde edilen değer olacaktır NOT NULL.
> 
> 

Bu ipucu, hesaplamalar bütünlüğünü sağlamak için yalnızca kullanışlı değildir. Bölüm tablo geçmek için önemlidir. Bu tablo, olgu tanımlanan olduğunu düşünün:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Ancak, değer alanına veri kaynağını parçası olmayan bir hesaplanan ifadesidir.

Bölümlenmiş kümenizi oluşturmak için bunu yapmak isteyebilirsiniz:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Sorgu mükemmel iyi çalışır. Bölüm anahtarı gerçekleştirmeye çalıştığında sorun gelir. Tablo tanımları eşleşmiyor. CTAS eşleşen tablo tanımları yapmak için değiştirilmesi gerekir.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Bu nedenle, türü tutarlılık ve bir CTAS null atanabilirlik özellikleri koruma iyi bir mühendislik en iyi uygulama olduğunu görebilirsiniz. Bu, hesaplamalarda bütünlüğünü sağlamaya yardımcı olur ve ayrıca bölüm değiştirme mümkün olmasını sağlar.

Lütfen [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) belgeleri. Azure SQL Data Warehouse en önemli deyimlerinde biridir. İyice anladığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

