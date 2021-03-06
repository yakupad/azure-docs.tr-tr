---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Clever | Microsoft Docs'
description: Azure Active Directory ve Clever arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 5e5f89ebef3f42380f3dcceab50fda0ace9516de
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054225"
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Öğretici: Azure Active Directory Clever ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Clever tümleştirme konusunda bilgi edinin.

Azure AD ile Clever tümleştirme ile aşağıdaki avantajları sağlar:

- Clever erişimi, Azure AD'de kontrol edebilirsiniz.
- Otomatik olarak imzalanan için Clever (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Clever yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir akıllı çoklu oturum açma abonelik etkin.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Clever ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-clever-from-the-gallery"></a>Galeriden Clever ekleme
Azure AD'de Clever tümleştirmesini yapılandırmak için Clever Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Clever eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Azure Active Directory düğmesi][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi][3]

4. Arama kutusuna **Clever**seçin **Clever** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde bulabilse](./media/clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Clever sınayın.

Tek iş için oturum açma için Azure AD ne Clever karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Clever ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Clever içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Clever ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Akıllı test kullanıcısı oluşturma](#create-a-clever-test-user)**  - kullanıcı Azure AD gösterimini bağlı Clever Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve akıllı uygulamanızda çoklu oturum açmayı yapılandırma.

**Azure AD çoklu oturum açma ile Clever yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Clever** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma iletişim kutusu](./media/clever-tutorial/tutorial_clever_samlbase.png)

3. Üzerinde **akıllı etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri Akıllı etki alanı ve URL'ler](./media/clever-tutorial/tutorial_clever_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://clever.com/in/<companyname>`

    b. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://clever.com/oauth/saml/metadata.xml`

    > [!NOTE]
    > Oturum açma URL değeri, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [akıllı istemci Destek ekibine](https://clever.com/about/contact/) bu değeri alınamıyor.

4. Üzerinde **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** kopyalayıp Not Defteri'ne yapıştırın.
    
    ![Çoklu oturum açmayı yapılandırın](./media/clever-tutorial/tutorial_metadataurl.png)

5. Akıllı uygulama eklemek için özel öznitelik eşlemeleri gerektiren belirli bir biçimde SAML onaylamalarını bekliyor, **SAML belirteci öznitelikleri** yapılandırma.

    Aşağıdaki ekran görüntüsü bunun bir örneği gösterilmektedir.

    ![Çoklu oturum açmayı yapılandırın](./media/clever-tutorial/tutorial_clever_07.png)

6. İçinde **kullanıcı öznitelikleri** bölümünde **çoklu oturum açma** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:
    
    | Öznitelik Adı  | Öznitelik Değeri |
    | --------------- | -------------------- |
    | clever.Teacher.credentials.district_username|User.userPrincipalName|
    | clever.Student.credentials.district_username| User.userPrincipalName |
    | firstName  | User.givenName |
    | Soyadı  | User.surname |

    a. Tıklayın **eklemek agentconfigutil** açmak için **öznitelik Ekle** iletişim.

    ![Çoklu oturum açmayı yapılandırın](./media/clever-tutorial/tutorial_attribute_04.png)
    
    ![Çoklu oturum açmayı yapılandırın](./media/clever-tutorial/tutorial_attribute_05.png)
    
    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Gelen **değer** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    d. Bırakın **Namespace** metin kutusu boş.
    
    d. **Tamam**’a tıklayın.
    
7. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açma Kaydet düğmesi yapılandırın](./media/clever-tutorial/tutorial_general_400.png)

8. Farklı bir web tarayıcı penceresinde Bulabilse şirketinizin sitesi için bir yönetici olarak oturum açın.

9. Araç çubuğunda **hızlı oturum açma**.

    ![Hızlı oturum açma](./media/clever-tutorial/ic798984.png "hızlı oturum açma")

    > [!NOTE]
    > Çoklu oturum açmayı Test edebilmek için önce birimine başvurmanız [akıllı istemci Destek ekibine](https://clever.com/about/contact/) arka uçta Office 365 SSO'yu etkinleştirmek üzere.

10. Üzerinde **hızlı oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:
    
      ![Hızlı oturum açma](./media/clever-tutorial/ic798985.png "hızlı oturum açma")
    
      a. Tür **oturum açma URL'si**.
    
      >[!NOTE]
      >**Oturum açma URL'si** özel bir değerdir. İlgili kişi [akıllı istemci Destek ekibine](https://clever.com/about/contact/) bu değeri alınamıyor.
    
      b. Olarak **kimlik sistemi**seçin **ADFS**.

      c. İçinde **meta veri URL'si** metin kutusu, yapıştırma **uygulama Federasyon meta verileri URL'sini** Azure portaldan kopyaladığınız değeri.
    
      d. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

   ![Bir Azure AD test kullanıcısı oluşturma][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, sol bölmede, tıklayın **Azure Active Directory** düğmesi.

    ![Azure Active Directory düğmesi](./media/clever-tutorial/create_aaduser_01.png)

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](./media/clever-tutorial/create_aaduser_02.png)

3. Açmak için **kullanıcı** iletişim kutusu, tıklayın **Ekle** en üstündeki **tüm kullanıcılar** iletişim kutusu.

    ![Ekle düğmesi](./media/clever-tutorial/create_aaduser_03.png)

4. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı iletişim kutusu](./media/clever-tutorial/create_aaduser_04.png)

    a. İçinde **adı** kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** Britta Simon kullanıcı e-posta adresini yazın.

    c. Seçin **Göster parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’a tıklayın.

### <a name="create-a-clever-test-user"></a>Akıllı test kullanıcısı oluşturma

Clever için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların Clever sağlanması gerekir.

Clever durumunda çalışmak [akıllı istemci Destek ekibine](https://clever.com/about/contact/) akıllı platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

>[!NOTE]
>Diğer Akıllı kullanıcı hesabı oluşturma araçlardan kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Clever tarafından sağlanan.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Clever erişim vererek Britta Simon etkinleştirin.

![Kullanıcı rolü atayın][200]

**Britta Simon Clever için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201]

2. Uygulamalar listesinde **Clever**.

    ![Uygulamalar listesinde Clever bağlantı](./media/clever-tutorial/tutorial_clever_app.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı][202]

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Akıllı kutucuğa tıkladığınızda, erişim Paneli'nde, otomatik olarak akıllı uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/clever-tutorial/tutorial_general_01.png
[2]: ./media/clever-tutorial/tutorial_general_02.png
[3]: ./media/clever-tutorial/tutorial_general_03.png
[4]: ./media/clever-tutorial/tutorial_general_04.png

[100]: ./media/clever-tutorial/tutorial_general_100.png

[200]: ./media/clever-tutorial/tutorial_general_200.png
[201]: ./media/clever-tutorial/tutorial_general_201.png
[202]: ./media/clever-tutorial/tutorial_general_202.png
[203]: ./media/clever-tutorial/tutorial_general_203.png

