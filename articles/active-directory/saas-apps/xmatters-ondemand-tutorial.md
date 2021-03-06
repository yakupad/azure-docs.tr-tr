---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle xMatters OnDemand | Microsoft Docs'
description: Azure Active Directory ve xMatters OnDemand arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 60991f2780de86f0b15f64def6b1776316f974d4
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055632"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Öğretici: Azure Active Directory xMatters OnDemand ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile xMatters OnDemand tümleştirme konusunda bilgi edinin.

Azure AD ile xMatters OnDemand tümleştirme ile aşağıdaki avantajları sağlar:

- OnDemand xMatters erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için xMatters OnDemand (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi OnDemand xMatters ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir xMatters OnDemand çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden xMatters OnDemand ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Galeriden xMatters OnDemand ekleme
Azure AD'de xMatters OnDemand tümleştirmesini yapılandırmak için xMatters OnDemand Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden xMatters OnDemand eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **xMatters OnDemand**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. Sonuçlar panelinde seçin **xMatters OnDemand**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı OnDemand tabanlı xMatters ile test edin.

Tek iş için oturum açma için Azure AD hangi karşılık gelen kullanıcı xMatters OnDemand bir kullanıcının Azure AD'de olduğunu bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının xMatters ilgili kullanıcı arasında bir bağlantı ilişki OnDemand kurulması gerekir.

Değerini xMatters OnDemand, Ata **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma xMatters OnDemand ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[XMatters OnDemand test kullanıcısı oluşturma](#creating-a-xmatters-ondemand-test-user)**  - Britta Simon xMatters kullanıcı Azure AD gösterimini bağlı OnDemand içinde bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma, xMatters OnDemand uygulama yapılandırın.

**Azure AD çoklu oturum açma OnDemand xMatters ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **xMatters OnDemand** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. Üzerinde **xMatters OnDemand etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [xMatters OnDemand Destek ekibine](https://www.xmatters.com/company/contact-us/) bu değerleri almak için.

4. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve sertifika dosyasını yerel olarak kaydedin **c:\\XMatters OnDemand.cer**.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)

    > [!IMPORTANT]
    > Sertifikayı iletmek için ihtiyacınız [xMatters OnDemand Destek ekibine](https://www.xmatters.com/company/contact-us/). Çoklu oturum açma yapılandırması son haline getir önce xMatters destek ekibi tarafından karşıya yüklenecek sertifika gerekir. 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_general_400.png)

6. Üzerinde **xMatters OnDemand yapılandırma** bölümünde **xMatters OnDemand yapılandırma** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. Farklı bir web tarayıcı penceresinde XMatters OnDemand şirketinizin sitesi için bir yönetici olarak oturum açın.

8. Üst araç çubuğunda tıklatın **yönetici**ve ardından **şirket ayrıntıları** sol taraftaki gezinti çubuğunda.

    ![Yönetici](./media/xmatters-ondemand-tutorial/IC776795.png "yönetici")

9. Üzerinde **SAML yapılandırma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML yapılandırma](./media/xmatters-ondemand-tutorial/IC776796.png "SAML yapılandırma")

    a. Seçin **etkinleştirme SAML**.

    b. İçinde **kimlik sağlayıcı kimliği** metin kutusu, yapıştırma **SAML varlık kimliği** Azure portaldan kopyaladığınız değeri.

    c. İçinde **üzerinde çoklu oturum URL'si** metin kutusu, yapıştırma **SAML çoklu oturum açma hizmeti URL'si** Azure portaldan kopyaladığınız değeri.

    d. İçinde **çoklu oturum kapatma URL'si** metin kutusu, yapıştırma **oturum kapatma URL'si**, hangi Azure portaldan kopyaladığınız.

    e. Şirket Ayrıntıları sayfası, en üstte tıklayarak **Değişiklikleri Kaydet**.

    ![Şirket ayrıntıları](./media/xmatters-ondemand-tutorial/IC776797.png "şirket ayrıntıları")

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Bir Azure AD test kullanıcısı oluşturma](./media/xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.

### <a name="creating-a-xmatters-ondemand-test-user"></a>XMatters OnDemand test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon OnDemand xMatters içinde adlı bir kullanıcı oluşturmaktır. OnDemand xMatters otomatik kullanıcı hazırlama, varsayılan olarak etkin olan destekler. Daha fazla ayrıntı bulabilirsiniz [burada](xmatters-ondemand-provisioning-tutorial.md) otomatik kullanıcı sağlamayı yapılandırma.

**Kullanıcı el ile oluşturmanız gerekiyorsa, aşağıdaki adımları uygulayın:**

1. Oturum açın, **XMatters OnDemand** Kiracı.

2.  Tıklayın **kullanıcılar** sekmesini ve ardından **Kullanıcı Ekle**.

    ![Kullanıcılar](./media/xmatters-ondemand-tutorial/IC781048.png "kullanıcılar")

3. İçinde **kullanıcı ekleme** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/xmatters-ondemand-tutorial/IC781049.png "kullanıcı ekleme")

    a. Seçin **etkin**.

    b. İçinde **kullanıcı kimliği** metin kutusu, kullanıcının kullanıcı kimliği türü ister Brittasimon@contoso.com.

    c. İçinde **ad** metin adı Britta gibi kullanıcı türü.

    d. İçinde **Soyadı** metin son Simon gibi kullanıcının adını yazın.

    e. İçinde **Site** metin Enter geçerli sitenin geçerli bir Azure AD hesabı sağlamak istediğiniz.

    f. **Kaydet**’e tıklayın.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma xMatters OnDemand erişim vererek kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon OnDemand xMatters için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **xMatters OnDemand**.

    ![Çoklu oturum açmayı yapılandırın](./media/xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

OnDemand kutucuk erişim Paneli'nde xMatters tıkladığınızda, otomatik olarak xMatters OnDemand uygulama için açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)
* [Kullanıcı sağlamayı yapılandırma](xmatters-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/xmatters-ondemand-tutorial/tutorial_general_203.png
