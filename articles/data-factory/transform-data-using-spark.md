---
title: Spark etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs
description: Spark Spark etkinliğini kullanarak bir Azure data factory işlem hattı çalışan programlar tarafından veri dönüştürme öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: douglasl
ms.openlocfilehash: abe2fabc505f94f19d4b15a406fc59bf6d6e7ac1
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050344"
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Spark etkinliği Azure Data Factory kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-spark.md)
> * [Geçerli sürüm](transform-data-using-spark.md)

Veri Fabrikası Spark etkinliğinde [ardışık düzen](concepts-pipelines-activities.md) üzerinde bir Spark programı yürütür [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Hdınsight kümesi. Bu makalede derlemeler [veri dönüştürme etkinlikleri](transform-data.md) makalesi, veri dönüştürme ve desteklenen dönüştürme etkinliklerinin genel bir bakış sunar. Bir isteğe bağlı Spark hizmeti kullandığınızda, veri fabrikası bir Spark kümesi, yalnızca verileri işlemek zaman için otomatik olarak oluşturur ve işlem tamamlandıktan sonra kümeyi siler. 

> [!IMPORTANT]
> Spark etkinliği Azure Data Lake Store birincil depolama alanı olarak kullanın Hdınsight Spark kümeleri desteklemez.

## <a name="spark-activity-properties"></a>Spark etkinlik özellikleri
Spark etkinlik örnek JSON tanımını şöyledir:    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark\\pyFiles",
        "entryFilePath": "test.py",
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

| Özellik              | Açıklama                              | Gerekli |
| --------------------- | ---------------------------------------- | -------- |
| ad                  | İşlem hattında etkinlik adı.    | Evet      |
| açıklama           | Etkinlik yaptığı açıklayan metin.  | Hayır       |
| type                  | Spark etkinliği için etkinlik HDInsightSpark türüdür. | Evet      |
| linkedServiceName     | Hdınsight Spark bağlı Spark programın çalıştığı hizmetin adı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| SparkJobLinkedService | Azure Storage bağlı Spark iş dosyası, bağımlılıklar ve günlükleri tutan hizmeti.  Bu özellik için bir değer belirtmezseniz, Hdınsight kümesi ile ilişkili depolama kullanılır. Bu özelliğin değeri, yalnızca Azure depolama bağlı hizmeti olabilir. | Hayır       |
| rootPath              | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harf duyarlıdır. Klasör yapısı bakın (sonraki bölümde) Bu klasör yapısı hakkında ayrıntılar için bölüm. | Evet      |
| entryFilePath         | Spark kod/paketi kök klasörüne göreli yolu. Girdi dosyası Python dosyası veya .jar dosyası olmalıdır. | Evet      |
| className             | Uygulamanın Java/Spark ana sınıfı      | Hayır       |
| bağımsız değişkenler             | Spark programın komut satırı bağımsız değişkenleri listesi. | Hayır       |
| proxyUser             | Spark program yürütülmeye kimliğine bürünmek için kullanıcı hesabı | Hayır       |
| sparkConfig           | Konu başlığı altında listelenen Spark yapılandırma özellikleri için değerleri belirtin: [Spark yapılandırması - uygulama özellikleri](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Hayır       |
| Getdebugınfo          | Hdınsight küme tarafından kullanılan Azure depolama Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır       |

## <a name="folder-structure"></a>Klasör yapısı
Spark işlerinin Pig/Hive işleri daha fazla genişletilebilir. Spark işleri, birden çok bağımlılıkları gibi sağlayabilirsiniz jar (java sınıf yerleştirilen) paketleri, python dosyaları (PYTHONPATH yerleştirilmiş) ve diğer dosyaları.

Hdınsight bağlı hizmeti tarafından başvurulan Azure Blob Depolama alanında aşağıdaki klasör yapısını oluşturun. Ardından, bağımlı dosyaları tarafından temsil edilen kök klasöründe uygun alt klasörlerdeki karşıya **entryFilePath**. Örneğin, python dosyalarını pyFiles alt klasöre ve jar dosyalarını kök klasörünün Kavanoz alt karşıya yükleyin. Çalışma zamanında Data Factory hizmeti Azure Blob depolama alanına aşağıdaki klasör yapısını bekler:     

| Yol                  | Açıklama                              | Gerekli | Tür   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.` (kök)            | Depolama bağlantılı hizmeti Spark işinde kök yolu | Evet      | Klasör |
| &lt;Kullanıcı tanımlı &gt; | Spark iş girişi dosyasına işaret eden yolu | Evet      | Dosya   |
| . / jar'lar                | Bu klasörü altındaki tüm dosyaları karşıya ve küme java sınıf yolu yerleştirilmiş | Hayır       | Klasör |
| . / pyFiles             | Bu klasörü altındaki tüm dosyaları karşıya ve küme PYTHONPATH yerleştirilmiş | Hayır       | Klasör |
| . / dosyaları               | Bu klasörü altındaki tüm dosyaları karşıya ve yürütücü çalışma dizini yerleştirilmiş | Hayır       | Klasör |
| . / arşivler            | Bu klasörü altındaki tüm dosyaları sıkıştırılmamış | Hayır       | Klasör |
| . / günlükleri                | Spark kümesi günlükleri içeren klasör. | Hayır       | Klasör |

Burada, Azure Blob Depolama Hdınsight bağlı hizmeti tarafından başvurulan iki Spark iş dosyalarını içeren bir depolama alanı için bir örnek verilmiştir.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning toplu iş yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
