---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle Pantheon | Microsoft Docs'
description: Azure Active Directory ve Pantheon arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 2f77fedddd84303ae489379a62a3b1845127b16b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051828"
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a>Öğretici: Azure Active Directory Pantheon ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Pantheon tümleştirme konusunda bilgi edinin.

Azure AD ile Pantheon tümleştirme ile aşağıdaki avantajları sağlar:

- Pantheon erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için Pantheon (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Pantheon yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik Pantheon çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeriden Pantheon ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-pantheon-from-the-gallery"></a>Galeriden Pantheon ekleme
Azure AD'de Pantheon tümleştirmesini yapılandırmak için Pantheon Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Pantheon eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Pantheon**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pantheon-tutorial/tutorial_pantheon_search.png)

5. Sonuçlar panelinde seçin **Pantheon**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı Pantheon sınayın.

Tek iş için oturum açma için Azure AD ne Pantheon karşılığı kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Pantheon ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Pantheon içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Pantheon ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Pantheon test kullanıcısı oluşturma](#creating-a-pantheon-test-user)**  - kullanıcı Azure AD gösterimini bağlı Pantheon Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve Pantheon uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile Pantheon yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Pantheon** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. Üzerinde **Pantheon etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_pantheon_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `urn:auth0:pantheon:<orgname>-SSO`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Pantheon Destek ekibine](https://pantheon.io/docs/getting-support/) bu değerleri almak için.

4. Pantheon uygulama, kullanıcının e-posta adresiyle UserIdentifier öznitelik değeri ayarlamanızı gerektiren belirli biçiminde SAML onayı bekliyor. Varsayılan olarak, Azure AD UserPrincipalName UserIdentifier özniteliği için kullanır. Ancak başarılı tümleştirme için kullanıcının e-posta adresiyle eşleşecek şekilde bu değeri ayarlamanız gerekir. Tümleştirme yalnızca doğru eşleme yaptıktan sonra çalışır.

    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_attribute.png)    


5. Üzerinde **SAML imzalama sertifikası** bölümünde **Certificate(Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_pantheon_certificate.png)

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_general_400.png)

7. Üzerinde **Pantheon yapılandırma** bölümünde **yapılandırma Pantheon** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_pantheon_configure.png) 

8. Çoklu oturum açmayı yapılandırma **Pantheon** tarafı, indirilen göndermek için ihtiyacınız **sertifika** ve **SAML çoklu oturum açma hizmeti URL'si** için [Pantheon Destek](https://pantheon.io/docs/getting-support/).

     > [!Note]
     > Ayrıca, bu bağlantıyı etkinleştirmek istediğinizde tarih saat ve e-posta etki alanı bilgileri vermeniz gerekir. Bununla ilgili daha fazla ayrıntı bulabilirsiniz [burada](https://pantheon.io/docs/sso-organizations/)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/pantheon-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pantheon-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pantheon-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/pantheon-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-pantheon-test-user"></a>Pantheon test kullanıcısı oluşturma

Bu bölümde, Britta Simon Pantheon içinde adlı bir kullanıcı oluşturun. Lütfen izleyin Pantheon içinde kullanıcı eklemek için aşağıdaki adımları. 

>[!NOTE] 
>Kullanıcı çalışmak SSO için Pantheon önce oluşturulması gerekir.

1. Yönetici kimlik bilgileriyle oturum açma Pantheon için.

2. Gidin **kuruluş** Pano sayfası.
 
3. Tıklayın **kişiler**.

4. Tıklayın **Kullanıcı Ekle**.

5. Kullanıcının e-posta adresini girin.

6. Kullanıcı rolü seçin.

7. Tıklayın **Kullanıcı Ekle**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için Pantheon erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Pantheon için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Pantheon**.

    ![Çoklu oturum açmayı yapılandırın](./media/pantheon-tutorial/tutorial_pantheon_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Pantheon kutucuğa tıkladığınızda, otomatik olarak Pantheon uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pantheon-tutorial/tutorial_general_01.png
[2]: ./media/pantheon-tutorial/tutorial_general_02.png
[3]: ./media/pantheon-tutorial/tutorial_general_03.png
[4]: ./media/pantheon-tutorial/tutorial_general_04.png

[100]: ./media/pantheon-tutorial/tutorial_general_100.png

[200]: ./media/pantheon-tutorial/tutorial_general_200.png
[201]: ./media/pantheon-tutorial/tutorial_general_201.png
[202]: ./media/pantheon-tutorial/tutorial_general_202.png
[203]: ./media/pantheon-tutorial/tutorial_general_203.png

