---
title: OneDrive - Azure mantıksal uygulamaları bağlanma | Microsoft Docs
description: Karşıya yükleme ve OneDrive REST API'leri ve Azure Logic Apps ile dosyalarını yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 10/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 50bd9ecdd665cf72c146c63ae25efa6773934a3e
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295389"
---
# <a name="get-started-with-the-onedrive-connector"></a>OneDrive Bağlayıcısı ile çalışmaya başlama
Karşıya yükleme de dahil olmak üzere dosyalarınızı yönetmek için OneDrive bağlanmak, almak için dosyaları ve diğer silin. 

OneDrive ile: 

* Onedrive'daki dosyaları depolayarak, iş akışı oluşturma veya varolan dosyaları OneDrive güncelleştirin. 
* Bir dosya oluşturulduğunda veya OneDrive'ınıza içinde güncelleştirildiğinde iş akışını başlatmak için Tetikleyiciler kullanın.
* Eylemler bir dosya oluşturun ve bir dosyayı silmek için kullanın. Örneğin, yeni Office 365 e-posta eki (tetikleyici) alındığında, OneDrive (bir eylem) yeni bir dosya oluşturun.

Bu makalede, bir mantıksal uygulama OneDrive Bağlayıcısı'nı kullanmayı gösterir ve ayrıca tetikleyiciler ve eylemler listelenmektedir.

Logic Apps hakkında daha fazla bilgi için bkz: [logic apps nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>OneDrive Bağlan
Mantıksal uygulamanızı herhangi bir hizmete erişebilmesi için önce oluşturduğunuz bir *bağlantı* hizmet. Bağlantı bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, OneDrive bağlanmak için önce bir OneDrive gerekir *bağlantı*. Bir bağlantı oluşturmak için normalde bağlanmak istediğiniz hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle, OneDrive ile bağlantı oluşturmak için OneDrive hesabınıza kimlik bilgilerini girin.

### <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Bir tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. Tetikleyiciler "hizmet bir aralığı ve istediğiniz sıklığı yoklama". [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Mantıksal uygulama "onedrive" Tetikleyiciler listesini almak için aşağıdakileri yazın:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Seçin **bir dosya değiştirildiği**. Bir bağlantı zaten varsa, bir klasör seçmek için seçiciyi Göster düğmesini seçin.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Oturum açmak için istenirse, oturum bağlantısı oluşturmak için Ayrıntılar girin. [Bağlantı oluşturmak](connectors-create-api-onedrive.md#create-the-connection) bu makaledeki adımları listeler. 
   
   > [!NOTE]
   > Bu örnekte, mantıksal uygulama güncelleştirilir seçtiğiniz klasöründe bir dosya açıldığında çalıştırır. Bu tetikleyici sonuçlarını görmek için bir e-posta gönderir başka bir eylem ekleyin. Örneğin, Office 365 Outlook ekleme *bir e-posta Gönder* bir dosya güncelleştirildiğinde, e-postalar eylem. 

3. Seçin **Düzenle** düğmesine tıklayın ve ayarlama **sıklığı** ve **aralığı** değerleri. Örneğin, 15 dakikada bir yoklamak için tetikleyicinin isterseniz, daha sonra ayarlamak **sıklığı** için **dakika**ve **aralığı** için **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="use-an-action"></a>Bir eylem kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçeneğiniz bkz: **Eylem Ekle**, **bir koşul eklemek**, veya biri **daha fazla** seçenekleri.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Seçin **Eylem Ekle**.
3. Metin kutusuna, kullanılabilir tüm eylemlerin bir listesini almak için "onedrive" yazın.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. Bizim örneğimizde seçin **OneDrive - dosyası oluşturma**. Bir bağlantı zaten varsa, ardından **klasör yolu** dosya yerleştirilecek girin **dosya adı**ve seçin **dosya içeriği** istediğiniz:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Bağlantı bilgilerini istenirse, bağlantı oluşturmak için ayrıntılarını girin. [Bağlantı oluşturmak](connectors-create-api-onedrive.md#create-the-connection) bu makalede bu özellikleri açıklar. 
   
   > [!NOTE]
   > Bu örnekte, OneDrive klasöründe yeni bir dosya oluşturun. Başka bir tetikleyici çıktısını OneDrive dosyası oluşturmak için kullanabilirsiniz. Örneğin, Office 365 Outlook ekleme *yeni bir e-posta geldiğinde* tetikleyici. OneDrive ekleme *dosyası oluştur* Content-Type ve ekleri kullandığı eylem Onedrive'da yeni dosyası oluşturmak için bir ForEach içindeki alanları. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.


## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Daha fazla bağlayıcılar
Geri dönerek [API'leri listesi](apis-list.md).