---
title: Javascript - Microsoft Bilişsel hizmetler Anomali Bulucu API kullanma | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get Anomali Bulucu Bilişsel hizmetler Javascript ile kullanmaya başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 105da0219efd7bf7c9a105a018dda7ea5b084cfd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353339"
---
# <a name="use-the-anomaly-finder-api-with-javascript"></a>Anomali Bulucu API Javascript ile kullanma

Bu makalede bilgiler sağlanmaktadır ve hızlı bir şekilde yardımcı olmak için kod örnekleri anomali algılama sonucu zaman serisi veri alma görevi gerçekleştirmek için Javascript ile Anomali algılama API kullanarak çalışmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-detection-api-using-javascript"></a>Javascript kullanarak Anomali algılama API'si ile anomali noktalarını alma

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Zaman serisi veri örneği

Zaman serisi veri noktaları örneği aşağıdaki gibidir: [!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-javascript-example"></a>Verileri çözümlemek ve Javascript örnek anomali noktaları Al

Örneği kullanarak adımlar aşağıdaki gibidir.

1. Yeni bir HTML dosyası oluşturun.
2. HTML dosyasını aşağıdaki kodla değiştirin.
3. Değiştir `[YOUR_SUBSCRIPTION_KEY]` değeri geçerli bir abonelik anahtarınızı ile.
4. RequestTextArea verileri, veri noktası ile değiştirin.
5. Bir Web tarayıcısında HTML dosyasını açın ve'ı tıklatın `Anomaly Detection` düğmesi.

```Javascript
<!DOCTYPE html>
<html>
<head>
    <title>Anomaly Detection Javascript Quick Starts</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>
<body>

    <script type="text/javascript">
        function detect() {
            // **********************************************
            // *** Update or verify the following values. ***
            // **********************************************
            // Replace the subscriptionKey string value with your valid subscription key.
            var subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";

            var uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

            var body = document.getElementById("requestTextArea").value;
            // Perform the REST API call.
            $.ajax({
                url: uriBase,
                // Request headers.
                beforeSend: function (xhrObj) {
                    xhrObj.setRequestHeader("Content-Type", "application/json");
                    xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                },
                type: "POST",
                // Request body.
                data: body,
            })
            .done(function (data) {
                // Show formatted JSON on webpage.
                $("#responseTextArea").val(JSON.stringify(data, null, 2));
            })
            .fail(function (jqXHR, textStatus, errorThrown) {
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
                alert(errorString);
            });
        };
    </script>

    <h1>Anomaly Detection Javascript Quick Starts:</h1>
    Enter the detection data, then click the <strong>Anomaly Detection</strong> button.

    <button onclick="detect()">Anomaly Detection</button>
    <br><br>
    <div id="wrapper" style="width:1020px; display:table;">
        <div id="imageDiv" style="width:420px; display:table-cell;">
            Request:
            <br><br>
            <textarea id="requestTextArea" class="UIInput" style="width:580px; height:400px;">
    {
        "Period": 7,
        "Points": [
            {
              "Timestamp": "2018-03-01T00:00:00Z",
              "Value": 32858923
            },
            {
              "Timestamp": "2018-03-02T00:00:00Z",
              "Value": 29615278
            },
            {
              "Timestamp": "2018-03-03T00:00:00Z",
              "Value": 22839355
            },
            {
              "Timestamp": "2018-03-04T00:00:00Z",
              "Value": 25948736
            },
            {
              "Timestamp": "2018-03-05T00:00:00Z",
              "Value": 34139159
            },
            {
              "Timestamp": "2018-03-06T00:00:00Z",
              "Value": 33843985
            },
            {
              "Timestamp": "2018-03-07T00:00:00Z",
              "Value": 33637661
            },
            {
              "Timestamp": "2018-03-08T00:00:00Z",
              "Value": 32627350
            },
            {
              "Timestamp": "2018-03-09T00:00:00Z",
              "Value": 29881076
            },
            {
              "Timestamp": "2018-03-10T00:00:00Z",
              "Value": 22681575
            },
            {
              "Timestamp": "2018-03-11T00:00:00Z",
              "Value": 24629393
            },
            {
              "Timestamp": "2018-03-12T00:00:00Z",
              "Value": 34010679
            },
            {
              "Timestamp": "2018-03-13T00:00:00Z",
              "Value": 33893888
            },
            {
              "Timestamp": "2018-03-14T00:00:00Z",
              "Value": 33760076
            },
            {
              "Timestamp": "2018-03-15T00:00:00Z",
              "Value": 33093515
            },
            {
              "Timestamp": "2018-03-16T00:00:00Z",
              "Value": 29945555
            },
            {
              "Timestamp": "2018-03-17T00:00:00Z",
              "Value": 22676212
            },
            {
              "Timestamp": "2018-03-18T00:00:00Z",
              "Value": 25262514
            },
            {
              "Timestamp": "2018-03-19T00:00:00Z",
              "Value": 33631649
            },
            {
              "Timestamp": "2018-03-20T00:00:00Z",
              "Value": 34468310
            },
            {
              "Timestamp": "2018-03-21T00:00:00Z",
              "Value": 34212281
            },
            {
              "Timestamp": "2018-03-22T00:00:00Z",
              "Value": 38144434
            },
            {
              "Timestamp": "2018-03-23T00:00:00Z",
              "Value": 34662949
            },
            {
              "Timestamp": "2018-03-24T00:00:00Z",
              "Value": 24623684
            },
            {
              "Timestamp": "2018-03-25T00:00:00Z",
              "Value": 26530491
            },
            {
              "Timestamp": "2018-03-26T00:00:00Z",
              "Value": 35445003
            },
            {
              "Timestamp": "2018-03-27T00:00:00Z",
              "Value": 34250789
            },
            {
              "Timestamp": "2018-03-28T00:00:00Z",
              "Value": 33423012
            },
            {
              "Timestamp": "2018-03-29T00:00:00Z",
              "Value": 30744783
            },
            {
              "Timestamp": "2018-03-30T00:00:00Z",
              "Value": 25825128
            },
            {
              "Timestamp": "2018-03-31T00:00:00Z",
              "Value": 21244209
            },
            {
              "Timestamp": "2018-04-01T00:00:00Z",
              "Value": 22576956
            },
            {
              "Timestamp": "2018-04-02T00:00:00Z",
              "Value": 31957221
            },
            {
              "Timestamp": "2018-04-03T00:00:00Z",
              "Value": 33841228
            },
            {
              "Timestamp": "2018-04-04T00:00:00Z",
              "Value": 33554483
            },
            {
              "Timestamp": "2018-04-05T00:00:00Z",
              "Value": 32383350
            },
            {
              "Timestamp": "2018-04-06T00:00:00Z",
              "Value": 29494850
            },
            {
              "Timestamp": "2018-04-07T00:00:00Z",
              "Value": 22815534
            },
            {
              "Timestamp": "2018-04-08T00:00:00Z",
              "Value": 25557267
            },
            {
              "Timestamp": "2018-04-09T00:00:00Z",
              "Value": 34858252
            },
            {
              "Timestamp": "2018-04-10T00:00:00Z",
              "Value": 34750597
            },
            {
              "Timestamp": "2018-04-11T00:00:00Z",
              "Value": 34717956
            },
            {
              "Timestamp": "2018-04-12T00:00:00Z",
              "Value": 34132534
            },
            {
              "Timestamp": "2018-04-13T00:00:00Z",
              "Value": 30762236
            },
            {
              "Timestamp": "2018-04-14T00:00:00Z",
              "Value": 22504059
            },
            {
              "Timestamp": "2018-04-15T00:00:00Z",
              "Value": 26149060
            },
            {
              "Timestamp": "2018-04-16T00:00:00Z",
              "Value": 35250105
            }
        ]
    }
        </textarea>
        </div>
        <div id="jsonOutput" style="width:600px; display:table-cell;">
            Response:
            <br><br>
            <textarea id="responseTextArea" class="UIInput" style="width:580px; height:400px;"></textarea>
        </div>
    </div>
</body>
</home>
```

### <a name="example-response"></a>Örnek yanıt

Başarılı yanıt JSON döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [JavaScript uygulaması](../tutorials/javascript-tutorial.md)
