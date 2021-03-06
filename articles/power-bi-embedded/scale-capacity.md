---
title: Power BI Embedded kapasitenizi ölçeklendirin | Microsoft Docs
description: Bu makalede, Microsoft Azure'da bir Power BI Embedded kapasite ölçeklendirme size yol göstermektedir.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/19/2018
ms.author: maghan
ms.openlocfilehash: a089d34b3f49188f3037ad57aab4d7bcc8c660f2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31408294"
---
# <a name="scale-your-power-bi-embedded-capacity"></a>Power BI Embedded kapasite ölçekleme

Bu makalede, Microsoft Azure'da bir Power BI Embedded kapasite ölçeklendirme size yol göstermektedir. Ölçeklemeyi artırmak veya kapasitenizi boyutunu azaltmak sağlar.

Bu Power BI Embedded kapasite oluşturduğunuz varsayar. Yüklemediyseniz, bkz: [Azure portalında Power BI Embedded oluşturmak kapasite](create-capacity.md) başlamak için.

> [!NOTE]
> Bir ölçeklendirme işlemi yaklaşık bir dakika sürebilir. Bu süre boyunca kapasite kullanılabilir olmaz. Katıştırılmış içerik yüklemek başarısız olabilir.

## <a name="scale-a-capacity"></a>Ölçek kapasitesi

1. [Azure portal](https://portal.azure.com/) oturum açın.

2. Seçin **tüm hizmetleri** > **Power BI Embedded** , kapasiteleri görmek için.

    ![Azure portal içindeki tüm hizmetler](media/scale-capacity/azure-portal-more-services.png)

3. Ölçeklendirmek istediğiniz kapasite seçin.

    ![Azure portalı içinde Power BI Embedded kapasite listesi](media/scale-capacity/azure-portal-capacity-list.png)

4. Seçin **fiyatlandırma katmanı** altında **ölçek** kapasitenizi içinde.

    ![Ölçek altında fiyatlandırma katmanı seçeneği](media/scale-capacity/azure-portal-scale-pricing-tier.png)

    Geçerli fiyatlandırma katmanınızı mavi renkte gösterilmiştir.

    ![Fiyatlandırma katmanı mavi renkte özetlenen geçerli](media/scale-capacity/azure-portal-current-tier.png)

5. Yukarı veya aşağı ölçeklendirmek için taşımak için yeni katmanı seçin. Yeni bir katman seçmek kesikli mavi anahat seçimi geçici yerleştirir. Seçin **seçin** yeni katmanı için.

    ![Yeni katmanı seçin](media/scale-capacity/azure-portal-select-new-tier.png)

    Kapasite ölçekleme veya iki tamamlamak için bir dakika sürebilir.

6. Genel Bakış sekmesi görüntüleyerek katmanınız onaylayın. Geçerli fiyatlandırma katmanı listelenir.

    ![Geçerli katmanı onaylayın](media/scale-capacity/azure-portal-confirm-tier.png)

## <a name="next-steps"></a>Sonraki adımlar

Duraklatmak veya kapasitenizi başlatmak için bkz: [duraklatma ve Power BI Embedded kapasitenizi Azure portalında Başlat](pause-start.md).

Power BI içeriğini, uygulamanızda katıştırma başlamak için bkz: [Power BI panoları, raporları ve döşeme katıştırma](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Başka sorunuz mu var? [Power BI topluluk isteyen deneyin](http://community.powerbi.com/)
