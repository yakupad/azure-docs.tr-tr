---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle tanı | Microsoft Docs'
description: Azure Active Directory ve tanı arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: a4ea4272b11c167d95840b45b39634abea8c1cea
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051607"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Öğretici: Azure Active Directory tümleştirmesiyle tanı

Bu öğreticide, Azure Active Directory (Azure AD) ile tanı tümleştirme konusunda bilgi edinin.

Tanı Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- Tanı erişimi, Azure AD'de denetleyebilirsiniz
- Otomatik olarak imzalanan için tanı (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile tanı yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Abonelik bir tanı çoklu oturum açma etkin

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme burada alabilirsiniz: [deneme teklifi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Tanı galeri ekleme
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-recognize-from-the-gallery"></a>Tanı galeri ekleme
Azure AD'de tanı tümleştirmesini yapılandırmak için tanı Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden tanı eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **tanı**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/recognize-tutorial/tutorial_recognize_search.png)

5. Sonuçlar panelinde seçin **tanı**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı tanı sınayın.

Tek iş için oturum açma için Azure AD ne tanı karşılık gelen kullanıcı için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısı ve tanı ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Tanı, değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma tanı ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Tanı test kullanıcısı oluşturma](#creating-a-recognize-test-user)**  - kullanıcı Azure AD gösterimini bağlı tanı Britta simon'un bir karşılığı vardır.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve tanı uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile tanı yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **tanı** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/recognize-tutorial/tutorial_recognize_samlbase.png)

3. Üzerinde **tanımak etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/recognize-tutorial/tutorial_recognize_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://recognizeapp.com/<your-domain>/saml/sso`

    b. İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. İlgili kişi [tanımak istemci Destek ekibine](mailto:support@recognizeapp.com) ve oturum açma URL'si almak için tanımlayıcı değeri öğreticinin ilerleyen bölümlerinde açıklanan SSO ayarları bölümünden, hizmet sağlayıcısı meta verileri URL'sini açarak alabilirsiniz. . 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **sertifika (Base64)** ve bilgisayarınızdaki sertifika dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/recognize-tutorial/tutorial_recognize_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/recognize-tutorial/tutorial_general_400.png)

6. Üzerinde **Tanı Yapılandırması** bölümünde **Yapılandırma tanı** açmak için **yapılandırma oturum açma** penceresi. Kopyalama **oturum kapatma URL'si, SAML varlık kimliği ve SAML çoklu oturum açma hizmeti URL'si** gelen **hızlı başvuru bölümü.**

    ![Çoklu oturum açmayı yapılandırın](./media/recognize-tutorial/tutorial_recognize_configure.png) 

7. Farklı bir web tarayıcı penceresinde tanı kiracınıza yönetici olarak oturum.

8. Sağ üst köşede tıklayın **menü**. Git **şirket Yöneticisi**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_000.png)

9. Sol gezinti bölmesinde **ayarları**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_001.png)

10. Aşağıdaki adımları gerçekleştirin **SSO ayarlarını** bölümü.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. Olarak **SSO etkinleştirme**seçin **ON**.

    b. İçinde **IDP varlık kimliği** metin değerini yapıştırın **SAML varlık kimliği** , Azure Portalı'ndan kopyaladığınız.
    
    c. İçinde **Sso hedef url** metin değerini yapıştırın **SAML çoklu oturum açma hizmeti URL'si** , Azure Portalı'ndan kopyaladığınız.
    
    d. İçinde **Slo hedef url** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız. 
    
    e. İndirilen açın **sertifika (Base64)** dosyasını Not Defteri'nde, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **sertifika** metin.
    
    f. Tıklayın **ayarlarını kaydetmek** düğmesi. 

11. Yanında **SSO ayarlarını** bölümünde, altında URL'yi kopyalayın **hizmet sağlayıcısı meta verileri URL'sini**.
   
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_003.png)

12. Açık **meta veri URL'si bağlantı** meta veri belgesini indirmek için boş bir tarayıcı altında. Ardından EntityDescriptor value(entityID) dosyasından kopyalayın ve yapıştırın **tanımlayıcı** metin kutusunda **tanımak etki alanı ve URL'ler bölüm** Azure portalında.
    
    ![Çoklu oturum açma üzerinde uygulama tarafı yapılandırma](./media/recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/recognize-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/recognize-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/recognize-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/recognize-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-recognize-test-user"></a>Tanı test kullanıcısı oluşturma

Tanı açarken Azure AD kullanıcılarının etkinleştirmek için bunların tanı sağlanmalıdır. Tanı söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

Bu uygulama SCIM sağlama desteklemez ancak kullanıcılar sağlayan bir alternatif kullanıcı eşitleme sahiptir. 

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Tanı şirket sitenize yönetici olarak oturum açın.

2. Sağ üst köşede tıklayın **menü**. Git **şirket Yöneticisi**.

3. Sol gezinti bölmesinde **ayarları**.

4. Aşağıdaki adımları gerçekleştirin **kullanıcı eşitleme** bölümü.
   
   ![Yeni kullanıcı](./media/recognize-tutorial/tutorial_recognize_005.png "yeni kullanıcı")
   
   a. Olarak **eşitleme özelliği etkin**seçin **ON**.
   
   b. Olarak **Seç eşitleme sağlayıcısı**seçin **Microsoft / Office 365**.
   
   c. Tıklayın **kullanıcı eşitleme Çalıştır**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, Azure çoklu oturum açma kullanmak için tanı erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon için tanı atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **tanı**.

    ![Çoklu oturum açmayı yapılandırın](./media/recognize-tutorial/tutorial_recognize_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümün amacı, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test sağlamaktır.

Erişim panelinde tanı kutucuğa tıkladığınızda, otomatik olarak tanı uygulamanıza açan. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/recognize-tutorial/tutorial_general_01.png
[2]: ./media/recognize-tutorial/tutorial_general_02.png
[3]: ./media/recognize-tutorial/tutorial_general_03.png
[4]: ./media/recognize-tutorial/tutorial_general_04.png

[100]: ./media/recognize-tutorial/tutorial_general_100.png

[200]: ./media/recognize-tutorial/tutorial_general_200.png
[201]: ./media/recognize-tutorial/tutorial_general_201.png
[202]: ./media/recognize-tutorial/tutorial_general_202.png
[203]: ./media/recognize-tutorial/tutorial_general_203.png

