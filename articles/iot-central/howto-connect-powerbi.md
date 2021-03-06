---
title: Azure IOT Central verilerinizi Power BI panosunda Görselleştirme | Microsoft Docs
description: Azure IOT Central Analytics Power BI çözüm şablonu, görselleştirin ve IOT Central verilerinizi analiz etmek için kullanın.
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: peterpr
ms.openlocfilehash: 85c0432bceef3e94b32fa9b4a2803276b3efee17
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324332"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Power BI panosunda, Azure IOT Central verilerini Görselleştirme ve çözümleme

![Power BI çözüm şablonu işlem hattı](media/howto-connect-powerbi/iot-continuous-data-export.png)

Azure IOT Central Analytics Power BI çözüm şablonu, IOT cihazlarınızı performansını izlemek için güçlü bir Power BI panosu oluşturmak için kullanın. Power BI Panonuzda şunları yapabilirsiniz:
- Cihazlarınızı zamanla gönderdiğiniz veri miktarını izleme
- Telemetri, durumları ve etkinlikler arasında veri hacmi karşılaştırın
- Ölçümler çok sayıda raporlama cihazları belirleyin
- Cihaz ölçümleri'nin geçmiş eğilimleri gözlemleyin
- Çok sayıda önemli olaylar göndermek sorunlu cihazları belirleyin

Bu çözüm şablonu, Azure Blob Depolama hesabınızda veri alan bir işlem hattı ayarlar [verileri sürekli dışarı aktarma](howto-export-data.md). Bu veriler Azure işlevleri, Azure Data Factory ve Azure SQL veritabanı, işlem ve görselleştirileceğini ve PBIX dosyası olarak karşıdan yükleyebileceğiniz bir Power BI raporundaki analiz için verileri dönüştürürsünüz aracılığıyla akar. Her bileşen, gereksinimlerinize uyacak şekilde özelleştirebilirsiniz. Bu nedenle tüm bu kaynakları Azure aboneliğinizde oluşturulur. Tamamen açık kaynaklı, bu çözüm şablonu mimarisi hakkında daha fazla bilgi edinin ve ziyaret ederek uzatabilirsiniz [Github deposunu](https://aka.ms/iotcentralgithubpowerbisolutiontemplate).

**[Azure IOT Central analizi çözüm şablonu, Microsoft Appsource'tan alma.](https://aka.ms/iotcentralpowerbisolutiontemplate)**

## <a name="prerequisites"></a>Önkoşullar
Şablonu ayarı için aşağıdakiler gereklidir:
- Bir Azure aboneliğine erişim
- Verileriniz dışarı [verileri sürekli dışarı aktarma](howto-export-data.md) IOT Central uygulamanızdan. Ölçümler, cihazları ve cihaz şablonu akışları en iyi Power BI panosu için etkinleştirmeniz önerilir.
- Power BI Desktop (en son sürüm)
- Power BI Pro (panoyu başkalarıyla paylaşmak istiyorsanız)

## <a name="reports"></a>Reports

İki raporlar otomatik olarak oluşturulur. 

İlk rapor, cihazların raporladığı ölçümleri bir geçmiş görünümünü gösterir ve ölçümleri ve ölçümler için en yüksek sayıda göndermiş cihazlar farklı türde keser.

![Power BI rapor sayfası 1](media/howto-connect-powerbi/template-page1-hasdata.PNG)

İkinci raporun daha ayrıntılı olay türlerine geçiyor ve hataları ve Uyarıları bildirilen bir geçmiş görünümünü gösterir. Ayrıca, hangi cihazların, özellikle hata olayları ve uyarı olayları yanı sıra tüm en yüksek olay sayısı raporladığınız gösterir.

![Power BI rapor sayfası 2](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="resources"></a>Kaynaklar

AppSource almak için ziyaret [Azure IOT Central analizi çözüm şablonuyla](https://aka.ms/iotcentralpowerbisolutiontemplate).

Ziyaret [Github deposunu](https://aka.ms/iotcentralgithubpowerbisolutiontemplate) mimarisi hakkında daha fazla bilgi edinin ve uzatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Verilerinizi Power bı'da görselleştirin gerçekleştirmeyi öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihazları yönetme](howto-manage-devices.md)
