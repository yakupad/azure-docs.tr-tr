---
title: 'Öğretici: Azure Active Directory tümleştirmesiyle RFPIO | Microsoft Docs'
description: Azure Active Directory ve RFPIO arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 59b05814be0be9042e7507cc8d928b5f5feb80ad
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051770"
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Öğretici: Azure Active Directory RFPIO ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile RFPIO tümleştirme konusunda bilgi edinin.

Azure AD ile RFPIO tümleştirme ile aşağıdaki avantajları sağlar:

- Denetleyebilirsiniz kimin RFPIO erişimi, Azure AD'de.
- Otomatik olarak imzalanan için RFPIO (çoklu oturum açma) ile Azure AD hesaplarına açma, kullanıcılarınızın etkinleştirebilirsiniz.
- Hesaplarınız bir merkezi konumda--Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile RFPIO yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliğiniz.
- RFPIO tek bir oturum üzerinde etkin olmayan abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için bir üretim ortamında kullanmanızı önermemekteyiz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri uygulayın:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa alabileceğiniz bir [bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Senaryo açıklaması
Bu öğreticide, Azure AD çoklu oturum açma bir test ortamında test edin. Bu öğreticide açıklanan senaryo iki temel yapı taşları oluşur:

1. Galeriden RFPIO ekleniyor.
2. Yapılandırma ve test Azure AD çoklu oturum açma.

## <a name="add-rfpio-from-the-gallery"></a>Galeriden RFPIO Ekle
Azure AD'de RFPIO tümleştirmesini yapılandırmak için RFPIO Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

### <a name="to-add-rfpio-from-the-gallery"></a>Galeriden RFPIO eklemek için

1. İçinde  **[Azure portalında](https://portal.azure.com)**, sol gezinti bölmesinde seçin **Azure Active Directory** simgesi. 

    ![Active Directory][1]

2. Seçin **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Uygulamalar][2]
    
3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst kısmındaki düğmesi.

    ![Uygulamalar][3]

4. Arama kutusuna **RFPIO**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/tutorial_rfpio_search.png)

5. Sonuçlar panelinde seçin **RFPIO**ve ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme
Bu bölümde, yapılandırın ve Azure AD çoklu oturum açma "Britta Simon" adlı bir test kullanıcı tabanlı RFPIO sınayın.

Tek iş için oturum açma için Azure AD RFPIO karşılığı kullanıcının Azure AD'de kullanıcı arasındaki ilişki nedir bilmesi gerekir. Diğer bir deyişle, bir Azure AD kullanıcısının RFPIO ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

RFPIO içinde değerini atayın **kullanıcı adı** değerini Azure AD'de **kullanıcıadı** bağlantı kurmak için.

Yapılandırma ve Azure AD çoklu oturum açma RFPIO ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configuring-azure-ad-single-sign-on)**--bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Bir Azure AD test kullanıcısı oluşturma](#creating-an-azure-ad-test-user)**--Azure AD çoklu oturum açma Britta Simon ile test etmek için.
3. **[RFPIO test kullanıcısı oluşturma](#creating-a-rfpio-test-user)**  --bir karşılığı Britta simon'un kullanıcı Azure AD gösterimini bağlı RFPIO sağlamak için.
4. **[Azure AD test kullanıcı atama](#assigning-the-azure-ad-test-user)**--Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
5. **[Çoklu oturum açmayı test](#testing-single-sign-on)**  --yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin ve RFPIO uygulamanızda çoklu oturum açmayı yapılandırın.

**Azure AD çoklu oturum açma ile RFPIO yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında, üzerinde **RFPIO** uygulama tümleştirme sayfasını tıklatın **çoklu oturum açma**.

    ![Çoklu oturum açmayı yapılandırın][4]

2. Üzerinde **çoklu oturum açma** iletişim kutusunda **modu** olarak **SAML tabanlı oturum açma** çoklu oturum açmayı etkinleştirmek için.
 
    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. Üzerinde **RFPIO etki alanı ve URL'ler** uygulamada yapılandırmak isterseniz, bölümü **IDP** başlatılan modu:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_url.png)

    a. İçinde **tanımlayıcı** metin kutusuna URL'yi yazın: `https://www.rfpio.com`

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Denetleme **Gelişmiş URL ayarlarını göster**.

    c. İçinde **geçiş durumu** metin kutusuna bir dize değeri. İlgili kişi [RFPIO Destek ekibine](https://www.rfpio.com/contact/) bu değeri alınamıyor. 

4. Denetleme **Gelişmiş URL ayarlarını göster**. Uygulamada yapılandırmak istiyorsanız **SP** başlatılan modu: 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_url2.png)

    İçinde **oturum açma URL'si** metin kutusuna URL'yi yazın: `https://www.app.rfpio.com`

5. Üzerinde **SAML imzalama sertifikası** bölümünde **meta veri XML** ve bilgisayarınızda meta veri dosyasını kaydedin.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Tıklayın **Kaydet** düğmesi.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_general_400.png)

7. Bir oturum açma için farklı bir web tarayıcı penceresinde **RFPIO** yönetici olarak Web sitesi.

8. Üzerindeki alt sol üst köşedeki açılır menüye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app1.png)

9. Tıklayarak **kuruluş ayarlarına**. 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app2.png)

10. Tıklayarak **özellikler ve tümleştirme**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app4.png)

11. İçinde **SAML SSO yapılandırma** tıklayın **Düzenle**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app3.png)

12. Bu bölümde, eylemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app5.png)
    
    a. İçeriğini kopyalayın **indirilen meta veri XML** yapıştırın **kimlik Yapılandırması** alan.

    > [!NOTE]
    >İçeriği kopyalamak için indirilen **meta veri XML** kullanım **not defteri ++** veya uygun **XML Düzenleyicisi**. 

    b. Tıklayın **doğrulama**.

    c. ' I tıklattıktan sonra **doğrulama**Çevir, **SAML(Enabled)** açık.

    d. Tıklayın **gönderme**.

> [!TIP]
> İçindeki bu yönergeleri kısa bir sürümünü artık okuyabilir [Azure portalında](https://portal.azure.com), uygulamayı hazırlama ayarladığınız sırada!  Bu uygulamadan ekledikten sonra **Active Directory > Kurumsal uygulamalar** bölümünde, tıklamanız yeterlidir **çoklu oturum açma** aracılığıyla katıştırılmış belgelere erişebilir ve sekmesinde  **Yapılandırma** alttaki bölümü. Daha fazla bilgi edinebilirsiniz embedded belgeleri özelliği hakkında: [Azure AD'ye embedded belgeleri]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma
Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

![Azure AD kullanıcısı oluşturun][100]

**Azure AD'de bir test kullanıcısı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. İçinde **Azure portalında**, sol gezinti bölmesinde **Azure Active Directory** simgesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_01.png) 

2. Kullanıcıların listesini görüntülemek için Git **kullanıcılar ve gruplar** tıklatıp **tüm kullanıcılar**.
    
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_02.png) 

3. Açmak için **kullanıcı** iletişim kutusunda, tıklayın **Ekle** iletişim kutusunun üst kısmındaki.
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_03.png) 

4. Üzerinde **kullanıcı** iletişim sayfasında, aşağıdaki adımları gerçekleştirin:
 
    ![Bir Azure AD test kullanıcısı oluşturma](./media/rfpio-tutorial/create_aaduser_04.png) 

    a. İçinde **adı** metin kutusuna **BrittaSimon**.

    b. İçinde **kullanıcı adı** metin kutusuna **e-posta adresi** BrittaSimon biri.

    c. Seçin **Göster parola** ve değerini yazma **parola**.

    d. **Oluştur**’a tıklayın.
 
### <a name="create-a-rfpio-test-user"></a>RFPIO test kullanıcısı oluşturma

RFPIO için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların RFPIO sağlanması gerekir.  
RFPIO söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. RFPIO şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Üzerindeki alt sol üst köşedeki açılır menüye tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app1.png)

3. Tıklayarak **kuruluş ayarlarına**. 

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app2.png)

4. Tıklayın **TAKIM ÜYELERİ**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app6.png)

5. Tıklayarak **ÜYELER Ekle**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app7.png)

6. İçinde **eklediğiniz yeni üyeler** bölümü. Eylemleri gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/app8.png)

    a. ENTER **e-posta adresi** içinde **her satırda bir e-posta girin** alan.

    b. Kaynakta seçin **rol** gereksinimlerinize göre.

    c. Tıklayın **ÜYELER Ekle**.
        
    > [!NOTE]
    > Azure Active Directory hesap sahibinin e-posta alır ve etkin hale gelir önce hesabını onaylamak için bir bağlantı izler.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için RFPIO erişim vererek Britta Simon etkinleştirin.

![Kullanıcı Ata][200] 

**Britta Simon RFPIO için atamak için aşağıdaki adımları gerçekleştirin:**

1. Azure portalında uygulama görünümü açtığınız dizin görünümüne gidin ve Git **kurumsal uygulamalar** ardından **tüm uygulamaları**.

    ![Kullanıcı Ata][201] 

2. Uygulamalar listesinde **RFPIO**.

    ![Çoklu oturum açmayı yapılandırın](./media/rfpio-tutorial/tutorial_rfpio_app.png) 

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcı Ata][202] 

4. Tıklayın **Ekle** düğmesi. Ardından **kullanıcılar ve gruplar** üzerinde **atama Ekle** iletişim.

    ![Kullanıcı Ata][203]

5. Üzerinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** kullanıcıları listesinde.

6. Tıklayın **seçin** düğmesini **kullanıcılar ve gruplar** iletişim.

7. Tıklayın **atama** düğmesini **atama Ekle** iletişim.
    
### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim panelinde RFPIO kutucuğa tıkladığınızda, otomatik olarak RFPIO uygulamanıza açan.
Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](tutorial-list.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/rfpio-tutorial/tutorial_general_01.png
[2]: ./media/rfpio-tutorial/tutorial_general_02.png
[3]: ./media/rfpio-tutorial/tutorial_general_03.png
[4]: ./media/rfpio-tutorial/tutorial_general_04.png

[100]: ./media/rfpio-tutorial/tutorial_general_100.png

[200]: ./media/rfpio-tutorial/tutorial_general_200.png
[201]: ./media/rfpio-tutorial/tutorial_general_201.png
[202]: ./media/rfpio-tutorial/tutorial_general_202.png
[203]: ./media/rfpio-tutorial/tutorial_general_203.png

