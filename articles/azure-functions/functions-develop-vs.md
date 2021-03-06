---
title: Visual Studio kullanarak Azure işlevleri geliştirme | Microsoft Docs
description: Geliştirin ve Visual Studio 2017 için Azure işlevleri araçları kullanarak Azure işlevlerini test etme hakkında bilgi edinin.
services: functions
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: glenga
ms.openlocfilehash: 318a39e244f0fca3a1b2d8531dd9197a15400e02
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205366"
---
# <a name="develop-azure-functions-using-visual-studio"></a>Visual Studio kullanarak Azure işlevleri geliştirme  

Visual Studio 2017 için Azure işlevleri araçları, geliştirme, test etme ve C# işlevlerini Azure'a dağıtma sayesinde Visual Studio uzantısıdır. Bu deneyim, Azure işlevleri ile ilk ise, daha fazla bilgi edinebilirsiniz [Azure işlevleri giriş](functions-overview.md).

Azure işlevleri araçları aşağıdaki avantajları sağlar: 

* Düzenleme, derleme ve yerel geliştirme bilgisayarınızda işlevleri çalıştırın. 
* Azure işlevleri projenizi doğrudan Azure'da yayımlayabilirsiniz. 
* WebJobs öznitelikleri, bağlama tanımları için ayrı bir function.json yerine doğrudan C# kodunda işlev bağlamaları bildirmek için kullanın.
* Geliştirin ve önceden derlenmiş C# işlevlerini dağıtın. Önceden derlenmiş işlevleri C# betiği tabanlı İşlevler'den daha iyi soğuk başlangıç performans sağlar. 
* Tüm Visual Studio geliştirme avantajları yaparken işlevlerinizi C# kod. 

Bu makalede, C# dilinde işlev geliştirme için Visual Studio 2017 için Azure işlevleri araçları kullanmayı gösterir. Ayrıca bir .NET bütünleştirilmiş kodu olarak azure'a projenizi yayımlamak öğrenin.

> [!IMPORTANT]
> Yerel geliştirme portalı geliştirme aynı işlev uygulaması ile karıştırmayın. Bir işlev uygulaması için yerel bir projeden yayımladığınızda, dağıtım işlemi portalda geliştirilen tüm işlevleri üzerine yazar.

## <a name="prerequisites"></a>Önkoşullar

Azure işlevleri araçları Azure geliştirme iş yükü dahil [Visual Studio 2017 sürüm 15.5](https://www.visualstudio.com/vs/), veya sonraki bir sürümü. Dahil olduğundan emin olun **Azure geliştirme** iş yükünde, Visual Studio 2017'yi yükleme:

![Azure geliştirme iş yüküyle Visual Studio 2017’yi yükleyin](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Visual Studio güncel olduğunu ve kullandığınız emin [en son sürüm](#check-your-tools-version) Azure işlevleri araçları.

### <a name="other-requirements"></a>Diğer gereksinimler

Oluşturma ve dağıtma işlevleri için de gerekir:

* Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa, [ücretsiz hesaplar](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) kullanılabilir.

* Azure Depolama hesabı. Bir depolama hesabı oluşturmak için bkz: [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

### <a name="check-your-tools-version"></a>Araçlar sürümünüzü kontrol edin

1. Gelen **Araçları** menüsünde seçin **Uzantılar ve güncelleştirmeler**. Genişletin **yüklü** > **Araçları** ve **Azure işlevleri ve Web işleri Araçları**.

    ![İşlevleri araçları sürümünü doğrula](./media/functions-develop-vs/functions-vstools-check-functions-tools.png)

2. Yüklü Not **sürüm**. Bu sürümü listelenen en son sürümle karşılaştırabilirsiniz [sürüm notlarında](https://github.com/Azure/Azure-Functions/blob/master/VS-AzureTools-ReleaseNotes.md). 

3. Sürümünüzün eski ise, Visual Studio Araçları aşağıdaki bölümde gösterildiği gibi güncelleştirin.

### <a name="update-your-tools"></a>Sizin araçlarınız güncelleştir

1. İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda Genişlet **güncelleştirmeleri** > **Visual Studio Market**, seçin **Azure işlevleri ve Web işleri araçları**  seçip **güncelleştirme**.

    ![Güncelleştirme işlevleri Araçlar sürümü](./media/functions-develop-vs/functions-vstools-update-functions-tools.png)   

2. Araçları güncelleştirme yüklendikten sonra Visual Studio Araçları VSIX Yükleyicisi'ni kullanarak güncelleştirme tetikleyiciye kapatın.

3. Yükleyicide seçin **Tamam** başlatmak için ve ardından **Değiştir** araçları güncelleştirmek için. 

4. Güncelleştirme tamamlandıktan sonra seçin **Kapat** ve Visual Studio'yu yeniden başlatın.

## <a name="create-an-azure-functions-project"></a>Azure işlevleri projesi oluşturma

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]

Proje şablonu, bir C# projesi oluşturur, yükler `Microsoft.NET.Sdk.Functions` NuGet paketi ve hedef Framework'ü ayarlar. .NET Framework 1.x hedefleyen çalışır ve .NET Standard 2.x hedefleri işlevleri. Yeni Proje aşağıdaki dosyaları içerir:

* **Host.JSON**: işlevleri konak yapılandırmanıza olanak sağlar. Bu ayarlar hem de yerel olarak ve azure'da çalışırken geçerlidir. Daha fazla bilgi için [host.json başvurusu](functions-host-json.md).

* **Local.Settings.JSON**: işlevleri yerel olarak çalıştırırken kullanılan ayarları bulundurur. Bu ayarlar, Azure tarafından kullanılmaz, tarafından kullanılan [Azure işlevleri çekirdek Araçları](functions-run-local.md). Bu dosya, işlevleriniz tarafından gereken değişkenleri uygulama ayarlarını belirtmek için kullanın. Yeni bir öğe ekleme **değerleri** projenizdeki işlevleri bağlamaları gerektirdiği her bağlantı için bir dizi. Daha fazla bilgi için [yerel ayarları dosyası](functions-run-local.md#local-settings-file) Azure işlevleri çekirdek araçları makaledeki.

Daha fazla bilgi için [işlevleri sınıf kitaplığı projesi](functions-dotnet-class-library.md#functions-class-library-project).

## <a name="configure-the-project-for-local-development"></a>Yerel geliştirme için proje yapılandırma

İşlevler çalışma zamanı, dahili bir Azure depolama hesabı kullanır. Tüm HTTP ve Web kancaları dışındaki türler tetiklemek için ayarlamanız gerekir **Values.AzureWebJobsStorage** geçerli bir Azure depolama hesabı bağlantı dizesi için anahtar. İşlev uygulamanızı kullanabilirsiniz [Azure storage öykünücüsü](../storage/common/storage-use-emulator.md) için **AzureWebJobsStorage** bağlantı ayarının, proje tarafından gerekli. Öykünücü kullanmak için değerini ayarlamak **AzureWebJobsStorage** için `UseDevelopmentStorage=true`. Dağıtımdan önce bir gerçek depolama bağlantısı için bu ayarı değiştirmelisiniz.

Depolama hesabı bağlantı dizesi ayarlamak için:

1. Visual Studio'da açın **Cloud Explorer**, genişletme **depolama hesabı** > **depolama hesabınızı**, ardından **özellikleri**ve kopyalama **PRIMARY CONNECTION Strıng'i** değeri.

2. Projenizdeki local.settings.json dosyasını açın ve değerini ayarlama **AzureWebJobsStorage** anahtar bağlantı dizesine kopyalanır.

3. Benzersiz anahtarlar için eklemek için önceki adımı yineleyin **değerleri** işlevleriniz tarafından gerekli herhangi bir bağlantı için bir dizi.

## <a name="create-a-function"></a>İşlev oluşturma

Önceden derlenmiş işlevleri'nde kod öznitelikleri uygulayarak işlev tarafından kullanılan bağlamaları tanımlanır. İşlevlerinizi sağlanan şablonları oluşturmak için Azure işlevleri Araçları'nı kullandığınızda, bu öznitelikler uygulanır. 

1. **Çözüm Gezgini**’nde, proje düğümünüze sağ tıklayın ve **Yeni** > **Öğe Ekle**’yi seçin. Seçin **Azure işlevi**, tür a **adı** sınıfı ve tıklatın **Ekle**.

2. Tetikleyici seçin, bağlama özellikleri ayarlayın ve tıklayın **Oluştur**. Aşağıdaki örnek oluşturma kuyruk depolama ile tetiklenen işlev ayarları gösterilir. 

    ![Kuyruk ile tetiklenen bir işlev oluşturma](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)

    Bu tetikleyici örnek adlı bir anahtar ile bir bağlantı dizesi kullanır **QueueStorage**. Bu bağlantı dizesi ayarı tanımlanmalıdır [local.settings.json dosyasında](functions-run-local.md#local-settings-file).

3. Yeni eklenen sınıfı inceleyin. Statik gördüğünüz **çalıştırma** ile öznitelendirilen bir yöntem **FunctionName** özniteliği. Bu öznitelik, yöntem işlevi için giriş noktası olduğunu gösterir.

    Örneğin, aşağıdaki C# sınıfı, temel bir kuyruk depolama ile tetiklenen işlevin temsil eder:

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;

    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    }
    ````
    Bağlama özgü öznitelik giriş noktası yöntemi için sağlanan her bağlama parametresi uygulanır. Öznitelik parametre olarak bağlama bilgilerini alır. Önceki örnekte, ilk parametresinin bir **QueueTrigger** kuyruk ile tetiklenen işlev belirten özniteliği uygulandı,. Kuyruk adı ve bağlantı dizesi ayarı adı için parametre olarak geçirilen **QueueTrigger** özniteliği. Daha fazla bilgi için [Azure işlevleri için Azure kuyruk depolama bağlamaları](functions-bindings-storage-queue.md#trigger---c-example).
    
Daha fazla işlev, işlev uygulaması projenizi eklemek için yukarıdaki yordamı kullanabilirsiniz. Projedeki her işlevin farklı bir tetikleyici olabilir ancak bir işlev tam olarak bir tetikleyici olmalıdır. Daha fazla bilgi için [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

## <a name="add-bindings"></a>Bağlama Ekle

Tetikleyicilerle olduğu gibi giriş ve çıkış bağlamaları işlevinize bağlama özniteliklerini olarak eklenir. Bağlamaları şekilde işleve ekleyin:

1. Olduğundan emin olun [proje yerel geliştirme için yapılandırılmış](#configure-the-project-for-local-development).

2. Özel bağlama için uygun NuGet uzantısı paketini ekleyin. Daha fazla bilgi için [Visual Studio kullanarak yerel C# geliştirme](functions-triggers-bindings.md#local-csharp) Tetikleyicileri ve bağlamaları makaledeki. Bağlama özgü NuGet paket gereksinimleri, bağlama için başvuru makalesinde bulunur. Örneğin, paket gereksinimleri için Event Hubs tetikleyici bulma [Event Hubs bağlama başvurusu makalesinde](functions-bindings-event-hubs.md).

3. Bağlama gerektiren uygulama ayarları varsa, bunları Ekle **değerleri** koleksiyonda [yerel ayar dosyası](functions-run-local.md#local-settings-file). İşlevi yerel olarak çalıştığında, bu değerler kullanılır. Azure işlev uygulaması, işlev çalıştığında [işlev uygulaması ayarları](#function-app-settings) kullanılır.

4. Yöntem imzası için uygun bağlama özniteliğini ekleyin. Aşağıdaki örnekte, bir kuyruk iletisi işlevi tetikler ve çıkış bağlaması aynı metni farklı bir sırada yeni bir kuyruk iletisi oluşturur.

    ```csharp
    public static class SimpleExampleWithOutput
    {
        [FunctionName("CopyQueueMessage")]
        public static void Run(
            [QueueTrigger("myqueue-items-source", Connection = "AzureWebJobsStorage")] string myQueueItem, 
            [Queue("myqueue-items-destination", Connection = "AzureWebJobsStorage")] out string myQueueItemCopy,
            TraceWriter log)
        {
            log.Info($"CopyQueueMessage function processed: {myQueueItem}");
            myQueueItemCopy = myQueueItem;
        }
    }
    ```
Kuyruk depolama bağlantısı elde edilen `AzureWebJobsStorage` ayarı. Daha fazla bilgi için özel bağlama için başvuru makalesine bakın. 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="testing-functions"></a>İşlevleri test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin. Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

Çalışan proje ile dağıtılan işlevi test ettiğiniz gibi kodunuzu test edebilirsiniz. Daha fazla bilgi için [kodunuzu Azure işlevleri'nde test stratejileri](functions-test-a-function.md). Hata ayıklama modunda çalışırken, kesme noktaları Visual Studio'da beklendiği gibi ulaşıldığından. 

Kuyruk ile tetiklenen işlevi test etmek nasıl bir örnek için bkz [kuyruk ile tetiklenen işlev hızlı başlangıç öğreticisinde](functions-create-storage-queue-triggered-function.md#test-the-function).  

Azure işlevleri çekirdek araçları kullanma hakkında daha fazla bilgi edinmek için [kod ve Azure işlevleri yerel olarak test](functions-run-local.md).

## <a name="publish-to-azure"></a>Azure’da Yayımlama

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="function-app-settings"></a>İşlev uygulaması ayarları

Azure işlev uygulaması da local.settings.json içinde eklenen herhangi bir ayarı eklenmesi gerekir. Proje yayımladığınızda, bu ayarlar otomatik olarak karşıya yüklenmemiş.

İşlev uygulamanızda Azure gerekli ayarları yüklemek için en kolay yolu kullanmaktır **uygulama ayarlarını yönet...**  projenizi başarıyla yayımladıktan sonra görüntülenen bağlantı. 

![](./media/functions-develop-vs/functions-vstools-app-settings.png)

Bu görüntüler **uygulama ayarları** iletişim kutusu için işlev uygulaması, yeni uygulama ayarlarını ekleyin veya var olanları düzenleyin.

![](./media/functions-develop-vs/functions-vstools-app-settings2.png)

Ayrıca şu diğer yöntemlerden birini kullanarak uygulama ayarları yönetebilirsiniz:

* [Azure portalını kullanarak](functions-how-to-use-azure-function-app-settings.md#settings).
* [Kullanarak `--publish-local-settings` Azure işlevleri çekirdek araçları seçeneği yayımlama](functions-run-local.md#publish).
* [Azure CLI kullanarak](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set). 

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri araçları hakkında daha fazla bilgi için bkz: sık sorulan sorular bölümünü [Azure işlevleri için Visual Studio 2017 Araçları](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog gönderisi.

Azure işlevleri çekirdek araçları hakkında daha fazla bilgi için bkz: [kod ve Azure işlevleri yerel olarak test](functions-run-local.md).

.NET sınıf kitaplıkları olarak işlevleri geliştirme hakkında daha fazla bilgi için bkz. [Azure işlevleri C# Geliştirici Başvurusu](functions-dotnet-class-library.md). Bu makalede ayrıca bağlamaları Azure işlevleri tarafından desteklenen çeşitli türlerde bildirmek için öznitelikleri kullanma örnekleri bağlantılarını içerir.    
