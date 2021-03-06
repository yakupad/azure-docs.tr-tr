---
title: Bir Android uygulaması Azure Active Directory B2C kullanarak bir belirteç alınırken | Microsoft Docs
description: Bu makale, kullanıcı kimliklerini yönetmek ve kullanıcıların kimliğini doğrulamak için Azure Active Directory B2C ile AppAuth kullanan bir Android uygulaması oluşturma işlemini gösterir.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 86ef621eccc7e6ba999318348f940a6a3931274e
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442415"
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Bir Android uygulaması kullanarak oturum açın

Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Bu standartlar, Azure Active Directory B2C ile tümleştirmek için istediğiniz herhangi bir kitaplıktan yararlanmasını sağlar. Diğer kitaplıkları kullanmanıza yardımcı olması için bunun gibi bir kılavuz 3 taraf kitaplıkların Microsoft identity platformuna bağlanmak için yapılandırma göstermek için kullanabilirsiniz. Uygulayan çoğu kitaplık [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) Microsoft Identity platformuna bağlanabilirsiniz.

> [!WARNING]
> Microsoft, 3 düzeltmelerin taraf kitaplıkları ve bu kitaplık bir gözden geçirme yapmış değil sağlamaz. Bu örnek, Azure AD B2C ile temel senaryolarda uyumluluk için test edilmiştir AppAuth adlı 3 bir taraf kitaplığı kullanıyor. Sorunları ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilebilir. Lütfen [bu makalede](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) daha fazla bilgi için.  
>
>

OAuth2 veya OpenID Connect kullanmaya yeni başladıysanız bu örnek yapılandırmanın büyük bölümü sizin için çok anlamlı olmayabilir. [burada belgelediğimiz protokolün genel bakışına](active-directory-b2c-reference-protocols.md) kısaca bakmanız önerilir.

## <a name="get-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## <a name="create-an-application"></a>Uygulama oluşturma

Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bir mobil uygulama oluşturmak için takip [bu yönergeleri](active-directory-b2c-app-registration.md). Şunları yaptığınızdan emin olun:

* Dahil bir **Native Client** uygulama.
* Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Bu daha sonra gerekecektir.
* Bir yerel istemcisi ayarlama **yeniden yönlendirme URI'si** (örneğin com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Buna da daha sonra ihtiyacınız olacak.

## <a name="create-your-policies"></a>İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama bir kimlik deneyimi içerir: birleşik bir oturum açma ve kaydolma. Açıklandığı gibi bu ilkeyi oluşturmak gereken [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). İlkeyi oluştururken şunları yaptığınızdan emin olun:

* Seçin **görünen ad** bir kaydolma ilkenizde özniteliği olarak.
* Her ilkede uygulamanın talep ettiği **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır.  Bu ilke adına daha sonra ihtiyacınız olacaktır.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

İlkelerinizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin

AppAuth kullanan Azure AD B2C ile çalışma örnek sağladık [github'da](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Kodu indirin ve çalıştırın. Hızlı bir şekilde kendi Azure AD B2C'yi yapılandırma yönergelerini takip ederek kullanarak kendi uygulamanızı kullanmaya başlayabileceğiniz [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

Örnek tarafından sağlanan örnek bir değişikliktir [AppAuth](https://openid.github.io/AppAuth-Android/). AppAuth ve özellikleri hakkında daha fazla bilgi için kendi sayfasını ziyaret edin.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Azure AD B2C ile AppAuth kullanmak için uygulamanızı değiştirme

> [!NOTE]
> AppAuth destekleyen Android API 16 (Jellybean) ve üzeri. API 23 kullanmanızı öneririz ve üstü.
>

### <a name="configuration"></a>Yapılandırma

URI bulma belirterek veya yetkilendirme uç noktası ve belirteç uç noktası URI belirterek Azure AD B2C ile iletişim yapılandırabilirsiniz. Her iki durumda da, aşağıdaki bilgiler gerekir:

* Kiracı kimliği (örn. contoso.onmicrosoft.com)
* İlke adı (örneğin B2C\_1\_SignUpIn)

Yetkilendirme ve belirteç uç noktası URI otomatik olarak bulmayı seçerseniz, URI keşiften bilgileri getirmek gerekir. URI Kiracının değiştirerek oluşturulabilir bulma\_kimliği ve ilke\_aşağıdaki URL adı:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Ardından, yetkilendirme ve belirteç uç noktası URI almak ve aşağıdaki komutu çalıştırarak AuthorizationServiceConfiguration nesne oluşturma:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

Yetkilendirme ve belirteç uç noktası URI elde etmek için bulma kullanmak yerine, bunları açıkça Kiracı değiştirerek belirtebilirsiniz\_kimliği ve ilke\_adı URL'nin aşağıdaki:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

AuthorizationServiceConfiguration nesneyi oluşturmak için aşağıdaki kodu çalıştırın:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a>Yetkilendiriliyor

Yapılandırma veya bir yetkilendirme hizmet yapılandırması alma sonra bir yetkilendirme isteği oluşturulabilir. İsteği oluşturmak için aşağıdaki bilgileri ihtiyacınız olacak:

* İstemci kimliği (örneğin 00000000-0000-0000-0000-000000000000)
* Özel bir düzen ile (ör. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect) yeniden yönlendirme URI'si

Olduğunda, her iki öğe kaydedilmiş olması [uygulamanızı kaydetme](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Lütfen [AppAuth Kılavuzu](https://openid.github.io/AppAuth-Android/) nasıl işlemi tamamlayın. İle çalışan bir uygulamayı hızlıca başlamak ihtiyacınız varsa, kullanıma [örneğimizi](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Bağlantısındaki [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) kendi Azure AD B2C yapılandırmasını girmek için.

Her zaman geri bildirim ve öneriler için açık duyuyoruz! Bu makalede herhangi bir güçlük sahip veya bu içeriğin geliştirilmesi için öneri, sayfanın sonundaki geri BİLDİRİMİNİZE değer veriyoruz. Özellik istekleri için ekleyebilmesi [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

