---
title: Intellij ile Java ile bir Azure işlev uygulaması oluşturma | Microsoft Docs
description: Java ve Intellij için Azure işlevleri'ni kullanarak sunucusuz uygulama oluşturma ve basit bir HTTP yayımlama ile ilgili nasıl yapılır Kılavuzu tetiklenir.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: Azure işlevleri, İşlevler, olay işleme, işlem, sunucusuz mimari, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: e926bfb023fe3edfd564aa6389e21f6594bec169
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117507"
---
# <a name="create-your-first-function-with-java-and-intellij-preview"></a>Intellij (Önizleme) ve Java ile ilk işlevinizi oluşturma

> [!NOTE] 
> Azure İşlevleri için Java şu anda önizleme aşamasındadır.

Bu makalede nasıl oluşturulacağını gösterir bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) işlev projesi Intellij Idea ve Apache Maven ile test ve kendi bilgisayarınıza IDE'den hata ayıklama ve Azure işlevleri'ne dağıtın. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve Intellij ile bir işlev uygulaması geliştirmek için aşağıdakilerin yüklü olması gerekir:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
-  [Intellij Idea](https://www.jetbrains.com/idea/download), topluluk veya Ultimate tooling Maven ile.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

Ayrıca yüklemek için önerilen yüksek olduğu [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2), yazma, çalıştırma ve Azure işlevlerinde hata ayıklama için bir yerel geliştirme ortamı sağlar. 


## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. İçin Intellij Idea içinde tıklayın **yeni proje oluştur**.  
1. Oluşturmak için Seç **Maven**
1. Onay kutusunu seçin **archetype Oluştur** ve **ekleme Archetype** için [azure işlevleri archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - GroupID: com.microsoft.azure
    - Artifactıd: azure işlevleri archetype
    - Sürüm: Kullanımı en son sürümü [merkezi depo](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Intellij Maven oluşturma](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Tıklayın **sonraki** ve ayrıntılarını girin geçerli projenin ve sonunda **son**.

Maven, _artifactId_ adlı yeni bir dosyada proje dosyalarını oluşturur. Oluşturulan kod projesinde basit bir [HTTP ile tetiklenen](/azure/azure-functions/functions-bindings-http-webhook) tetikleme HTTP istek gövdesini yankılayan işlevi.

## <a name="run-functions-locally-in-the-ide"></a>İşlevleri IDE içinde yerel olarak çalıştırma

> [!NOTE]
> [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2) çalıştırın ve işlevleri yerel olarak hata ayıklama için yüklü olması gerekir.

1. Emin olun ya da değişiklikleri içeri seçin [otomatik olarak içeri aktarma](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html) olan etkinleştirir.
1. Açık **Maven projeleriyle** araç çubuğu
1. Yaşam döngüsü altında çift **paket** paketlemeyi ve Çözümü derleyin ve bir hedef dizin oluşturun.
1. Azure işlevleri çift eklentileri altında -> **azure-İşlevler: çalışma** azure işlevleri yerel çalışma zamanı'nı başlatmak için.  
  ![Azure işlevleri için maven araç çubuğu](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

İşiniz bittiğinde Çalıştır iletişim kutusunu kapatın, işlevinizi test etme. İşlevi yalnızca bir ana bilgisayar etkin olduğu ve çalıştırıldığı yerel olarak bir zaman olabilir.

### <a name="debug-the-function-in-intellij"></a>Intellij işlevde hata ayıklama

Başlatma işleminden sonra işlevi konağa ekleyerek Intellij işlevlerinde hata ayıklama yapabilirsiniz.  Yerel olarak yukarıda ve sonra da adımları kullanarak Azure işlevini çalıştırma **çalıştırma** menüsünü seçin **iliştirme yerel**.  Kullanılabilir 5005 bağlantı noktasına bir işlem görmeniz gerekir.  Ekledikten sonra isabet ve işlev uygulamanızı içinde hata ayıklama kesme noktaları olabilir.

İşiniz bittiğinde hata ayıklayıcı ve çalışan işlemi durdurun. Yalnızca bir işlev konak aynı anda etkin olduğu ve çalıştırıldığı yerel olarak en olabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. [Azure CLI ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bilgisayarınızın komut istemini kullanarak devam etmeden önce.

```azurecli
az login
```

Kodunuzu yeni işlev uygulama kullanarak bir dağıtın `azure-functions:deploy` Maven hedefini veya azure işlevleri seçin: Maven projeleriyle penceresinde seçeneği dağıtın.

```
mvn azure-functions:deploy
```

Dağıtım tamamlandığında, Azure işlev uygulamanıza erişmek için kullanabileceğiniz URL’yi görürsünüz:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Sonraki adımlar

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin.
