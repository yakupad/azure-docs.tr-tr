---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma - nasıl çalışır? | Microsoft Docs'
description: Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma özelliğinin nasıl çalıştığı açıklanır.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 6507158a63de508164fc74bcafe39785046a2c79
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213359"
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory sorunsuz çoklu oturum açma: ayrıntılı Teknik İnceleme

Bu makalede, Azure Active Directory sorunsuz çoklu oturum açma (sorunsuz SSO) özelliğinin nasıl çalıştığı ile teknik ayrıntılar sunar.

## <a name="how-does-seamless-sso-work"></a>Sorunsuz çoklu oturum açma nasıl çalışır?

Bu bölüm, üç bölümü vardır:
1. Sorunsuz çoklu oturum açma özelliği kurulumu.
2. Nasıl bir tek kullanıcı işlemi bir web tarayıcısında oturum açma sorunsuz çoklu oturum açma ile çalışır.
3. Nasıl sorunsuz SSO ile oturum açma tek bir kullanıcı işlemi yerel bir istemci üzerinde çalışır.

### <a name="how-does-set-up-work"></a>Nasıl iş ayarlayamayan dilbilimsel?

Sorunsuz çoklu oturum açma etkin gösterildiği gibi Azure AD Connect kullanarak [burada](active-directory-aadconnect-sso-quick-start.md). Bu özellik etkinleştirilirken, aşağıdaki adımlar oluşur:
- Adlı bir bilgisayar hesabı `AZUREADSSOACC` (Azure AD temsil eden), şirket içi Active Directory (AD) her AD ormanında oluşturulur.
- Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır. Birden fazla AD ormanına varsa, her biri kendi Kerberos şifre çözme anahtarı gerekir.
- Ayrıca, Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.

>[!NOTE]
> (Azure AD Connect kullanarak) Azure AD ile eşitleyebilir ve olan kullanıcılar için sorunsuz SSO istediğiniz her AD ormanında bilgisayar hesabını ve Kerberos SPN'ler oluşturulur. Taşıma `AZUREADSSOACC` bilgisayar hesabı için bir kuruluş birimi (aynı şekilde yönetilir ve silinmediğinden emin olmak için diğer bilgisayar hesapları depolandığı OU).

>[!IMPORTANT]
>Yüksek oranda olmasını öneririz, [Kerberos şifre çözme anahtarını başa döndürmek](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) , `AZUREADSSOACC` en az 30 günde bir bilgisayar hesabı.

Kurulum tamamlandıktan sonra sorunsuz çoklu oturum açma herhangi diğer tümleşik Windows kimlik doğrulaması (IWA) kullanan oturum aynı şekilde çalışır.

### <a name="how-does-sign-in-on-a-web-browser-with-seamless-sso-work"></a>Nasıl iş sorunsuz SSO ile bir web tarayıcısında oturum?

Bir web tarayıcısında oturum açma akışı şu şekildedir:

1. Kullanıcı bir web uygulamasına erişmeye çalışır (örneğin, Outlook Web App - https://outlook.office365.com/owa/) bir etki alanına katılmış Kurumsal CİHAZDAN Kurumsal ağınızdaki.
2. Kullanıcı zaten oturum açmamış, kullanıcının Azure AD oturum açma sayfasına yönlendirilir.
3. Kullanıcı türleri kendi Azure AD oturum açma sayfasında kullanıcı adı.

  >[!NOTE]
  >İçin [belirli uygulamaları](./active-directory-aadconnect-sso-faq.md#what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso), adım 2 ve 3 atlanır.

4. JavaScript arka planda kullanarak, Azure AD aracılığıyla bir Kerberos anahtarı sağlamak için 401 Yetkisiz yanıt, tarayıcının sınar.
5. Tarayıcı için Active Directory'den bir bilet sırayla ister `AZUREADSSOACC` (Azure AD temsil eden) bilgisayar hesabı.
6. Active Directory bilgisayar hesabına bulur ve bir Kerberos anahtarı bilgisayar hesabının parolası ile şifrelenmiş tarayıcıya döndürür.
7. Tarayıcı, alınan Kerberos anahtarı, Active Directory'den Azure AD'ye iletir.
8. Azure AD, önceden paylaşılan anahtar kullanan kurumsal cihazda oturum açan kullanıcının kimliğini içeren Kerberos anahtarı şifresini çözer.
9. Değerlendirme sonra Azure AD belirteç uygulamaya geri döndürür veya çok faktörlü kimlik doğrulaması gibi ek kanıtları gerçekleştirmek için kullanıcıya sorar.
10. Kullanıcı oturum açma başarılı olursa, uygulamaya erişmeye çalıştığında bir kullanıcıdır.

Aşağıdaki diyagram, tüm bileşenleri ve ilgili adımları gösterir.

![Sorunsuz çoklu oturum açma uygulama akışı On - Web](./media/active-directory-aadconnect-sso/sso2.png)

Sorunsuz çoklu oturum açma başarısız olursa, oturum açma deneyimini normal davranışını - yani, kullanıcının oturum açmak için parola girmeniz gerekiyorsa geri döner. fırsatçı, yani.

### <a name="how-does-sign-in-on-a-native-client-with-seamless-sso-work"></a>Nasıl sorunsuz çoklu oturum açma çalışma ile yerel bir istemcide oturum?

Yerel bir istemci oturum açma akışı aşağıdaki gibidir:

1. Kullanıcı bir etki alanına katılmış Kurumsal CİHAZDAN Kurumsal ağınızdaki (örneğin, Outlook istemcisi) yerel bir uygulamaya erişmeye çalışır.
2. Kullanıcı zaten oturum açmamış, yerel uygulama cihazın Windows oturumu kullanıcı adını alır.
3. Uygulama, kullanıcı adını Azure AD'ye gönderir ve kiracınızın MEX WS-Trust uç noktasını alır.
4. Uygulama daha sonra WS-Trust MEX uç nokta kimlik doğrulama uç noktası kullanılabilir Tümleşik olmadığını görmek için sorgular.
5. Adım 4 başarılı olursa, bir Kerberos challenge verilir.
6. Uygulama Kerberos anahtarını almak mümkün ise, bunu Azure AD'nin tümleşik kimlik doğrulama uç noktası kadar iletir.
7. Azure AD, Kerberos anahtarı şifresini çözer ve bunu doğrular.
8. Azure AD kullanıcının oturum açtığı ve uygulamaya bir SAML belirteci verir.
9. Uygulama, Azure AD'nin OAuth2 belirteç uç noktası için SAML belirtecinde ardından gönderir.
10. Azure AD, SAML belirteci doğrular ve uygulamaya bir erişim belirteci ve yenileme belirteci için belirtilen kaynak ve bir kimlik belirteci verir.
11. Kullanıcı, uygulamanın kaynağına erişimi alır.

Aşağıdaki diyagram, tüm bileşenleri ve ilgili adımları gösterir.

![Sorunsuz tek oturum açma - yerel uygulama akışı](./media/active-directory-aadconnect-sso/sso14.png)

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-sso-quick-start.md) - getirmek ve Azure AD sorunsuz çoklu oturum açma çalışıyor.
- [**Sık sorulan sorular** ](active-directory-aadconnect-sso-faq.md) -sık sorulan soruların yanıtları.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.
