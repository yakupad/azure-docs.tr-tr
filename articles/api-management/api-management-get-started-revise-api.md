---
title: Azure API Management’ta hataya neden olmayan değişiklikleri güvenli bir şekilde yapmak için revizyonları kullanma | Microsoft Docs
description: API Management’ta revizyonları kullanarak hataya neden olmayan değişiklikler yapmayı öğrenmek için bu öğreticideki adımları uygulayın.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 864c269da61bcea885249021a44fe0259ca83e90
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33935173"
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Hataya neden olmayan değişiklikleri güvenli bir şekilde yapmak için düzeltmeleri kullanma
API’niz kullanıma hazır olduğunda ve geliştiriciler tarafından kullanılmaya başladığında genellikle bu API’de değişiklikler yapmak ve aynı zamanda API’nizi çağıranları kesintiye uğratmamak için dikkatli olmanız gerekir. Yaptığınız değişiklikleri geliştiricilere bildirmeniz de yararlıdır. Azure API Management’da **düzeltmeleri** kullanarak bunu yapabilirsiniz. Daha fazla bilgi için bkz. [Sürümler ve revizyonlar](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) ve [Azure API Management ile API Sürümü Oluşturma](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni düzeltme ekleme
> * Düzeltmenizde hataya neden olmayan değişiklikler yapma
> * Düzeltmenizi geçerli hale getirme ve bir değişiklik günlüğü girdisi ekleme
> * Değişiklikleri ve değişiklik günlüğünü görmek için geliştirici portalına göz atma

![Geliştirici Portalında Değişiklik Günlüğü](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Ön koşullar

+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

## <a name="add-a-new-revision"></a>Yeni düzeltme ekleme

1. **API’ler** sayfasını seçin.
2. API listesinden **Konferans API’sini** (veya düzeltme eklemek istediğiniz diğer API’yi) seçin.
3. Sayfanın üst kısmındaki menüden **Düzeltmeler** sekmesine tıklayın.
4. **+ Düzeltme Ekle**’yi seçin

    > [!TIP]
    > Ayrıca API’nin açılır menüsünden (**...**) **Düzeltme Ekle**’yi seçebilirsiniz.
    
    ![Ekranın üst kısmındaki Düzeltmeler menüsü](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Ne için kullanılacağını hatırlamaya yardımcı olmak için revizyonunuza ilişkin bir açıklama ekleyin.
6. **Oluştur**’u seçin
7. Yeni bir düzeltme oluşturulur.

    > [!NOTE]
    > Özgün API'niz **Düzeltme 1**’de kalır. Farklı bir düzeltmeyi güncel yapana kadar kullanıcılarınız bu düzeltmeyi çağırmaya devam eder.

## <a name="make-non-breaking-changes-to-your-revision"></a>Düzeltmenizde hataya neden olmayan değişiklikler yapma

1. API listesinden **Konferans API’sini** seçin.
2. Ekranın üst kısmında **Tasarım** sekmesini seçin.
3. **Düzeltme seçici**’nin (tasarım sekmesinin hemen üzerinde) geçerli düzeltme olarak **Düzeltme 2**’yi gösterdiğinden emin olun.

    > [!TIP]
    > Üzerinde çalışmak istediğiniz düzeltmeler arasında geçiş yapmak için düzeltme seçiciyi kullanın.

4. **+ İşlem Ekle**’yi seçin.
5. Yeni işleminizi **POST** olarak, işlemin Adını ve Görünen Adını ise **test** olarak ayarlayın
6. Yeni işleminizi **kaydedin**.
7. **Düzeltme 2**’de bir değişiklik yapmış olduk. **Düzeltme 1**’e geri dönmek için sayfanın üst kısmındaki **Düzeltme Seçici**’yi kullanın.
8. Yeni işleminizin **Düzeltme 1**’de görünmediğine dikkat edin. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Düzeltmenizi geçerli hale getirme ve bir değişiklik günlüğü girdisi ekleme
1. Sayfanın üst kısmındaki menüden **Düzeltmeler** sekmesini seçin.

    ![Düzeltme ekranındaki düzeltme menüsü.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. **Düzeltme 2**’nin açılır menüsünü (**...** ) açın.
2. **Geçerli Hale Getir**’i seçin. Bu değişiklik hakkında notlar yayınlamak istiyorsanız **Bu API için Genel Değişiklik Günlüğüne Gönder**’i işaretleyin.
3. **Bu API için Genel Değişiklik Günlüğüne Gönder**’i seçin
4. Düzeltmeleriniz için geliştiricilerin gördüğü bir açıklama ekleyin, örn. **Test düzeltmeleri. Yeni "test" işlemi eklendi.**
5. **Düzeltme 2** artık geçerlidir.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Değişiklikleri ve değişiklik günlüğünü görmek için geliştirici portalına göz atma
1. Azure portalında **API’ler** öğesini seçin.
2. Üstteki menüden **Geliştirici Portalı**’nı seçin.
3. **API'ler** ve ardından **Konferans API’si** öğesini seçin.
4. Yeni **test** işleminizin artık kullanılabilir olduğuna dikkat edin.
5. API adının altından **API Değişiklik Geçmişi**’ni seçin.
6. Değişiklik günlüğü girdinizin bu listede göründüğüne dikkat edin.

    ![Geliştirici portalı](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yeni düzeltme ekleme
> * Düzeltmenizde hataya neden olmayan değişiklikler yapma
> * Düzeltmenizi geçerli hale getirme ve bir değişiklik günlüğü girdisi ekleme
> * Değişiklikleri ve değişiklik günlüğünü görmek için geliştirici portalına göz atma

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [API'nizin birden fazla sürümünü yayımlama](api-management-get-started-publish-versions.md)