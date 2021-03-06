---
title: T-SQL deyimlerini - Azure SQL Data Warehouse | Microsoft Docs
description: Azure SQL Data Warehouse'da desteklenen T-SQL deyimleri için belgelere bağlantılar.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: ec708f30b59bbd6af878720c65f9a464224c57a5
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="t-sql-statements-supported-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'da desteklenen T-SQL deyimleri
Azure SQL Data Warehouse'da desteklenen T-SQL deyimleri için belgelere bağlantılar.

## <a name="data-definition-language-ddl-statements"></a>Veri tanımlama dili (DDL) deyimleri
* [ALTER DATABASE](https://msdn.microsoft.com/library/mt204042.aspx)
* [ALTER INDEX](https://msdn.microsoft.com/library/ms188388.aspx)
* [DEĞİŞTİRME YORDAMI](https://msdn.microsoft.com/library/ms189762.aspx)
* [ALTER SCHEMA](https://msdn.microsoft.com/library/ms173423.aspx)
* [ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)
* [COLUMNSTORE DİZİNİ OLUŞTURMA](https://msdn.microsoft.com/library/gg492153.aspx)
* [CREATE DATABASE](https://msdn.microsoft.com/library/mt204021.aspx)
* [VERİTABANI KAPSAMLI OLUŞTURMAK KİMLİK BİLGİSİ](https://msdn.microsoft.com/library/mt270260.aspx)
* [DIŞ VERİ KAYNAĞI OLUŞTURUN](https://msdn.microsoft.com/library/dn935022.aspx)
* [CREATE EXTERNAL FILE FORMAT](https://msdn.microsoft.com/library/dn935026.aspx)
* [DIŞ TABLO OLUŞTURMA](https://msdn.microsoft.com/library/dn935021.aspx)
* [CREATE İŞLEVİ](https://msdn.microsoft.com/library/mt203952.aspx)
* [DİZİN OLUŞTURMA](https://msdn.microsoft.com/library/ms188783.aspx)
* [YORDAM OLUŞTURMA](https://msdn.microsoft.com/library/ms187926.aspx)
* [ŞEMA OLUŞTURUN](https://msdn.microsoft.com/library/ms189462.aspx)
* [CREATE STATISTICS](https://msdn.microsoft.com/library/ms188038.aspx)
* [CREATE TABLE](https://msdn.microsoft.com/library/mt203953.aspx)
* [CREATE TABLE AS SELECT](https://msdn.microsoft.com/library/mt204041.aspx)
* [GÖRÜNÜM OLUŞTUR](https://msdn.microsoft.com/library/ms187956.aspx)
* [BIRAKMA DIŞ VERİ KAYNAĞI](https://msdn.microsoft.com/library/mt146367.aspx)
* [DIŞ DOSYA BİÇİMİNİ BIRAK](https://msdn.microsoft.com/library/mt146379.aspx)
* [DIŞ TABLO BIRAKMA](https://msdn.microsoft.com/library/mt130698.aspx)
* [DROP INDEX](https://msdn.microsoft.com/library/ms176118.aspx)
* [BIRAKMA YORDAMI](https://msdn.microsoft.com/library/ms174969.aspx)
* [AÇILAN İSTATİSTİKLERİ](https://msdn.microsoft.com/library/ms175075.aspx)
* [TABLO BIRAKMA](https://msdn.microsoft.com/library/ms173790.aspx)
* [ŞEMAYI](https://msdn.microsoft.com/library/ms186751.aspx)
* [GÖRÜNÜMÜ](https://msdn.microsoft.com/library/ms173492.aspx)
* [YENİDEN ADLANDIR](https://msdn.microsoft.com/library/mt631611.aspx)
* [TRUNCATE TABLE](https://msdn.microsoft.com/library/ms177570.aspx)
* [UPDATE STATISTICS](https://msdn.microsoft.com/library/ms187348.aspx)

## <a name="data-manipulation-language-dml-statements"></a>Veri işleme dili (DML) deyimleri
* [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)
* [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)
* [GÜNCELLEŞTİRME](https://msdn.microsoft.com/library/ms177523.aspx)

## <a name="database-console-commands"></a>Veritabanı Konsolu komutları
* [DBCC DROPCLEANBUFFERS](https://msdn.microsoft.com/library/ms187762.aspx)
* [DBCC FREEPROCCACHE](https://msdn.microsoft.com/library/mt204018.aspx)
* [DBCC SHRINKLOG](https://msdn.microsoft.com/library/mt204020.aspx)
* [DBCC PDW_SHOWEXECUTIONPLAN](https://msdn.microsoft.com/library/mt204017.aspx)
* [DBCC PDW_SHOWPARTITIONSTATS](https://msdn.microsoft.com/library/mt204013.aspx)
* [DBCC PDW_SHOWSPACEUSED](https://msdn.microsoft.com/library/mt204028.aspx)
* [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/mt204043.aspx)

## <a name="query-statements"></a>Sorgu ifadeleri
* [SEÇİN](https://msdn.microsoft.com/library/ms189499.aspx)
* [Common_table_expression ile](https://msdn.microsoft.com/library/ms175972.aspx)
* [DIŞINDA ve KESİŞTİĞİ](https://msdn.microsoft.com/library/ms188055.aspx)
* [AÇIKLANMAKTADIR](https://msdn.microsoft.com/library/mt631615.aspx)
* [KAYNAK](https://msdn.microsoft.com/library/ms177634.aspx)
* [Özet ve UNPIVOT kullanma](https://msdn.microsoft.com/library/ms177410.aspx)
* [GRUPLANDIRMA ÖLÇÜTÜ](https://msdn.microsoft.com/library/ms177673.aspx)
* [SAHİP](https://msdn.microsoft.com/library/ms180199.aspx)
* [SIRALAMA ÖLÇÜTÜ](https://msdn.microsoft.com/library/ms188385.aspx)
* [OPTION](https://msdn.microsoft.com/library/ms190322.aspx)
* [BİRLEŞİM](https://msdn.microsoft.com/library/ms180026.aspx)
* [BURADA](https://msdn.microsoft.com/library/ms188047.aspx)
* [SAYFANIN ÜSTÜ](https://msdn.microsoft.com/library/ms189463.aspx)
* [Diğer ad](https://msdn.microsoft.com/library/mt631614.aspx)
* [Arama koşulu](https://msdn.microsoft.com/library/ms173545.aspx)
* [Alt sorgular](https://msdn.microsoft.com/library/mt631613.aspx)

## <a name="security-statements"></a>Güvenlik deyimleri
* İzinleri: [GRANT](https://msdn.microsoft.com/library/ms187965.aspx), [REDDETME](https://msdn.microsoft.com/library/ms188338.aspx), [iptal etme](https://msdn.microsoft.com/library/ms187728.aspx)
* [ALTER YETKİLENDİRME](https://msdn.microsoft.com/library/ms187359.aspx)
* [SERTİFİKA DEĞİŞTİRME](https://msdn.microsoft.com/library/ms189511.aspx)
* [ALTER VERİTABANI ŞİFRELEME ANAHTARI](https://msdn.microsoft.com/library/bb630389.aspx)
* [ALTER OTURUM AÇMA](https://msdn.microsoft.com/library/ms189828.aspx)
* [ANA ANAHTAR ALTER](https://msdn.microsoft.com/library/ms186937.aspx)
* [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx)
* [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)
* [YEDEKLEME SERTİFİKA](https://msdn.microsoft.com/library/ms178578.aspx)
* [ANA ANAHTAR KAPATIN](https://msdn.microsoft.com/library/ms188387.aspx)
* [SERTİFİKA OLUŞTURMA](https://msdn.microsoft.com/library/ms187798.aspx)
* [VERİTABANI ŞİFRELEME ANAHTARI OLUŞTUR](https://msdn.microsoft.com/library/bb677241.aspx)
* [OTURUM AÇMA OLUŞTURUN](https://msdn.microsoft.com/library/ms189751.aspx)
* [ANA ANAHTAR OLUŞTURMA](https://msdn.microsoft.com/library/ms174382.aspx)
* [ROLÜ OLUŞTURMA](https://msdn.microsoft.com/library/ms187936.aspx)
* [KULLANICI OLUŞTUR](https://msdn.microsoft.com/library/ms173463.aspx)
* [SERTİFİKA BIRAK](https://msdn.microsoft.com/library/ms179906.aspx)
* [DROP DATABASE ENCRYPTION KEY](https://msdn.microsoft.com/library/bb630256.aspx)
* [OTURUM AÇMA BIRAK](https://msdn.microsoft.com/library/ms188012.aspx)
* [ANA ANAHTAR BIRAK](https://msdn.microsoft.com/library/ms180071.aspx)
* [ROL BIRAK](https://msdn.microsoft.com/library/ms174988.aspx)
* [KULLANICIYI BIRAKIN](https://msdn.microsoft.com/library/ms189438.aspx)
* [ANA ANAHTARI AÇILAMIYOR](https://msdn.microsoft.com/library/ms174433.aspx)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgileri için bkz: [T-SQL dil öğeleri Azure SQL Data warehouse'da](sql-data-warehouse-reference-tsql-language-elements.md), ve [Azure SQL veri ambarı sistem görünümleri](sql-data-warehouse-reference-tsql-system-views.md).
