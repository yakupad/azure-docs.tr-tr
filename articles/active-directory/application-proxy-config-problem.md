---
title: Uygulama proxy'si uygulama oluşturma sorunu | Microsoft Docs
description: Azure AD Yönetim Portalı'nda uygulama proxy'si uygulamaları oluşturma sorunlarını giderme
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 97795593ecc83a0b15f018861f5cd9139d26443b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335117"
---
# <a name="problem-creating-an-application-proxy-application"></a>Uygulama proxy'si uygulama oluşturma sorunu 

Aşağıda bazı yaygın sorunlar kişiler yüz yeni bir uygulama proxy'si uygulama oluştururken verilmektedir.

## <a name="recommended-documents"></a>Önerilen belgeler 

Yönetim Portalı üzerinden uygulama proxy'si uygulama oluşturma hakkında daha fazla bilgi için bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](manage-apps/application-proxy-publish-azure-portal.md).

Bu belgede yer alan adımlar aşağıdaki ve uygulama oluşturulurken bir hata alıyor, bilgi için hata ayrıntıları ve uygulama gidermeye yönelik öneriler bakın. Çoğu hata iletileri önerilen bir düzeltmeyi içerir. 

## <a name="specific-things-to-check"></a>Denetlenecek belirli öğeler

Sık karşılaşılan hataları önlemek için doğrulayın:

-   Uygulama proxy'si uygulama oluşturmak için izne sahip bir yönetici

-   İç URL benzersizdir

-   Dış URL benzersizdir

-   URL'leri http veya https ile başlamalı ve bitmelidir bir "/"

-   URL bir etki alanı adı ve IP adresi olmalıdır

Uygulama oluşturduğunuzda, hata iletisi sağ üst köşedeki görüntülemelidir. Hata iletilerini görmek için bildirim simgesine de seçebilirsiniz.

   ![Bildirim istemi](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure portalında uygulama ara sunucusunu etkinleştirme](manage-apps/application-proxy-enable.md)
