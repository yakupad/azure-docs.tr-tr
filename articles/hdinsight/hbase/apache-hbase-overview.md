---
title: Azure HDInsight'ta HBase nedir? | Microsoft Docs
description: Hadoop’ta oluşturulan bir NoSQL veritabanı olan HDInsight’ta Apache HBase’e giriş. Kullanım örnekleri hakkında bilgi edinin ve HBase’i diğer Hadoop kümeleriyle karşılaştırın.
keywords: bigtable,nosql,hbase nedir,apache hbase,hbase,habase genel bakış,
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: d2a76d53-133a-4849-a30c-88d9c794391c
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: jgao
ms.openlocfilehash: 36c6a16520d78d4fa2465a1fdea585a3b3c88cd7
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31520924"
---
# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>HDInsight’ta HBase nedir: Hadoop için BigTable benzeri özellikler sağlayan bir NoSQL veritabanıdır.
Apache HBase, Hadoop’ta oluşturulan ve Google BigTable’a göre modellenen açık kaynaklı bir NoSQL veritabanıdır. HBase, sütun aileleri tarafından veritabanında büyük miktarlardaki yapılandırılmamış ve yarı yapılandırılmış veriler için rasgele erişim ve güçlü tutarlılık sağlar.

Veriler bir tablonun satırlarında depolanır ve satır içindeki veriler sütun ailesi tarafından gruplandırılır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. Veri yedekleme, toplu işleme ve Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan diğer özelliklere dayanabilir.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Azure HDInsight’ta HBase nasıl uygulanır?
HDInsight HBase, Azure ortamına tümleştirilmiş yönetilen bir küme olarak sunulur. Kümeler verileri, düşük gecikme süresi ve performans ve maliyet seçeneklerinde artan esneklik sağlayan [Azure Depolama](./../hdinsight-hadoop-use-blob-storage.md) veya [Azure Data Lake Store](./../hdinsight-hadoop-use-data-lake-store.md)'da depolayacak şekilde yapılandırılmıştır. Bu, müşterilerin milyonlarca uç noktadan gelen algılayıcı ve telemetri verilerini depolayan hizmetleri oluşturmak ve bu verileri Hadoop işleriyle çözümlemek üzere etkileşimli web siteleri oluşturmalarını sağlar. HBase ve Hadoop Azure’da büyük veri projeleri için yi başlangıç noktalarıdır; bunlar özellikle büyük veri kümeleriyle çalışmak üzere gerçek zamanlı uygulamaları etkinleştirebilir.

HDInsight uygulaması, tabloların otomatik parçalanmasını, okumalar ve yazmalar için yüksek tutarlılık ve otomatik yük devretme sağlamak için HBase’in ölçek genişletmeli mimarisinden yararlanır. Performans, okumalar için bellek içi önbelleğe alma ve yazmalar için yüksek verimlilikli akış tarafından geliştirilmiştir. HBase kümesi sanal ağda oluşturulabilir. Ayrıntılar için bkz. [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Veriler HDInsight HBase’de nasıl yönetilir?
Veriler HBase kabuğunda `create`, `get`, `put` ve `scan` komutları kullanılarak HBase tarafından yönetilebilir. Veriler `put` kullanılarak veritabanına yazılır ve `get` kullanarak okunur. `scan` komutu, bir tablodaki birden çok satırdaki verileri almak için kullanılır. Veriler, HBase REST API’sinin üstünde bir istemci kitaplığı sağlayan HBase C# API’si kullanılarak da yönetilebilir. Bir HBase veritabanı, Hive kullanarak da sorgulanabilir. Bu programlama modellerine giriş için bkz. [HDInsight'ta Hadoop ile HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md). Veritabanı barındıran düğümlerde veri işlemeye olanak sağlayan ortak işlemciler de kullanılabilir.

> [!NOTE]
> Thrift, HDInsight’ta HBase tarafından desteklenmez.
>

## <a name="scenarios-use-cases-for-hbase"></a>Senaryolar: HBase kullanım örnekleri
BigTable’ın (ve uzantılarının, HBase) oluşturulma nedeni olan kurallı kullanım örneği amacı web aramasıydı. Arama motorları terimleri bunları içeren web siteleriyle eşleştiren dizinler oluşturur. Ancak HBase için uygun olan diğer birçok kullanım örneği vardır; bunların birkaçı bu bölümde listelenmektedir.

* Anahtar değeri deposu
  
    HBase bir anahtar değeri deposu olarak kullanılabilir ve ileti sistemlerini yönetmeye uygundur. Facebook kendi ileti sistemi için HBase kullanır ve Internet iletişimlerini depolamak ve yönetmek için idealdir. WebTable web sayfalarından çıkarılan tabloları aramak ve yönetmek için HBase kullanır.
* Algılayıcı verileri
  
    HBase çeşitli kaynaklardan artımlı olarak toplanan verileri yakalamak için yararlıdır. Bu, sosyal analizler, zaman serileri, etkileşimli panoları eğilimler ve sayaçlar ile güncel tutma ve denetim günlüğü sistemlerini yönetmeyi içerir. Örnek olarak, Bloomberg tüccar terminali ve sunucu sistemlerinin durumuna ilişkin toplanan ölçümleri toplayan ve bunlara erişim imkanı sağlayan Open Time Series Veritabanı (OpenTSDB) verilebilir.
* Gerçek zamanlı sorgu
  
    [Phoenix](http://phoenix.apache.org/) Apache HBase için bir SQL sorgu alt yapısıdır. Buna JDBC sürücüsü olarak erişilir ve bu SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar.
* Bir platform olarak HBase
  
    Uygulamalar, bir veri deposu olarak kullanarak HBase’in üstünde çalışabilir. Örnek olarak Phoenix, OpenTSDB, Kiji ve Titan verilebilir. Uygulamalar HBase ile de tümleştirebilir. Örnek olarak, Hive, Pig, Solr, Storm, Flume, Impala, Spark, Ganglia ve Dril verilebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [HDInsight'ta Hadoop ile HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md)
* [Azure Sanal Ağ'da HDInsight kümeleri oluşturma](./apache-hbase-provision-vnet.md)
* [HDInsight’ta HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
* [HDInsight ile HBase kullanan Java derlemeleri oluşturmak için Maven kullanma (Hadoop)](./apache-hbase-build-java-maven-linux.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Apache HBase](https://hbase.apache.org/)
* [Bigtable: Yapılandırılmış Veriler için Dağıtılmış Depolama Sistemi](http://research.google.com/archive/bigtable.html)




