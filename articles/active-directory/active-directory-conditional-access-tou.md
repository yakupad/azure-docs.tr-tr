---
title: Hızlı Başlangıç - Azure Active Directory koşullu erişim tarafından korunan bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektiren | Microsoft Docs
description: Bu hızlı başlangıçta, seçili bulut uygulamalarına erişim tarafından tarafından Azure Active Directory koşullu erişim sağlanmadan önce kullanım koşullarınızı kabul edildiğini nasıl gerektirebilir öğrenin.
services: active-directory
keywords: uygulamalar, Azure AD ile koşullu erişim, şirket kaynaklarına koşullu erişim ilkeleri, kullanım koşullarını güvenli erişim için koşullu erişim
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
ms.openlocfilehash: d5fad320c49691626c820429b07b6864137760b9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450275"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Hızlı Başlangıç: Bulut uygulamaları erişmeden önce kabul edilmesi için kullanım koşullarını gerektirir 

Ortamınızdaki belirli bulut uygulamalarına erişmeden önce kullanıcılardan (ToU) kullanım koşullarınızı kabul eden bir formun onay almak isteyebilirsiniz. Azure Active Directory (Azure AD) koşullu erişim aşağıdakileri sağlar: 

- Kullanım koşullarını yapılandırmak için basit bir yöntem
- Koşullu erişim ilkesi aracılığıyla kullanım koşullarınızı kabul etmesini Gerektir seçeneği  

Bu hızlı başlangıçta nasıl yapılandırılacağını gösteren bir [Azure AD koşullu erişim ilkesi](active-directory-conditional-access-azure-portal.md) seçili bulut uygulaması ortamınızda kabul edilmesi bir ToU gerektirir.

![İlke oluşturma](./media/active-directory-conditional-access-tou/5555.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="prerequisites"></a>Önkoşullar 

Bu hızlı başlangıçta senaryoyu tamamlamak için gerekir:

- **Bir Azure AD Premium sürümü için erişim** -Azure AD koşullu erişimi olan bir Azure AD Premium özelliği. 

- **Adlı bir test hesabı Isabella Simonsen** - bir test hesabı oluşturmak için bkz bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


## <a name="test-your-sign-in"></a>Oturum açma testi

Bu adımın amacı, bir koşullu erişim ilkesi olmadan oturum açma deneyimi izlenimi almaktır.

**Oturum açma işleminizi test etmek için:**

1. Oturum açın, [Azure portalında](https://portal.azure.com/) Isabella Simonsen olarak.

2. Oturumunuzu kapatın.




## <a name="create-your-terms-of-use"></a>Kullanım koşullarınızın oluşturma

Bu bölümde örnek bir ToU oluşturma adımlarını sağlar. İçin bir değer bir ToU oluşturduğunuzda, seçtiğiniz **zorla koşullu erişim ilkesi şablonlarıyla**. Seçme **özel İlkesi** , ToU oluşturulduktan hemen sonra yeni bir koşullu erişim ilkesi oluşturmak için iletişim kutusu açılır.


**Kullanım koşullarınızın oluşturmak için:**

1. Microsoft Word'de yeni bir belge oluşturun.

2. Tür **My kullanım koşullarını**ve belge olarak bilgisayarınıza kaydedin **mytou.pdf**.

3. Oturum açın, [Azure portalında](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.

4. Azure portalında sol gezinti çubuğunda tıklatın **Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-tou/02.png)

5. Üzerinde **Azure Active Directory** sayfasında **Yönet** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-tou/03.png) 

6. İçinde **Yönet** bölümünde **kullanım koşullarını**.

    ![Kullanım koşulları](./media/active-directory-conditional-access-tou/04.png) 

7. Üstteki menüden **yeni terimler**.

    ![Kullanım koşulları](./media/active-directory-conditional-access-tou/05.png) 

8. Üzerinde **yeni kullanım koşulları** sayfası:

    ![Kullanım koşulları](./media/active-directory-conditional-access-tou/112.png) 

    a. İçinde **adı** metin kutusuna **My TOU**.

    b. İçinde **görünen ad** metin kutusuna **My TOU**.

    c. PDF dosyasını kullanma koşullarınızın karşıya yükleyin.

    d. Olarak **dil**seçin **İngilizce**.

    e. Olarak **kullanıcıların kullanım koşullarını genişletmesini gerekli kıl**seçin **üzerinde**.

    f. Olarak **zorla koşullu erişim ilkesi şablonlarıyla**seçin **özel İlkesi**.

    g. **Oluştur**’a tıklayın.
 


## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturun 

Bu bölümde, gerekli koşullu erişim ilkesi oluşturma işlemi gösterilmektedir. Bu hızlı başlangıçta bir senaryo kullanır:

- Azure portalı, kullanım koşullarını kabul edilmesini gerektiren bir bulut uygulaması için yer tutucu olarak. 
- Koşullu erişim ilkesini test etmek için örnek kullanıcı.  

İlkenizde, ayarlayın:

|Ayar |Değer|
|---     | --- |
|Kullanıcılar ve gruplar | Isabella Simonsen |
|Bulut uygulamaları | Microsoft Azure Yönetimi |
|Erişim verme | My TOU |
 

![İlke oluşturma](./media/active-directory-conditional-access-tou/1234.png)

 


**Koşullu erişim ilkenizi yapılandırmak için:**

1. Üzerinde **yeni** sayfasında **adı** metin kutusuna **gerektiren TOU Isabella için**.

    ![Ad](./media/active-directory-conditional-access-tou/71.png)

2. İçinde **atama** bölümünde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar](./media/active-directory-conditional-access-tou/06.png)

3. Üzerinde **kullanıcılar ve gruplar** sayfası:

    ![Kullanıcılar ve gruplar](./media/active-directory-conditional-access-tou/24.png)

    a. Tıklayın **kullanıcıları ve grupları seçin**ve ardından **kullanıcılar ve gruplar**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Isabella Simonsen**ve ardından **seçin**.

    d. Üzerinde **kullanıcılar ve gruplar** sayfasında **Bitti**.

4. Tıklayın **bulut uygulamaları**.

    ![Bulut uygulamaları](./media/active-directory-conditional-access-tou/08.png)

5. Üzerinde **bulut uygulamaları** sayfası:

    ![Bulut uygulamalarını seçin](./media/active-directory-conditional-access-tou/26.png)

    a. Tıklayın **uygulamaları Seç**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

    d. Üzerinde **bulut uygulamaları** sayfasında **Bitti**.


6. İçinde **erişim denetimleri** bölümünde **Grant**.

    ![Erişim denetimleri](./media/active-directory-conditional-access-tou/10.png)

7. Üzerinde **Grant** sayfası:

    ![Erişim İzni Verme](./media/active-directory-conditional-access-tou/111.png)

    a. Seçin **erişim ver**.

    a. Seçin **My TOU**.

    b. **Seç**'e tıklayın.

8. İçinde **ilkesini etkinleştir** bölümünde **üzerinde**.

    ![İlkeyi etkinleştir](./media/active-directory-conditional-access-tou/18.png)

9. **Oluştur**’a tıklayın.


## <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre büyük olasılıkla beklenen şekilde çalışıp çalışmadığını bilmek ister. İlk adım, koşullu erişim ilkesi durum aracı bir oturum açma, test kullanıcının benzetimini yapmak için kullanın. Benzetim etkisi bu oturum açma ilkelerinizi üzerinde sahip ve bir simülasyon rapor oluşturan tahmin eder.  

İlke değerlendirmesi aracı, ayarlarsanız ne başlatmak için:

- **Isabella Simonsen** kullanıcı olarak 
- **Microsoft Azure Management** bulut uygulaması olarak


Tıklayarak **ne** gösteren bir simülasyon raporu oluşturur:

- **İçin Isabella TOU gerektiren** altında **uygulanacak ilkeler** 
- **My TOU** olarak **verme denetimleri**.

![Durum ilkesi aracı](./media/active-directory-conditional-access-tou/79.png)



**Koşullu erişim ilkenizi değerlendirmek için:**

1. Üzerinde [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **ne**.  
 
    ![What If](./media/active-directory-conditional-access-tou/14.png)

2. Tıklayın **kullanıcılar**seçin **Isabella Simonsen**ve ardından **seçin**.

    ![Kullanıcı](./media/active-directory-conditional-access-tou/15.png)

2. Bulut uygulaması seçmek için:

    ![Bulut uygulamaları](./media/active-directory-conditional-access-tou/16.png)

    a. Tıklayın **bulut uygulamaları**.

    b. Üzerinde **bulut uygulamaları sayfası**, tıklayın **uygulamaları Seç**.

    c. **Seç**'e tıklayın.

    d. Üzerinde **seçin** sayfasında **Microsoft Azure Management**ve ardından **seçin**.

    e. Bulut uygulamaları sayfasında tıklayın **Bitti**.

3. Tıklayın **ne**.


## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test

Önceki bölümde, bir sanal oturum değerlendirilecek öğrendiniz. Bir simülasyon ek olarak, koşullu erişim ilkenizi, beklendiği gibi çalıştığından emin olmak için sınamalısınız. 

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) kullanarak, **Isabella Simonsen** test hesabı. Kullanım koşullarınızı kabul etmenizi gerektiren bir iletişim kutusu görüntülenir.

![Kullanım koşulları](./media/active-directory-conditional-access-tou/57.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı ve koşullu erişim ilkesini Sil:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [Azure AD'den kullanıcı silme](fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- İlkeniz silinecek ilkenizi seçin ve ardından **Sil** hızlı erişim araç çubuğu.

    ![Multi-factor authentication](./media/active-directory-conditional-access-tou/33.png)

- Kullanım koşullarınızın silmek için onu seçin ve ardından **koşulları Sil** araç çubuğunda üstte. 

    ![Multi-factor authentication](./media/active-directory-conditional-access-tou/29.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirli uygulamalar için mfa'yı gerekli](./active-directory-conditional-access-app-based-mfa.md)
> [oturumu risk algılandığında erişimi engelle](./active-directory-conditional-access-app-sign-in-risk.md)

