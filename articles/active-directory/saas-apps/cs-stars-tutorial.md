---
title: 'Öğretici: Azure Active Directory Tümleştirme ile CS yıldız | Microsoft Docs'
description: Çoklu oturum açma Azure Active Directory ve CS yıldız arasında yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5704d151-afb8-40a4-b286-8bacd4f279ee
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: e0c601b602d2c4fd495cb4f55cc44b2254fa70cf
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218678"
---
# <a name="tutorial-azure-active-directory-integration-with-cs-stars"></a>Öğretici: Azure Active Directory Tümleştirme CS yıldız ile

Bu öğreticide, Azure Active Directory (Azure AD) ile CS yıldız tümleştirmek öğrenin.

CS yıldız Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

- CS yıldız erişimi, Azure AD'de kontrol edebilirsiniz
- Azure AD hesaplarına otomatik olarak CS yıldız (çoklu oturum açma) açan kullanıcılarınıza etkinleştirebilirsiniz
- Hesaplarınızı bir merkezi konumda - Azure portalında yönetebilir

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz: [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirme CS yıldız ile yapılandırmak için aşağıdaki öğeleri gerekir:

- Bir Azure AD aboneliği
- Bir CS yıldız çoklu oturum açma etkin abonelik

> [!NOTE]
> Bu öğreticide adımları test etmek için bir üretim ortamı'nı kullanarak önermiyoruz.

Bu öğreticide test adımları için bu önerileri uygulamanız gerekir:

- Gerekli olmadığı sürece, üretim ortamınızın kullanmayın.
- Bir Azure AD deneme ortam yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide gösterilen senaryo iki ana yapı taşlarını oluşur:

1. Galeriden CS yıldız ekleme
2. Çoklu oturum açmayı yapılandırma ve Azure AD sınama

## <a name="adding-cs-stars-from-the-gallery"></a>Galeriden CS yıldız ekleme
Azure AD CS yıldız tümleştirilmesi yapılandırmak için yönetilen SaaS uygulamaları listenize Galeriden CS yıldız eklemeniz gerekir.

**Galeriden CS yıldız eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde  **[Azure portal](https://portal.azure.com)**, sol gezinti panosunda, tıklatın **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Gidin **kurumsal uygulamalar**. Ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmında düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **CS yıldız**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/tutorial_csstars_search.png)

5. Sonuçlar panelinde seçin **CS yıldız**ve ardından **Ekle** uygulama eklemek için düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/tutorial_csstars_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Çoklu oturum açmayı yapılandırma ve Azure AD sınama
Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma CS "Britta Simon." olarak adlandırılan bir test kullanıcı tabanlı yıldız ile test etme

Tekli çalışmaya oturum için Azure AD CS yıldız karşılık gelen kullanıcı için bir kullanıcı Azure AD'de nedir bilmek ister. Diğer bir deyişle, bir Azure AD kullanıcısının CS yıldız ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

CS yıldızları değerini atayın **kullanıcı adı** değeri olarak Azure AD'de **kullanıcıadı** bağlantı ilişkisi oluşturmak için.

Yapılandırma ve Azure AD çoklu oturum açma CS yıldız ile test etmek için aşağıdaki yapı taşları tamamlanması gerekir:

1. **[Azure AD çoklu oturum açma yapılandırma](#configuring-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[CS yıldız test kullanıcısı oluşturma](#creating-a-cs-stars-test-user)**  - Britta Simon, karşılık gelen kullanıcı Azure AD gösterimini bağlı CS yıldız sağlamak için.
4. **[Azure AD test kullanıcısı atama](#assigning-the-azure-ad-test-user)**  - Azure AD çoklu oturum açma kullanmak Britta Simon etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve çoklu oturum açma CS yıldız uygulamanızda yapılandırın.

**Azure AD çoklu oturum açma ile CS yıldız yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında üzerinde **CS yıldız** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_samlbase.png)

3. Üzerinde **CS yıldız etki alanı ve URL'leri** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_url.png)

    a. İçinde **oturum açma URL'si** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`

    b. İçinde **tanımlayıcısı** metin kutusuna, URL şu biçimi kullanarak bir yazın: `https://<subdomain>.csstars.com/enterprise/`

    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si ve tanımlayıcı ile güncelleştirin. Kişi [CS yıldız istemci destek ekibi](http://www.marshclearsight.com/support/) bu değerleri almak için. 
 
4. Üzerinde **SAML imzalama sertifikası** 'yi tıklatın **meta veri XML** ve meta veri dosyası, bilgisayarınıza kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_certificate.png) 

5. Tıklatın **kaydetmek** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_general_400.png) 
<CS>
6. Çoklu oturum açma yapılandırmak için **CS yıldız** yan, indirilen göndermek için ihtiyacınız **meta veri XML** için [CS yıldız destek ekibi](http://www.marshclearsight.com/support/). 
<CE>

> [!TIP]
> Şimdi bu yönergeleri içinde kısa bir sürümünü okuyabilirsiniz [Azure portal](https://portal.azure.com)uygulaması kuruluyor yaparken!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** sekmesinde ve aracılığıyla katıştırılmış belgelere erişebilir **yapılandırma** alt bölüm. Daha fazla bilgiyi burada embedded belgeler özelliği hakkında: [Azure AD embedded belgeler]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcı oluşturmaktır.

![Azure AD Kullanıcı oluşturma][100]

**Azure AD'de bir test kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portal**, sol gezinti bölmesinde tıklatın **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için şu adrese gidin **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklatın **Ekle** iletişim kutusunun üst kısmında.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/cs-stars-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna, türü **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna, türü **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="creating-a-cs-stars-test-user"></a>CS yıldız test kullanıcısı oluşturma

Bu bölümün amacı Britta Simon CS yıldızları adlı bir kullanıcı oluşturmaktır.

Kullanıcı tarafından oluşturulan CS yıldızları almak için başvurmanız gerekir, [CS yıldız destek ekibi](http://www.marshclearsight.com/support/).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atama

Bu bölümde, Britta CS yıldız erişim vererek, Azure çoklu oturum açma kullanılacak Simon etkinleştirin.

![Kullanıcı atama][200] 

**CS yıldız Britta Simon atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulamaları görünümünü açın ve ardından dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı atama][201] 

2. Uygulamalar listesinde **CS yıldız**.

    ![Çoklu oturum açmayı yapılandırın](./media/cs-stars-tutorial/tutorial_csstars_app.png) 

3. Soldaki menüde tıklatın **kullanıcılar ve gruplar**.

    ![Kullanıcı atama][202] 

4. Tıklatın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **eklemek atama** iletişim.

    ![Kullanıcı atama][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklatın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklatın **atamak** düğmesini **eklemek atama** iletişim.
    
### <a name="testing-single-sign-on"></a>Çoklu oturum açmayı test etme

Bu bölümün amacı erişim paneli kullanılarak Azure AD çoklu oturum açma yapılandırmanızı test etmektir.  
Erişim paneli CS yıldız parçasında tıklattığınızda, otomatik olarak CS yıldız uygulamanıza açan.
 

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme ile nasıl öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/cs-stars-tutorial/tutorial_general_04.png

[100]: ./media/cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/cs-stars-tutorial/tutorial_general_202.png
[203]: ./media/cs-stars-tutorial/tutorial_general_203.png

