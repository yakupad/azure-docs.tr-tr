---
title: Azure Veri Kopyalama aracını kullanarak veri kopyalama | Microsoft Docs
description: Azure Blob depolama alanındaki bir konumda bulunan verileri başka bir konuma kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 4df392ec7e100ef0efcbb3876079710a6b9ca4fb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38623544"
---
# <a name="use-the-copy-data-tool-to-copy-data"></a>Veri Kopyalama aracını kullanarak veri kopyalama 
> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](quickstart-create-data-factory-copy-data-tool.md)

Bu hızlı başlangıçta Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak Azure Blob depolama alanındaki bir klasörde bulunan verileri başka bir klasöre kopyalarsınız. 

> [!NOTE]
> Azure Data Factory'yi kullanmaya yeni başlıyorsanız, bu hızlı başlangıçtaki işlemleri gerçekleştirmeden önce [Azure Data Factory'ye giriş](data-factory-introduction.md) konusuna bakın. 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden **Yeni**’yi, sonra **Veri ve Analiz**’i ve ardından **Data Factory**’i seçin. 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **Ad** için **ADFTutorialDataFactory** girin. 
      
   ![“Yeni veri fabrikası” sayfası](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin **&lt; adınız&gt;ADFTutorialDataFactory**) ve veri fabrikası oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - adlandırma kuralları](naming-rules.md) makalesini inceleyin.
  
   ![Bir ad kullanılamadığında alınan hata](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. **Abonelik** için, veri fabrikasını oluşturmak istediğiniz Azure aboneliğini seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı ve ardından listeden var olan bir kaynak grubunu seçin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
   Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** bölümünde **V2**'yi seçin.
5. **Konum** için, veri fabrikasının konumunu seçin. 

   Bu listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları (Azure Depolama ve Azure SQL Veritabanı gibi) ve işlemler (Azure HDInsight gibi) başka konumlarda veya bölgelerde olabilir.

6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’u seçin.
8. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz: 

    ![“Data Factory Dağıtılıyor” kutucuğu](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, **Data Factory** sayfasını görürsünüz. Azure Data Factory kullanıcı arabirimi (UI) uygulamasını ayrı bir sekmede başlatmak için **Yazar ve İzleyici** kutucuğunu seçin.
   
   ![Veri fabrikasının “Yazar ve İzleyici” kutucuğuna sahip ana sayfası](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Veri Kopyalama aracını başlatma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını başlatmak için **Veri Kopyala** kutucuğunu seçin. 

   ![“Veri kopyala” kutucuğu](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)

2. Veri Kopyalama aracının **Özellikler** sayfasında işlem hattı için bir ad ve açıklama belirtip **İleri**'yi seçebilirsiniz. 

   ![“Özellikler” sayfası](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında aşağıdaki adımları tamamlayın:

    a. Bağlantı eklemek için **+ Yeni bağlantı oluştur**'a tıklayın.

    ![“Kaynak veri deposu” sayfası](./media/quickstart-create-data-factory-copy-data-tool/new-source-linked-service.png)

    b. Galeriden **Azure Blob Depolama**'yı ve ardından **İleri**'yi seçin.

    ![Galeriden blob depolamayı seçin](./media/quickstart-create-data-factory-copy-data-tool/select-blob-source.png)

    c. **Azure Blob depolama hesabı belirtin** sayfasında, **Depolama hesabı adı** listesinden depolama hesabınızı ve sonra **Son**’u seçin. 

   ![Azure Blob depolama hesabını yapılandırın](./media/quickstart-create-data-factory-copy-data-tool/configure-blob-storage.png)

   d. Kaynak olarak yeni oluşturulan bağlantılı hizmeti seçin ve **İleri**'ye tıklayın.

   ![Kaynak olarak bağlantılı hizmeti seçin](./media/quickstart-create-data-factory-copy-data-tool/select-source-linked-service.png)


4. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:

   a. **Göz at**'a tıklayarak **adftutorial/input** klasörüne gidin, **emp.txt** dosyasını seçin ve **Seç**'e tıklayın. 

   ![“Girdi dosyası veya klasörü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/configure-source-path.png)

   d. Dosyayı olduğu gibi kopyalamak için **İkili kopya**'a tıklayıp **İleri**'yi seçin. 

   ![“Girdi dosyası veya klasörü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/select-binary-copy.png)


5. **Hedef veri deposu** sayfasında yeni oluşturduğunuz bağlantılı **Azure Blob Depolama** hizmetini ve sonra **İleri**'yi seçin. 

   ![“Hedef veri deposu” sayfası](./media/quickstart-create-data-factory-copy-data-tool/select-sink-linked-service.png)

6. **Çıkış dosyasını veya klasörünü seçin** sayfasında klasör yolu olarak **adftutorial/output** girip **İleri**'yi seçin. 

   ![“Çıktı dosyasını veya klasörünü seçin” sayfası](./media/quickstart-create-data-factory-copy-data-tool/configure-sink-path.png) 

7. **Ayarlar** sayfasında varsayılan yapılandırmaları kullanmak için **İleri**'yi seçin. 

8. **Özet** sayfasında tüm ayarları gözden geçirin ve **İleri**'yi seçin. 

    ![“Özet” sayfası](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)

9. **Dağıtım tamamlandı** sayfasında, oluşturduğunuz işlem hattını izlemek için **İzleyici**’yi seçin. 

    ![“Dağıtım tamamlandı” sayfası](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)

10. Uygulama **İzleyici** sekmesine geçer. Bu sekmede işlem hattının durumunu görürsünüz. Listeyi yenilemek için **Yenile**’yi seçin. 
    
    ![İşlem hattı çalıştırmasını izleme](./media/quickstart-create-data-factory-copy-data-tool/pipeline-monitoring.png)

11. **Eylemler** sütununda **Etkinlik Çalıştırma İşlemlerini Görüntüle**’yi seçin. İşlem hattı, **Kopyalama** türünde tek bir etkinlik içerir. 

    ![Etkinlik çalıştırmasını izleme](./media/quickstart-create-data-factory-copy-data-tool/activity-monitoring.png)
    
12. Kopyalama işlemiyle ilgili ayrıntıları görüntülemek için **Eylemler** sütunundaki **Ayrıntılar**’ı (gözlük resmi) seçin. Özelliklerle ilgili ayrıntılar için bkz. [Kopyalama Etkinliğine genel bakış](copy-activity-overview.md).

    ![İşlem ayrıntılarını kopyalama](./media/quickstart-create-data-factory-copy-data-tool/activity-execution-details.png)

13. **adftutorial** kapsayıcısının **çıktı** klasöründe **emp.txt** dosyasının oluşturulduğunu doğrulayın. Çıkış klasörü yoksa, Data Factory hizmeti tarafından otomatik olarak oluşturulur. 

14. Bağlantılı hizmetleri, veri kümelerini ve işlem hatlarını düzenlemek için sol panelde **İzleyici** sekmesinin üzerindeki **Yazar** sekmesine geçin. Bunları Data Factory kullanıcı arabiriminde düzenleme hakkında bilgi edinmek için bkz. [Azure portalını kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. Data Factory’yi daha fazla senaryoda kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-portal.md) okuyun. 