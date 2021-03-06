---
title: Visual Studio için Azure Data Lake Araçları’nı yükleme
description: Bu makalede, Visual Studio için Azure Data Lake araçları yüklemeyi açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 05/03/2018
ms.openlocfilehash: c520c437212c23cc9dc8327c95b9f2a77b08e1ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34622879"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme.

Azure Data Lake Analytics hesapları oluşturmak, [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde işler tanımlamak ve Data Lake Analytics hizmetine iş göndermek için Visual Studio’nun nasıl kullanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* **Visual Studio**: Express dışında tüm sürümler desteklenir.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **.NET için Microsoft Azure SDK** 2.7.1 sürümü veya sonraki sürümleri.  [Web platformu yükleyicisini](http://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Data Lake Analytics** hesabı. Hesap oluşturmak için bkz. [Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Visual Studio 2017 için Azure Data Lake Araçları’nı yükleme

Visual Studio için Azure Data Lake Araçları, Visual Studio 2017 15.3 veya üzeri sürümlerde desteklenir. Araç, Visual Studio Yükleyicisi’ndeki **Veri depolama ve işleme** ve **Azure Geliştirme** iş yüklerinin parçasıdır. Visual Studio yüklemenizin parçası olarak bu iki iş yükünden birini etkinleştirin.  

![Veri depolama ve işleme iş yükünü etkinleştirme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png) bölümünde gösterildiği gibi **Veri depolama ve işleme** iş yükünü etkinleştirin

![Azure geliştirme iş yükünü etkinleştirme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png) bölümünde gösterildiği gibi **Azure geliştirme** iş yükünü etkinleştirin

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Visual Studio 2013 ve 2015 için Azure Data Lake Araçları’nı yükleme

[İndirme Merkezi'nden](http://aka.ms/adltoolsvs) Visual Studio için Azure Data Lake Araçları’nı indirip yükleyin. Yükleme işleminden sonra şunları kontrol edin:
* **Sunucu Gezgini** > **Azure** düğümü, **Data Lake Analytics** düğümü içerir. 
* **Araçlar** menüsünde **Data Lake** öğesi vardır.


## <a name="next-steps"></a>Sonraki Adımlar
* Tanılama bilgilerini günlüğe kaydetmek için bkz. [Azure Data Lake Analytics için tanılama günlüklerine erişme](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Köşe yürütme görünümü kullanmak için bkz: [köşe yürütme görünümü Data Lake araçları Visual Studio için kullanın.](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)
