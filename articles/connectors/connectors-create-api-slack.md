---
title: Azure logic apps içinde kayma Bağlayıcısı'nı kullanın | Microsoft Docs
description: Logic apps içinde Slack'e bağlanın
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 88b134a90ac385ad957d76f420fe85dc2dbbf751
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296232"
---
# <a name="get-started-with-the-slack-connector"></a>Slack Bağlayıcısı ile çalışmaya başlama
Slack, ekibinizin tüm yazışmalarını anında aranabilecek ve gittiğiniz her yerden ulaşılabilecek bir yerde toplayan bir ekip iletişim aracıdır. 

Bir mantıksal uygulama artık oluşturarak başlama; bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-slack"></a>Kayma bağlantı oluşturun.
Slack Bağlayıcısı'nı kullanmak için önce oluşturduğunuz bir **bağlantı** ardından ayrıntılar için bu özellikleri sağlar: 

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Belirteç |Evet |Slack Kimlik Bilgilerini Sağlayın |

Slack'e aşağıdaki adımları kullanarak oturum açın ve kayma yapılandırmasını tamamlamak **bağlantı** mantığı uygulamanıza:

1. Seçin **yineleme**
2. Seçin bir **sıklığı** ve girin bir **aralığı**
3. Seçin **Eylem Ekle**  
   ![Kayma yapılandırın][1]  
4. Kayma arama kutusuna girin ve adlarında boşluk içeren tüm girişleri döndürmek arama bekleyin
5. Seçin **Slack'e - posta iletisi**
6. Seçin **Slack'e oturum**:  
   ![Kayma yapılandırın][2]
7. Uygulama yetkilendirmek oturum açmak için Slack kimlik bilgilerinizi sağlayın    
   ![Kayma yapılandırın][3]  
8. Kuruluşunuzun oturum açma sayfasına yeniden yönlendirilmesi. **Yetki** mantıksal uygulamanızı ile etkileşim kurmak için kayma:      
   ![Kayma yapılandırın][5] 
9. Yetkilendirme tamamlandıktan sonra mantıksal uygulamanızı yapılandırarak tamamlamak için yönlendirilirsiniz **Slack - tüm iletileri Al** bölümü. Diğer tetikleyiciler ve gereken eylemleri ekleyin.  
   ![Kayma yapılandırın][6]
10. Seçerek çalışmanızı kaydedin **kaydetmek** (üst doğru) menüsünde.

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/slack/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
