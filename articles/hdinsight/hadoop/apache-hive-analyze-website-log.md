---
title: Web sitesi günlüğü analizi - Azure Hdınsight Hadoop ile Hive kullanma | Microsoft Docs
description: Web sitesi günlüklerini çözümlemek için Hdınsight ile Hive kullanma öğrenin. Bir Hdınsight tabloya giriş olarak bir günlük dosyası kullanmak ve verileri sorgulamak için HiveQL kullanma.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 53a0560d3bc5a52069d5829b9c3bd353e0c37ef3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31398166"
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Web sitesi günlüklerini çözümlemek için Windows tabanlı Hdınsight ile Hive kullanma
Bir Web sitesi günlüklerini çözümlemek için Hdınsight ile HiveQL kullanmayı öğrenin. Web sitesi günlüğü analizi benzer etkinliklere dayalı kitlenizi segmentlere, site ziyaretçilerini demografisine göre kategorilere ayırmak ve bulmak için içeriği teslim bunlar görünümü, Web siteleri geliyor ve benzeri kullanılabilir.

> [!IMPORTANT]
> Bu belgede yer alan adımlar, yalnızca Windows tabanlı Hdınsight kümeleri ile çalışır. Hdınsight yalnızca Windows'da Hdınsight 3.4 ' düşük sürümleri için kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

Bu örnekte, Web sitesi günlük dosyaları ziyaretlerin sıklığını Web sitesine dış Web sitelerinden bir gün içinde hakkında bilgi edinme çözümlemek için Hdınsight kümesi kullanın. Ayrıca, kullanıcının karşılaştığı Web sitesi hatalarının özetini de oluşturur. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

* Web sitesinin günlük dosyalarını içeren bir Azure Blob Depolama birimine bağlayın.
* HIVE tablolarını Bu günlükleri sorgu oluşturun.
* Verileri çözümlemek için HIVE sorguları oluşturun.
* Hdınsight'a (açık veritabanı bağlantısı (ODBC) çözümlenen verileri almak üzere kullanarak. bağlanmak için Microsoft Excel kullanın

![HDI. Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>Önkoşullar
* Azure Hdınsight Hadoop kümesinde sağlanması gerekir. Yönergeler için bkz: [sağlama Hdınsight kümeleri](../hdinsight-hadoop-provision-linux-clusters.md).
* Excel 2010 yüklü veya Microsoft Excel 2013 yüklü olmalıdır.
* Bilmeniz gereken [Microsoft Hive ODBC sürücüsü](http://www.microsoft.com/download/details.aspx?id=40886) kovanından Excel'e veri almak için.

## <a name="to-run-the-sample"></a>Örneği çalıştırmak için
1. Gelen [Azure portal](https://portal.azure.com/), (küme var. sabitlenmiş varsa) Sabitle gelen istediğiniz örneği çalıştırmak küme kutucuğa tıklayın.
2. Küme dikey penceresinden altında **hızlı bağlantılar**, tıklatın **küme Panosu**ve sonra **küme Panosu** dikey penceresinde tıklatın **Hdınsight küme Panosu**. Alternatif olarak, aşağıdaki URL'yi kullanarak Pano doğrudan açabilirsiniz:

         https://<clustername>.azurehdinsight.net

    İstendiğinde, yönetici kullanıcı adını ve küme hazırlama sırasında kullanılan parola kullanarak kimlik doğrulaması.
3. Web açan sayfasından tıklatın **alma başlatıldı galeri** sekmesi ve ardından **örnek verilerle çözümleri** kategorisi, tıklatın **Web sitesi günlüğü analizi** örnek.
4. Örnek tamamlamak için web sayfasında sağlanan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örnek deneyin: [Hdınsight ile Hive kullanma algılayıcı verilerini çözümleme](apache-hive-analyze-sensor-data.md).

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
