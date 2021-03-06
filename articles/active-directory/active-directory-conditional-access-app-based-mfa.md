---
title: Hızlı Başlangıç - Azure Active Directory koşullu erişimiyle belirli uygulamalar için çok faktörlü kimlik doğrulaması (MFA) gerekli | Microsoft Docs
description: Bu hızlı başlangıçta, Azure Active Directory (Azure AD) koşullu erişim kullanarak erişilen bulut uygulaması türü için kimlik doğrulaması gereksinimlerinizi nasıl bağlayabilirsiniz öğrenin.
services: active-directory
keywords: uygulamalar, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 0a2cddec1308b96b960cd0bbc5ccae9fd1a1a9a7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450382"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Hızlı Başlangıç: Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektirme 

Kullanıcıların oturum açma deneyimini basitleştirmek için bulut uygulamalarınızdaki kullanıcı adı ve parola kullanarak oturum açmak izin vermek isteyebilirsiniz. Ancak, birçok ortamda, daha güçlü bir form çok faktörlü kimlik doğrulaması (MFA) gibi hesap doğrulama gerektirecek şekilde tavsiye olduğu en az birkaç uygulama vardır. Bu, örneğin true, kuruluşunuzun e-posta sisteminize veya ik uygulamalarınıza erişim için olabilir. Azure Active Directory'de (Azure AD), bir koşullu erişim ilkesi ile bu hedefe gerçekleştirebilirsiniz.    

Bu hızlı başlangıçta nasıl yapılandırılacağını gösteren bir [Azure AD koşullu erişim ilkesi](active-directory-conditional-access-azure-portal.md) , seçili bulut uygulaması ortamınızda çok faktörlü kimlik doğrulaması gerektirir.

![İlke oluşturma](./media/active-directory-conditional-access-app-based-mfa/32.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="prerequisites"></a>Önkoşullar 

Bu hızlı başlangıçta senaryoyu tamamlamak için gerekir:

- **Bir Azure AD Premium sürümü için erişim** -Azure AD koşullu erişimi olan bir Azure AD Premium özelliği. 

- **Adlı bir test hesabı Isabella Simonsen** - bir test hesabı oluşturmak için bkz bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


## <a name="test-your-sign-in"></a>Oturum açma testi

Bu adımın amacı, bir koşullu erişim ilkesi olmadan oturum açma deneyimi izlenimi almaktır.

**Ortamınızı başlatmak için:**

1. Isabella Simonsen, Azure portalında oturum açın.

2. Oturumunuzu kapatın.


## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturun 

Bu bölümde, gerekli koşullu erişim ilkesi oluşturma işlemi gösterilmektedir. Bu hızlı başlangıçta bir senaryo kullanır:

- Azure portalı MFA gerektiren bir bulut uygulaması için yer tutucu olarak. 
- Koşullu erişim ilkesini test etmek için örnek kullanıcı.  

İlkenizde, ayarlayın:

|Ayar |Değer|
|---     | --- |
|Kullanıcılar ve gruplar | Isabella Simonsen |
|Bulut uygulamaları | Microsoft Azure Yönetimi |
|Erişim verme | Çok faktörlü kimlik doğrulaması gerektir |
 

![İlke oluşturma](./media/active-directory-conditional-access-app-based-mfa/31.png)

 


**Koşullu erişim ilkenizi yapılandırmak için:**

1. Oturum açın, [Azure portalında](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.

2. Azure portalında sol gezinti çubuğunda tıklatın **Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-app-based-mfa/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **Yönet** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-app-based-mfa/03.png)
 
4. Üzerinde **koşullu erişim** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.

    ![Ekle](./media/active-directory-conditional-access-app-based-mfa/04.png)

5. Üzerinde **yeni** sayfasında **adı** metin kutusuna **Azure portal erişimi için MFA gerektiren**.

    ![Ad](./media/active-directory-conditional-access-app-based-mfa/05.png)

6. İçinde **atama** bölümünde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar](./media/active-directory-conditional-access-app-based-mfa/06.png)

7. Üzerinde **kullanıcılar ve gruplar** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcılar ve gruplar](./media/active-directory-conditional-access-app-based-mfa/24.png)

    a. Tıklayın **kullanıcıları ve grupları seçin**ve ardından **kullanıcılar ve gruplar**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Isabella Simonsen**ve ardından **seçin**.

    d. Üzerinde **kullanıcılar ve gruplar** sayfasında **Bitti**.

8. Tıklayın **bulut uygulamaları**.

    ![Bulut uygulamaları](./media/active-directory-conditional-access-app-based-mfa/08.png)

9. Üzerinde **bulut uygulamaları** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bulut uygulamalarını seçin](./media/active-directory-conditional-access-app-based-mfa/26.png)

    a. Tıklayın **uygulamaları Seç**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

    d. Üzerinde **bulut uygulamaları** sayfasında **Bitti**.


10. İçinde **erişim denetimleri** bölümünde **Grant**.

    ![Erişim denetimleri](./media/active-directory-conditional-access-app-based-mfa/10.png)

11. Üzerinde **Grant** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Erişim İzni Verme](./media/active-directory-conditional-access-app-based-mfa/11.png)

    a. Seçin **erişim ver**.

    a. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.

    b. **Seç**'e tıklayın.

12. İçinde **ilkesini etkinleştir** bölümünde **üzerinde**.

    ![İlkeyi etkinleştir](./media/active-directory-conditional-access-app-based-mfa/18.png)

13. **Oluştur**’a tıklayın.


## <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre büyük olasılıkla beklenen şekilde çalışıp çalışmadığını bilmek ister. İlk adım, koşullu erişim ilkesi durum aracı bir oturum açma, test kullanıcının benzetimini yapmak için kullanın. Benzetim etkisi bu oturum açma ilkelerinizi üzerinde sahip ve bir simülasyon rapor oluşturan tahmin eder.  

İlke değerlendirmesi aracı, ayarlarsanız ne başlatmak için:

- **Isabella Simonsen** kullanıcı olarak 
- **Microsoft Azure Management** bulut uygulaması olarak

 Tıklayarak **ne** gösteren bir simülasyon raporu oluşturur:

- **Azure portala erişim için mfa'yı gerekli** altında **uygulanacak ilkeler** 
- **Çok faktörlü kimlik doğrulaması gerektiren** olarak **denetimleri ver**.

![Durum ilkesi aracı](./media/active-directory-conditional-access-app-based-mfa/23.png)



**Koşullu erişim ilkenizi değerlendirmek için:**

1. Üzerinde [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **ne**.  
 
    ![What If](./media/active-directory-conditional-access-app-based-mfa/14.png)

2. Tıklayın **kullanıcılar**seçin **Isabella Simonsen**ve ardından **seçin**.

    ![Kullanıcı](./media/active-directory-conditional-access-app-based-mfa/15.png)

2. Bulut uygulaması seçmek için aşağıdaki adımları gerçekleştirin:

    ![Bulut uygulamaları](./media/active-directory-conditional-access-app-based-mfa/16.png)

    a. Tıklayın **bulut uygulamaları**.

    b. Üzerinde **bulut uygulamaları sayfası**, tıklayın **uygulamaları Seç**.

    c. **Seç**'e tıklayın.

    d. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

    e. Bulut uygulamaları sayfasında tıklayın **Bitti**.

3. Tıklayın **ne**.


## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test

Önceki bölümde, bir sanal oturum değerlendirilecek öğrendiniz. Bir simülasyon ek olarak, koşullu erişim ilkenizi, beklendiği gibi çalıştığından emin olmak için sınamalısınız. 

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) kullanarak, **Isabella Simonsen** test hesabı. Hesabınızı ek güvenlik doğrulaması için ayarlamanızı gerektiren bir iletişim kutusu görüntülenir.

![Multi-factor authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı ve koşullu erişim ilkesini Sil:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [Azure AD'den kullanıcı silme](fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- İlkeniz silinecek ilkenizi seçin ve ardından **Sil** hızlı erişim araç çubuğu.

    ![Multi-factor authentication](./media/active-directory-conditional-access-app-based-mfa/33.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kabul edilmesi için kullanım koşullarını gerektiren](./active-directory-conditional-access-tou.md)
> [oturumu risk algılandığında erişimi engelle](./active-directory-conditional-access-app-sign-in-risk.md)
