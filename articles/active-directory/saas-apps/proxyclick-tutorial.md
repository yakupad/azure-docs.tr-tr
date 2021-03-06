---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Proxyclick | Microsoft Docs'
description: Azure Active Directory ve Proxyclick arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeedes
ms.openlocfilehash: 71b9b54e3b8eef1be9f6da7fa812bd8f9d246f47
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051695"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Öğretici: Azure Active Directory Proxyclick ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Proxyclick tümleştirme konusunda bilgi edinin.

Azure AD ile Proxyclick tümleştirme ile aşağıdaki avantajları sağlar:

- Proxyclick erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Proxyclick (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Proxyclick yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Proxyclick çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Proxyclick ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-proxyclick-from-the-gallery"></a>Galeriden Proxyclick ekleme
Azure AD'de Proxyclick tümleştirmesini yapılandırmak için Proxyclick Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Proxyclick eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Proxyclick**seçin **Proxyclick** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Proxyclick](./media/proxyclick-tutorial/tutorial_proxyclick_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Proxyclick sınayın.

Tek iş için oturum açma için Azure AD ne Proxyclick karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Proxyclick ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Proxyclick ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Proxyclick test kullanıcısı oluşturma](#create-a-proxyclick-test-user)**  - kullanıcı Azure AD gösterimini bağlı Proxyclick Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Proxyclick uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Proxyclick yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Proxyclick** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/proxyclick-tutorial/tutorial_proxyclick_samlbase.png)

3. Üzerinde **Proxyclick etki alanı ve URL'ler** bölümünde, uygulamada yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin **IDP** başlatılan modu:

    ![Proxyclick etki alanı ve URL'ler tek oturum açma bilgileri](./media/proxyclick-tutorial/tutorial_proxyclick_url2.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://saml.proxyclick.com/init/<companyId>`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://saml.proxyclick.com/consume/<companyId>`

4. Denetleme **Gelişmiş URL ayarlarını göster** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![Proxyclick etki alanı ve URL'ler tek oturum açma bilgileri](./media/proxyclick-tutorial/tutorial_proxyclick_url1.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://saml.proxyclick.com/init/<companyId>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma, öğreticinin ilerleyen bölümlerinde açıklanan URL ile güncelleştirir.

5. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Sertifika indirme bağlantısı](./media/proxyclick-tutorial/tutorial_proxyclick_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/proxyclick-tutorial/tutorial_general_400.png)

7. Üzerinde **Proxyclick yapılandırma** bölümünde **yapılandırma Proxyclick** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/tutorial_proxyclick_configure.png)

8. Farklı bir web tarayıcı penceresinde Proxyclick şirketinizin sitesi için bir yönetici olarak oturum açın.

9. Seçin **hesabı & ayarları**.

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/configure1.png)

10. Ekranı aşağı kaydırarak **TÜMLEŞTİRMELER** seçip **SAML**.

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/configure2.png)

11. İçinde **SAML** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Proxyclick yapılandırma](./media/proxyclick-tutorial/configure3.png)

    a. Kopyalama **SAML tüketici URL** yapıştırın ve değer **yanıt URL'si** metin kutusunda **Proxyclick etki alanı ve URL'ler** bölümü Azure portalı.

    b. Kopyalama **SAML SSO tekrar yönlendirme URL'sini** yapıştırın ve değer **oturum açma URL'si** ve **tanımlayıcı** metin kutuları içinde **Proxyclick etki alanı ve URL'ler** bölümü Azure portalında.

    c. Seçin **SAML isteği yöntemi** olarak **HTTP yeniden yönlendirme**.

    d. İçinde **veren** metin değerini yapıştırın **SAML varlık kimliği** Azure Portalı'ndan kopyaladığınız değeri.

    e. İçinde **SAML 2.0 uç nokta URL'si** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız.

    f. Not Defteri'nde Azure portalından indirilen sertifika dosyanızı açın ve ardından yapıştırın **sertifika** metin.

    g. Tıklayın **değişiklikleri kaydetmek**.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/proxyclick-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/proxyclick-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/proxyclick-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/proxyclick-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-proxyclick-test-user"></a>Proxyclick test kullanıcısı oluşturma

Proxyclick için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Proxyclick sağlanması gerekir. Proxyclick söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Proxyclick şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Tıklayın **iş arkadaşlarınıza** üst gezinti çubuğundan.

    ![Çalışan Ekle](./media/proxyclick-tutorial/user1.png)

3. Tıklayın **iş arkadaşı Ekle**

    ![Çalışan Ekle](./media/proxyclick-tutorial/user2.png)

4. İçinde **bir iş arkadaşınız ekleme** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/proxyclick-tutorial/user3.png)

    a. İçinde **e-posta** metin kutusuna kullanıcı e-posta adresi türünü ister **brittasimon@contoso.com**.

    b. İçinde **ad** metin kutusu, kullanıcının Britta gibi ilk tür adı.

    c. İçinde **Soyadı** metin son Simon gibi kullanıcı adını yazın.

    d. Tıklayın **kullanıcı ekleme**.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Proxyclick erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Proxyclick için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Proxyclick**.

    ![Uygulamalar listesinde Proxyclick bağlantı](./media/proxyclick-tutorial/tutorial_proxyclick_app.png)  

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Proxyclick kutucuğa tıkladığınızda, otomatik olarak Proxyclick uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/proxyclick-tutorial/tutorial_general_01.png
[2]: ./media/proxyclick-tutorial/tutorial_general_02.png
[3]: ./media/proxyclick-tutorial/tutorial_general_03.png
[4]: ./media/proxyclick-tutorial/tutorial_general_04.png

[100]: ./media/proxyclick-tutorial/tutorial_general_100.png

[200]: ./media/proxyclick-tutorial/tutorial_general_200.png
[201]: ./media/proxyclick-tutorial/tutorial_general_201.png
[202]: ./media/proxyclick-tutorial/tutorial_general_202.png
[203]: ./media/proxyclick-tutorial/tutorial_general_203.png

