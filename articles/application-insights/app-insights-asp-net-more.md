---
title: Azure Application Insights dışında daha fazlasına | Microsoft Docs
description: Application Insights ile Başlarken sonra keşfedebilirsiniz özelliklerinin bir özeti aşağıda verilmiştir.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 2f03083367de4e818bdc953ab76c28ff687f0a48
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294345"
---
# <a name="more-telemetry-from-application-insights"></a>Daha fazla Application Insights telemetrisini
Sonra [ASP.NET kodunuza Application Insights eklenen](app-insights-asp-net.md), daha fazla telemetrisi almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) [Durum İzleyicisi yükleme](http://go.microsoft.com/fwlink/?LinkId=506648) her server makinesinde.<br/>(Azure web uygulamaları) Web uygulaması için Azure denetim masasında Application Insights dikey penceresini açın.| [**Performans sayaçları**](app-insights-performance-counters.md)<br/>[**Özel durumlar** ](app-insights-asp-net-exceptions.md) - ayrıntılı Yığın izlemeleri<br/>[**Bağımlılıklar**](app-insights-asp-net-dependencies.md)|
|[JavaScript kod parçacığı web sayfalarınıza ekleme](app-insights-javascript.md)|[Sayfa performans](app-insights-web-track-usage.md), tarayıcı özel durumları, AJAX performans. Özel istemci-tarafı telemetri.|
|[Kullanılabilirlik web testleri oluşturma](app-insights-monitor-web-app-availability.md)|Sitenizi kullanılamaz hale gelirse uyarıları alma|
|[Buildinfo.config olun](https://msdn.microsoft.com/library/dn449058.aspx) MSBuild tarafından oluşturulan|[Ölçüm grafiklerinde ek açıklamaları oluşturma](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Özel olayları ve ölçümleri yazma](app-insights-api-custom-events-metrics.md)|İş olayları ve ölçümleri sayısı, ayrıntılı kullanım ve daha fazla izleme.|
|[Canlı sitenizi profil](https://aka.ms/AIProfilerPreview)|Canlı web uygulamanızdan ayrıntılı işlevi zamanlamaları|






