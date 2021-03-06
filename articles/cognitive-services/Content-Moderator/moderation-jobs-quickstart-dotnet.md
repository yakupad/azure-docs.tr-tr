---
title: Azure içerik aracı - başlangıç yönetimini işleri .NET kullanarak | Microsoft Docs
description: .NET için Azure içerik denetleyici SDK'sını kullanarak yönetimini işleri başlatmasını nasıl
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/06/2018
ms.author: sajagtap
ms.openlocfilehash: a103875607355993e216ce1ddea02009fc8fa1c4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351694"
---
# <a name="start-moderation-jobs-using-net"></a>.NET kullanarak yönetimini işleri Başlat

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri için .NET için içerik denetleyici SDK'sını kullanarak kullanmaya başlama:
 
- Tarama ve İnsan denetleyiciler için incelemeler oluşturmak için bir denetleme işi Başlat
- Bekleyen durumunu Al
- İzleme ve gözden geçirme son durumunu Al
- Geri çağırma URL'si sonucu Gönder

Bu makalede, Visual Studio ve C# ile bilginiz olduğunu varsayar.

## <a name="sign-up-for-content-moderator-services"></a>İçerik denetleyici Hizmetleri için kaydolun

REST API veya SDK üzerinden içerik denetleyici Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir.
Başvurmak [Hızlı Başlangıç](quick-start.md) anahtarı nasıl edinebilirsiniz öğrenin.

## <a name="define-a-custom-moderation-workflow"></a>Bir özel Yönetimi iş akışı tanımlayın

Denetleme işi kullanır ve API'lerini kullanarak içeriğinizi tarar bir **iş akışı** veya incelemeler oluşturulup oluşturulmayacağını belirlemek için.
Gözden geçirme Aracı'nı varsayılan iş akışı şimdi içerirken [özel bir iş akışında tanımlayın](Review-Tool-User-Guide/Workflows.md) Bu Hızlı Başlangıç için.

İş akışının adı denetleme işini başlatır, kodunuzda kullanın.

## <a name="create-your-visual-studio-project"></a>Visual Studio projesi oluşturma

1. Yeni bir ekleme **konsol uygulaması (.NET Framework)** çözümünüzü projeye.

   Örnek kodda proje adı **CreateReviews**.

1. Bu proje çözüme yönelik tek başlangıç projesi olarak seçin.

1. Bir başvuru ekleyin **ModeratorHelper** proje oluşturduğunuz derleme [içerik denetleyici istemci Yardımcısı quickstart](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Aşağıdaki NuGet paketlerini yükleyin:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Güncelleştirme program using deyimleri

Değiştirme program using deyimleri kullanıcının.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatma

Statik alanları ve aşağıdaki sabitleri ekleyin **Program** Program.cs sınıfında.

> [!NOTE]
> TeamName sabiti içerik denetleyici aboneliğinizi oluştururken kullandığınız adına ayarlayın. Gelen TeamName almak [içerik denetleyicinin web sitesi](https://westus.contentmoderator.cognitive.microsoft.com/).
> Oturum açtığınızda, seçeneğini **kimlik bilgileri** gelen **ayarları** (dişli) menüsü.
>
> Takım adınızı değeri **kimliği** alanındaki **API** bölümü.


    /// <summary>
    /// The moderation job will use this workflow that you defined earlier.
    /// See the quickstart article to learn how to setup custom workflows.
    /// </summary>
    private const string WorkflowName = "OCR";
    
    /// <summary>
    /// The name of the team to assign the job to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "***";

    /// <summary>
    /// The URL of the image to create a review job for.
    /// </summary>
    private const string ImageUrl =
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png";

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Otomatik-Orta için kodu ekleyin, bir gözden geçirme oluşturun ve iş ayrıntılarını alma

> [!Note]
> Uygulamada, ayarladığınız geri çağırma URL'si **CallbackEndpoint** URL'sine el ile gözden geçirme (aracılığıyla bir HTTP POST isteği) sonuçlarını alır.

Aşağıdaki kodu eklemeye başlayın **ana** yöntemi.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the nameof the workflow defined in the online review tool.
            // See the quickstart article to learn more.
            var jobResult = client.Reviews.CreateJobWithHttpMessagesAsync(
                    TeamName, "image", "contentID", WorkflowName, "application/json", content, CallbackEndpoint);

            // Record the job ID.
            var jobId = jobResult.Result.Body.JobIdProperty;

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                jobResult.Result.Body, Formatting.Indented));

            Thread.Sleep(2000);
            writer.WriteLine();

            writer.WriteLine("Get review job status.");
            var jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
                    TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                    jobDetails.Result.Body, Formatting.Indented));

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propagate.");
            Thread.Sleep(latencyDelay * 1000);

            writer.WriteLine("Get review details.");
            jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
            TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
            jobDetails.Result.Body, Formatting.Indented));
        }
        writer.Flush();
        writer.Close();
    }

> [!NOTE]
> İçerik denetleyici hizmeti anahtarınızı ikinci (RPS) hız sınırı başına bir istek var. Sınırı aşarsanız, SDK 429 hata kodunu içeren bir özel durum oluşturur. 
>
> Ücretsiz katmanı anahtarı bir RPS hızı sınırı vardır.

## <a name="run-the-program-and-review-the-output"></a>Programını çalıştırın ve çıktıyı gözden geçirin

Aşağıdaki örnek çıkış konsolunda bakın:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

İçerik denetleyici içine oturum gözden bekleyen görüntü görmek için aracı gözden geçirin.

Kullanım **sonraki** gönderme düğmesi.

![İnsan araburucu için görüntü gözden geçirme](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Örnek çıktı günlük dosyasına bakın

> [!NOTE]
> Çıktı dosyanızda, dizeleri **Teamname**, **ContentID**, **CallBackEndpoint**, ve **Workflowıd** kullandığınız değerleri yansıtır daha önce.

    Create moderation job for an image.
    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

    Get review details.
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": [
        {
            "Ts": "2018-01-07T00:38:26.7714671",
            "Msg": "Successfully got hasText response from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:26.4181346",
            "Msg": "Getting hasText from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:25.5122828",
            "Msg": "Starting Execution - Try 1"
        }
        ]
    }


## <a name="your-callback-url-if-provided-receives-this-response"></a>Geri çağırma URL'si sağladıysa, bu yanıt alır.

Aşağıdaki örneğe benzer bir yanıt görürsünüz:

> [!NOTE]
> Dizeleri, geri çağırma yanıt **ContentID** ve **Workflowıd** daha önce kullanılan değerleri yansıtır.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe",
        "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
        "WorkFlowId": "OCR",
        "Status": "Complete",
        "ContentType": "Image",
        "CallBackType": "Job",
        "ContentId": "contentID",
        "Metadata": {
            "hastext": "True",
            "ocrtext": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
            "imagename": "contentID"
        }
    }


## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için ve tümleştirme üzerinde başlayın.
