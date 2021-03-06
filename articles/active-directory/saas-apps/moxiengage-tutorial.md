---
title: 'Öğretici: Azure Active Directory Tümleştirme ile Moxi etkileşim kurun | Microsoft Docs'
description: Azure Active Directory ve Moxi ilgisini arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1135a879-8f00-43b0-ac8a-831593d9586d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jeedes
ms.openlocfilehash: 59546e19190e11a1f32b24e0d7b8fb293556c69c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046133"
---
# <a name="tutorial-azure-active-directory-integration-with-moxi-engage"></a>Öğretici: Azure Active Directory Tümleştirme ile Moxi etkileşim kurun

Bu öğreticide, Azure Active Directory (Azure AD) ile tümleştirme Moxi etkileşim kurun öğrenin.

Azure AD ile tümleştirme Moxi ilgisini ile aşağıdaki avantajları sağlar:

- Kimlerin erişebildiğini Moxi etkileşim kurmak için Azure AD'de denetleyebilirsiniz
- Azure AD hesaplarına otomatik olarak imzalanan Moxi etkileşim kurmak için (çoklu oturum açma) açma, kullanıcılarınızın etkinleştirebilirsiniz
- Hesaplarınız bir merkezi konumda - Azure portalında yönetebilirsiniz.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Moxi ilgisini ile yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz
- Bir Moxi ilgisini çoklu oturum açma etkin aboneliği

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide özetlenen senaryo iki temel yapı taşları oluşur:

1. Galeri ekleme Moxi etkileşim kurun
2. Yapılandırma ve test Azure AD çoklu oturum açma

## <a name="adding-moxi-engage-from-the-gallery"></a>Galeri ekleme Moxi etkileşim kurun
Tümleştirilmesi Moxi etkileşim kurun, Azure AD'de yapılandırmak için Moxi etkileşim kurun galerideki yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Moxi etkileşim eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **Moxi ilgisini**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxiengage-tutorial/tutorial_moxiengage_search.png)

5. Sonuçlar panelinde seçin **Moxi ilgisini**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxiengage-tutorial/tutorial_moxiengage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yapılandırma ve test Azure AD çoklu oturum açma
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma "Britta Simon." adlı bir test kullanıcı tabanlı Moxi ilgisini ile test etme

Tek iş için oturum açma için Azure AD ne karşılık gelen kullanıcı Moxi etkileşim kurmak için bir kullanıcı Azure AD'de olduğunu bilmeniz gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının Moxi etkileşim kurun, ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Değeri atamak Moxi etkileşim kurun **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma Moxi ilgisini ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[Moxi ilgisini bir test kullanıcısı oluşturma](#creating-a-moxi-engage-test-user)**  - bir karşılığı Britta simon'un Moxi kullanıcı Azure AD gösterimini bağlı etkileşim sağlamak için.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açma testi](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma Moxi ilgisini uygulamanızı yapılandırın.

**Azure AD çoklu oturum açma Moxi ilgisini ile yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **Moxi etkileşim kurun** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/moxiengage-tutorial/tutorial_moxiengage_samlbase.png)

3. Üzerinde **Moxi katılım etki alanı ve URL'ler** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/moxiengage-tutorial/tutorial_moxiengage_url.png)

    İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://svc.<moxiworks-integration-domain>/service/v1/auth/inbound/saml/aad`

    > [!NOTE] 
    > Bu değer, gerçek değil. Bu değer, gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [Moxi ilgisini istemci Destek ekibine](mailto:support@moxiworks.com) bu değeri alınamıyor. 
 
4. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/moxiengage-tutorial/tutorial_moxiengage_certificate.png) 

5. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/moxiengage-tutorial/tutorial_general_400.png)

6. Çoklu oturum açmayı yapılandırmak için **Moxi ilgisini** tarafı, indirilen göndermek için ihtiyacınız **meta veri XML** için [Moxi etkileşim kurun, Destek ekibine](mailto:support@moxiworks.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxiengage-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxiengage-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxiengage-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/moxiengage-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-moxi-engage-test-user"></a>Moxi ilgisini bir test kullanıcısı oluşturma

Bu bölümde, Britta Simon Moxi etkileşim kurun adlı bir kullanıcı oluşturun. Çalışmak [Moxi etkileşim kurun, Destek ekibine](mailto:support@moxiworks.com) Moxi ilgisini platform kullanıcıları eklemek için.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcı atama

Bu bölümde, erişim izni verme Moxi etkileşim kurmak için Azure çoklu oturum açmayı kullanmak Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon Moxi etkileşim kurmak için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **Moxi ilgisini**.

    ![Çoklu oturum açmayı yapılandırın](./media/moxiengage-tutorial/tutorial_moxiengage_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Moxi ilgisini kutucuğa tıkladığınızda, uygulama Moxi etkileşim kurmak için otomatik oturum açma almanız gerekir.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ek kaynaklar

* [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/moxiengage-tutorial/tutorial_general_01.png
[2]: ./media/moxiengage-tutorial/tutorial_general_02.png
[3]: ./media/moxiengage-tutorial/tutorial_general_03.png
[4]: ./media/moxiengage-tutorial/tutorial_general_04.png

[100]: ./media/moxiengage-tutorial/tutorial_general_100.png

[200]: ./media/moxiengage-tutorial/tutorial_general_200.png
[201]: ./media/moxiengage-tutorial/tutorial_general_201.png
[202]: ./media/moxiengage-tutorial/tutorial_general_202.png
[203]: ./media/moxiengage-tutorial/tutorial_general_203.png

