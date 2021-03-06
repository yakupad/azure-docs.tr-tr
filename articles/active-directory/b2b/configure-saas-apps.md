---
title: Azure Active Directory'de SaaS uygulamaları B2B işbirliği için yapılandırma | Microsoft Docs
description: Azure Active Directory B2B işbirliği için PowerShell ve kod örnekleri
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: f0f291216a3031d50d304c02b97786f23d1a6267
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267536"
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a>SaaS uygulamaları B2B işbirliği için yapılandırma

Azure AD ile tümleştirmek çoğu uygulamaları ile Azure Active Directory (Azure AD) B2B işbirliği çalışır. Bu bölümde, Azure AD B2B ile kullanmak için bazı yaygın SaaS uygulamaları yapılandırmaya yönelik yönergeler size rehberlik eder.

Uygulamaya özgü yönergeleri göz önünde bulundurmanız önce bazı kurallar altın şunlardır:

* Uygulamaların çoğu için Kullanıcı Kurulumu el ile gerçekleştirilmesi gerekir. Diğer bir deyişle, kullanıcı uygulamada el ile oluşturulması gerekir.

* Dropbox gibi otomatik kurulum destekleyen uygulamalar için ayrı davetleri uygulamalardan oluşturulur. Kullanıcıların her daveti kabul etmek emin olması gerekir.

* Her zaman kullanıcı öznitelikleri karıştırılmış kullanıcı profili diski (UPD) Konuk kullanıcılar ile ilgili tüm sorunları hafifletmek için ayarlanmış **kullanıcı tanımlayıcısı** için **user.mail**.


## <a name="dropbox-business"></a>Dropbox iş

Kullanıcıların kendi kuruluş hesabını kullanarak oturum sağlamak için Dropbox Azure AD güvenlik onaylama işlemi biçimlendirme dili (SAML) kimlik sağlayıcısı kullanmak için iş el ile yapılandırmanız gerekir. Bunu yapmak için Dropbox iş yapılandırılmamış, bu isteyebilir veya aksi takdirde Azure AD kullanarak kullanıcıların oturum açmasını sağlar.

1. Azure AD ile Dropbox iş uygulama eklemek için seçin **kurumsal uygulamalar** sol bölmesinde ve ardından **Ekle**.

  ![Kuruluş uygulamaları sayfasında "Ekle" düğmesi](media/configure-saas-apps/add-dropbox.png)

2. İçinde **bir uygulama eklemek** penceresinde girin **dropbox** arama kutusuna ve ardından **iş için Dropbox** sonuçlar listesinde.

  ![Uygulama Sayfası Ekle "dropbox" arayın](media/configure-saas-apps/add-app-dialog.png)

3. Üzerinde **çoklu oturum açma** sayfasında, **çoklu oturum açma** sol bölmede ve ardından girin **user.mail** içinde **kullanıcı tanımlayıcısı** kutusu. (Bunu UPN varsayılan olarak ayarlanır.)

  ![Uygulama için çoklu oturum açmayı yapılandırma](media/configure-saas-apps/configure-app-sso.png)

4. Dropbox yapılandırması için kullanılacak sertifikayı yüklemek üzere seçin **yapılandırma DropBox**ve ardından **SAML çoklu oturum üzerinde hizmet URL'si** listesinde.

  ![Dropbox yapılandırması için sertifika indirme](media/configure-saas-apps/download-certificate.png)

5. Oturum açtığınızda Dropbox oturum açma URL'si ile **çoklu oturum açma** sayfası.

  ![Dropbox oturum açma sayfası](media/configure-saas-apps/sign-in-to-dropbox.png)

6. Menüsünde seçin **Yönetici Konsolu**.

  ![Dropbox menüsünde "Yönetici Konsolu" bağlantısı](media/configure-saas-apps/dropbox-menu.png)

7. İçinde **kimlik doğrulaması** iletişim kutusunda **daha fazla**, sertifikayı karşıya yüklemek ve ardından **URL'de oturum** kutusuna, SAML çoklu oturum açma URL'si girin.

  ![Daraltılmış kimlik doğrulama iletişim kutusunda "Daha fazla" bağlantı](media/configure-saas-apps/dropbox-auth-01.png)

  !["Oturum URL'de" genişletilmiş kimlik doğrulama iletişim kutusunda](media/configure-saas-apps/paste-single-sign-on-URL.png)

8. Otomatik kullanıcı kurulum Azure portalında yapılandırmak için seçin **sağlama** sol bölmesinde seçin **otomatik** içinde **sağlama modu** kutusuna ve ardından **Authorize**.

  ![Azure portalında otomatik kullanıcı sağlamayı yapılandırma](media/configure-saas-apps/set-up-automatic-provisioning.png)

Konuk veya üye kullanıcı Dropbox uygulamada ayarlanan sonra Dropbox'tan ayrı bir davet alırsınız. Dropbox çoklu oturum açma kullanmak için davetlilerin daveti bir bağlantıya tıklayarak kabul etmelisiniz.

## <a name="box"></a>Box
SAML protokolünü temel Federasyon kullanarak Azure AD hesabıyla kutusunu konuk kullanıcıların kimliklerini doğrulamak kullanıcıların sağlayabilirsiniz. Bu yordamda, meta veriler için Box.com yükleyin.

1. Box uygulamasına Kurumsal uygulamalardan ekleyin.

2. Çoklu oturum açma şu sırayla yapılandırın:

  ![Kutusunu çoklu oturum açmayı yapılandırın](media/configure-saas-apps/configure-box-sso.png)

 a. İçinde **URL üzerinde oturum** kutusunda, Azure portalında kutusunu, oturum açma URL'si uygun şekilde ayarlandığından emin olun. Bu Box.com kiracınızın URL'dir. Adlandırma kuralına uymalı *https://.box.com*.  
 **Tanımlayıcısı** bu uygulama için geçerli değildir, ancak bu zorunlu bir alan görünmeye devam eder.

 b. İçinde **kullanıcı tanımlayıcısı** kutusuna **user.mail** (için SSO Konuk hesapları için).

 c. Altında **SAML imzalama sertifikası**, tıklatın **yeni sertifika oluştur**.

 d. Azure AD kimlik sağlayıcısı olarak kullanmak için Box.com Kiracı yapılandırmaya başlamak için meta veri dosyası karşıdan yükle ve yerel diskinize kaydedin.

 e. İleri kutusuna meta veri dosyası, çoklu oturum açma sizin için yapılandırır takım destekler.

3. Sol bölmede, Azure AD otomatik kullanıcı kurulumu seçin **sağlama**ve ardından **Authorize**.

  ![Azure AD kutusuna bağlanmak için yetki](media/configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

Dropbox davetlilerin gibi kutusunu davetlilerin Box uygulamasına kendi davetini almak gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Dinamik gruplar ve B2B işbirliği](use-dynamic-groups.md)
- [B2B işbirliği kullanıcı taleplerini eşleme](claims-mapping.md)
- [Office 365 dış paylaşım](o365-external-user.md)

