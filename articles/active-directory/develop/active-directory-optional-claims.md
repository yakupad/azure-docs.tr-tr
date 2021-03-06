---
title: Azure AD uygulamanız için isteğe bağlı bir talep sağla öğrenin | Microsoft Docs
description: Azure Active Directory tarafından verilen SAML 2.0 ve JSON Web belirteçleri (JWT) belirteçleri talepleri özel veya ek eklemek için bir kılavuz.
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 6e0b00117c35cd5222c69e72819afb37f9ec14dd
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265073"
---
# <a name="optional-claims-in-azure-ad-preview"></a>İsteğe bağlı bir talep Azure AD'de (Önizleme)

Bu özellik, uygulama geliştiriciler tarafından hangi kullanıcıların uygulamaya gönderilen belirteçlerde istediği talepleri belirtmek için kullanılır. İsteğe bağlı talepleri kullanabilirsiniz:
-   Ek talep belirteçleri uygulamanıza eklemek için seçin.
-   Azure AD belirteçleri döndüren belirli talep davranışını değiştirin.
-   Ekleme ve özel talepler uygulamanıza erişebilirsiniz. 

> [!Note]
> Bu özellik şu anda genel Önizleme aşamasındadır. Değişiklikleri geri almaya veya kaldırmaya hazırlıklı olun. Bu özellik, herhangi bir Azure AD aboneliğiniz genel Önizleme sırasında kullanılabilir. Ancak, bazı yönlerini özellik özelliği genel kullanıma sunulduğunda, bir Azure AD premium aboneliği gerektirebilir.

Standart talepler ve belirteçler nasıl kullanıldığı listesi için bkz. [Azure AD tarafından verilen belirteçlere Temelleri](active-directory-token-and-claims.md). 

Hedeflerinden [Azure AD v2.0 uç noktası](active-directory-appmodel-v2-overview.md) istemciler tarafından en iyi performansı elde etmek için daha küçük simge boyutları.  Sonuç olarak, eski erişim ve kimlik belirteçlerini dahil birkaç talep artık v2.0 belirteçleri varsa ve için özellikle uygulama başına temelinde sorulması gerekir.  

**Tablo 1: uygulanabilirliği**

| Hesap türü | V1.0 uç noktası                      | V2.0 uç noktası  |
|--------------|------------------------------------|----------------|
| Kişisel Microsoft hesabı  | Yok - RPS biletleri yerine kullanılır | Destek yakında |
| Azure AD hesabı          | Desteklenen                          | Desteklenen      |

## <a name="standard-optional-claims-set"></a>Standart isteğe bağlı bir talep kümesi
İsteğe bağlı varsayılan olarak kullanılabilir talepler için uygulamaları kullanmaya kümesini aşağıda listelenmiştir.  Uygulamanız için isteğe bağlı özel talepler eklemek için bkz [dizin genişletmeleri](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions)aşağıdaki. 

> [!Note]
>Bu talep çoğunu içinde Jwt'ler v1.0 ve v2.0 belirteçleri, ancak değil SAML belirteçlerini dışında belirteç Türü sütununda belirtilmedikçe dahil edilebilir.  Ayrıca, isteğe bağlı talepleri yalnızca AAD kullanıcıları için şu anda desteklendiğinden, MSA desteği ekleniyor.  MSA v2.0 uç noktada destek isteğe bağlı bir talep varsa, kullanıcı türü sütunu bir AAD veya MSA kullanıcısı için bir talep olup olmadığını gösterir.  

**Tablo 2: Standart isteğe bağlı bir talep kümesi**

| Ad                        | Açıklama   | Belirteç türü | Kullanıcı Türü | Notlar  |
|-----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Zaman zaman son kullanıcı kimlik doğrulaması.  Bkz: Openıd Connect belirtimi.| JWT        |           |  |
| `tenant_region_scope`      | Kaynak Kiracı bölgesi | JWT        |           | |
| `signin_state`             | Oturum durumu talebi   | JWT        |           | dönüş değerleri bayrakları 6:<br> "dvc_mngd": cihaz yönetilir<br> "dvc_cmp": cihaz uyumlu<br> "dvc_dmjd": cihaz etki alanına katılmış olan<br> "dvc_mngd_app": cihaz MDM yönetilir<br> "inknownntwk": bilinen ağ içinde cihazdır.<br> "kmsı": Canlı bana imzalı olarak kullanıldı. <br> |
| `controls`                 | Birden çok değerli talep koşullu erişim ilkeleri tarafından zorlanan oturum denetimleri içeren.  | JWT        |           | 3 değeri:<br> "app_res": uygulamanın daha ayrıntılı sınırlamalar zorlamak için gerekir. <br> "ca_enf": koşullu erişim zorlama ertelendi ve yine de gereklidir. <br> "no_cookie": Bu tarayıcıda tanımlama bilgisi için değişimi yetersiz bir belirteçtir. <br>  |
| `home_oid`                 | Konuk kullanıcılar için kullanıcının giriş kiracısında kullanıcının nesne kimliği.| JWT        |           | |
| `sid`                      | Oturum başına kullanıcı signout için kullanılan oturum kimliği. | JWT        |           |         |
| `platf`                    | Cihaz platformu    | JWT        |           | Cihaz türü doğrulayabilirsiniz yönetilen cihazlar için kısıtlı.|
| `verified_primary_email`   | Kullanıcının PrimaryAuthoritativeEmail kaynağı      | JWT        |           |         |
| `verified_secondary_email` | Kullanıcının SecondaryAuthoritativeEmail kaynağı   | JWT        |           |        |
| `enfpolids`                | Uygulanan ilkeyi kimlikleri. Geçerli kullanıcı için değerlendirilen kimlikleri ilke listesi.  | JWT |  |  |
| `vnet`                     | VNET belirleyici bilgi.    | JWT        |           |      |
| `fwd`                      | IP adresi.| JWT    |   | İstekte bulunan istemciye (bir sanal ağ içindeki olduğunda) özgün IPv4 adresini ekler |
| `ctry`                     | Kullanıcının ülke | JWT |           | Azure AD döndürür `ctry` varsa ve FR, JP, SZ ve benzeri gibi bir standart iki harfli ülke kodu talep değeri isteğe bağlı bir talep. |
| `tenant_ctry`              | Kaynak kiracının ülke | JWT | | |
| `xms_pdl`          | Tercih edilen veri konumu   | JWT | | Çoklu coğrafi kiracılar için bu yöntem, kullanıcının hangi coğrafi bölgede gösteren 3 harfli koddur.  Daha fazla ayrıntı için [tercih edilen veri konumu hakkında Azure AD Connect belgelerini](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation). <br> Örneğin: `APC` Asya Pasifik için. |
| `xms_pl`                   | Kullanıcı tercih edilen dil  | JWT ||Kullanıcının, uygulamanın tercih edilen dil, ayarlayın.  Konuk erişimi senaryoları kendi giriş kiracısında kaynağı.  Tümünü CC biçimlendirilmiş ("en-us"). |
| `xms_tpl`                  | Kiracı tercih edilen dil| JWT | | Kaynak Kiracı kişinin tercih ettiği dili, ayarlayın.  Biçimlendirilmiş LL ("tr"). |
| `ztdid`                    | Sıfır dokunma dağıtım kimliği | JWT | | Cihaz kimliği için kullanılan [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) |
| `acct`             | Kiracıdaki kullanıcıların hesap durumu.   | JWT, SAML | | Kullanıcı, kiracısının üyesi ise, değer `0`.  Bir konuk olmaları durumunda değerdir `1`.  |
| `upn`                      | UserPrincipalName talep.  | JWT, SAML  |           | Bu talep otomatik olarak dahil olsa da, Konuk kullanıcı durumda davranışını değiştirmek için ek özellikler eklemek için isteğe bağlı bir talep olarak belirtebilirsiniz.  <br> Ek özellikleri: <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash` |

### <a name="v20-optional-claims"></a>İsteğe bağlı taleplerin v2.0
Bu talepler her zaman v1.0 belirteçlerinde dahil, ancak v2.0 belirteçlerinde istenmedikçe dahil değildir.  Bu talepler yalnızca (kimlik ve erişim belirteçler) Jwt'ler için geçerlidir.  

**Tablo 3: Yalnızca V2.0 isteğe bağlı talepleri**

| JWT talep     | Ad                            | Açıklama                                                                                                                    | Notlar |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | IP Adresi                      | Oturum açtığınız istemci IP adresi.                                                                                      |       |
| `onprem_sid`  | Şirket içi güvenlik tanımlayıcısı |                                                                                                                                |       |
| `pwd_exp`     | Parola süre sonu zamanı        | Parola süresinin dolma datetime.                                                                                    |       |
| `pwd_url`     | Parola URL'sini değiştirme             | Kullanıcının parolasını değiştirmek için ziyaret edebileceği bir URL.                                                                        |       |
| `in_corp`     | İç şirket ağı        | Sinyaller istemci ve şirket ağından oturum açılıyor. Değilse, talep dahil değildir                     |       |
| `nickname`    | Takma ad                        | İlk veya son adından ayrı kullanıcı için ek bir ad.                                                             |       |                                                                                                                |       |
| `family_name` | Soyadı                       | Son adını, soyadını veya kullanıcının aile adı Azure AD kullanıcı nesnesinde tanımlanan sağlar. <br>"family_name": "Mert" |       |
| `given_name`  | Ad                      | İlk sağlar veya "Azure AD kullanıcı nesnesindeki belirlenen kullanıcı adı verilen".<br>"given_name": "Ferdi"                   |       |

### <a name="additional-properties-of-optional-claims"></a>İsteğe bağlı taleplerin ek özellikler

Bazı isteğe bağlı bir talep, talep döndürülen şeklini değiştirmek için yapılandırılabilir.  Bu ek özellikler genellikle farklı veri beklentileri ile şirket içi uygulamaların taşınmasına yardımcı olmak için kullanılır (örneğin, `include_externally_authenticated_upn_without_hash` hashmarks işleyemiyor istemcilerle yardımcı olur (`#`) UPN içinde)

**Tablo 4: standart isteğe bağlı talep yapılandırma değerleri**

| Özellik adı                                     | Ek özellik adı                                                                                                             | Açıklama |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |  SAML hem JWT yanıtlar için kullanılabilir.            |
| | `include_externally_authenticated_upn`              | Kaynak kiracıda depolanmış olarak UPN Konuk içerir.  Örneğin, `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | Yukarıdaki aynı dışındaki hashmarks (`#`) alt çizgi ile değiştirilir (`_`), örneğin `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>Bir ek özellik olmadan upn isteğe bağlı talebi, belirteçte verilen yeni bir talep görmek için herhangi bir davranış – değiştirmez belirtilmesi, en az bir ek özellikler eklenmelidir. 


#### <a name="additional-properties-example"></a>Ek özellikleri örneği:

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Bu OptionalClaims nesne başka bir upn ile ek giriş kiracısında ve kaynak Kiracı bilgileri içerecek şekilde istemciye döndürülen kimlik belirteci neden olur.  Bu, yalnızca değiştirecek `upn` (farklı bir IDP kimlik doğrulaması kullanan için) kiracıda bir Konuk kullanıcı, belirteçte talep. 

## <a name="configuring-optional-claims"></a>İsteğe bağlı bir talep yapılandırma

(Aşağıdaki örneğe bakın) uygulama bildirimini değiştirerek, uygulamanız için isteğe bağlı bir talep yapılandırabilirsiniz. Daha fazla bilgi için [Azure AD uygulama bildirim makaleyi anlama](active-directory-application-manifest.md).

**Örnek şeması:**

```json
"optionalClaims":  
   {
       "idToken": [
             { 
                   "name": "auth_time", 
                   "essential": false
              }
        ],
 "accessToken": [ 
             {
                    "name": "ipaddr", 
                    "essential": false
              }
        ],
"saml2Token": [ 
              { 
                    "name": "upn", 
                    "essential": true
               },
               { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>OptionalClaims türü

Bir uygulama tarafından istenen isteğe bağlı talepleri bildirir. Bir uygulama güvenlik belirteci Hizmeti'nden alabileceği her üç tür belirteçleri (kimlik, erişim belirteci, SAML 2 token) döndürülecek isteğe bağlı taleplerin yapılandırabilirsiniz. Uygulamayı farklı bir belirteç türlerinin döndürülecek isteğe bağlı bir talep kümesini yapılandırabilirsiniz. Uygulama varlığın OptionalClaims özelliği OptionalClaims bir nesnedir.

**Tablo 5: OptionalClaims türü özellikleri**

| Ad        | Tür                       | Açıklama                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Koleksiyon (OptionalClaim) | İsteğe bağlı talep kimliği JWT belirteci döndürdü.     |
| `accessToken` | Koleksiyon (OptionalClaim) | JWT erişim belirtecinde verilen talepleri isteğe bağlı. |
| `saml2Token`  | Koleksiyon (OptionalClaim) | SAML belirtecinde verilen talepleri isteğe bağlı.       |


### <a name="optionalclaim-type"></a>OptionalClaim türü

Bir uygulama veya hizmet sorumlusu ile ilişkili isteğe bağlı bir talep içerir. Idtoken accessToken ve saml2Token özelliklerini [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) türü OptionalClaim koleksiyonudur.
Belirli bir talep tarafından destekleniyorsa, ayrıca AdditionalProperties alanını kullanarak OptionalClaim davranışını değiştirebilirsiniz.

**Tablo 6: OptionalClaim türü özellikleri**

| Ad                 | Tür                    | Açıklama                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | İsteğe bağlı bir talep adı.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Kaynağı (dizin nesnesi) talep. Önceden tanımlanmış talep ve kullanıcı tanımlı talep uzantı özelliklerinden vardır. Kaynak değeri null ise, talep önceden tanımlanmış isteğe bağlı bir talep var. Kullanıcı kaynak değeri ise name özelliği kullanıcı nesnesi uzantı özelliği değer. |
| `essential`            | Edm.Boolean             | Değer true ise, istemci tarafından belirtilen talep son kullanıcı tarafından istenen belirli görev için bir yumuşak yetkilendirme deneyimi sağlamak gereklidir. Varsayılan değer false'tur.                                                                                                                 |
| `additionalProperties` | Koleksiyon (Edm.String) | Talep ek özellikleri. Bu koleksiyonda bir özellik varsa, belirtilen ad özelliği isteğe bağlı bir talep davranışını değiştirir.                                                                                                                                                   |

## <a name="configuring-custom-claims-via-directory-extensions"></a>Özel talepler dizin uzantıları aracılığıyla yapılandırma

Standart isteğe bağlı talep kümesine ek olarak, belirteçleri directory şema uzantıları içerecek şekilde de yapılandırılabilir (bkz [Directory şema uzantılarını makalesi](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) daha fazla bilgi için).  Bu özellik, uygulamanızı – örneğin kullanabileceğiniz ek kullanıcı bilgileri, bir ek tanımlayıcı veya kullanıcı önemli yapılandırma seçeneğini eklemek için yararlıdır. 

> [!Note]
> Uygulamanızı bildirim istekleri, özel uzantı ve bir MSA kullanıcısı oturum uygulamanıza directory şema uzantıları yalnızca AAD özelliği olduğundan, bu uzantıları döndürülmez. 

### <a name="values-for-configuring-additional-optional-claims"></a>İsteğe bağlı ek talep yapılandırma değerleri 

Uzantı öznitelikleri için uzantı tam adını kullanın (biçimde: `extension_<appid>_<attributename>`) uygulama bildiriminde. `<appid>` Talep isteyen uygulama kimliği ile eşleşmelidir. 

JWT içinde bu talepler aşağıdaki ad biçimiyle yayılan: `extn.<attributename>`.

SAML belirteçlerini içinde bu talepler aşağıdaki URI biçimi ile yayılan: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="optional-claims-example"></a>İsteğe bağlı bir talep örneği

Bu bölümde, isteğe bağlı bir talep özelliği, uygulamanız için nasıl kullanabileceğinizi görmek için bir senaryo size yol.
Özellikleri etkinleştirmek ve isteğe bağlı talepleri yapılandırmak için uygulamanın kimlik yapılandırmasını güncelleştirmek için kullanılabilir birden çok seçenek vardır:
-   Uygulama bildirimini değiştirebilirsiniz. Aşağıdaki örnekte, yapılandırmayı gerçekleştirmek için bu yöntemi kullanır. Okuma [Azure AD uygulama bildirimi belge anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) ilk bildirimi için giriş.
-   Kullanan bir uygulamayı yazmak mümkündür [Graph API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) uygulamanızı güncelleştirmek için. [Varlık ve karmaşık tür başvurusu](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) Graph API Başvurusu Kılavuzu, isteğe bağlı talepleri yapılandırmaya yardımcı olabilir.

**Örnek:** aşağıdaki örnekte, erişim kimliği ve SAML talepleri eklemek için bir uygulamanın bildirim değiştirecek uygulama için amaçlanan belirteçleri.
1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Kimlik doğrulaması yaptınız sonra sayfanın sağ üst köşesinde seçerek Azure AD kiracınızı seçin.
3.  Seçin **Azure AD uzantısı** tıklayın ve sol gezinti bölmesi **uygulama kayıtları**.
4.  İsteğe bağlı talepler için listeden yapılandırın ve üzerine tıklayarak istediğiniz uygulamayı bulun.
5.  Uygulama sayfasında **bildirim** satır içi bildirim düzenleyicisini açın. 
6.  Bu düzenleyici kullanılarak bildirimde doğrudan düzenleyebilirsiniz. Bildirim için şema izleyen [uygulama varlığı](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)ve otomatik biçimleri bildirim kez kaydedildi. Yeni öğeleri eklenecek `OptionalClaims` özelliği.

      ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
      "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
      "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }
      ```
      Bu durumda, isteğe bağlı farklı talepler her uygulama alabilir belirteci türüne eklendi. Kimlik belirteçlerini artık tam formunda Federasyon kullanıcıları için UPN içerecektir (`<upn>_<homedomain>#EXT#@<resourcedomain>`). Erişim belirteçleri artık auth_time talep alır. SAML belirteçlerini skypeId directory şema uzantısı şimdi içerecektir (Bu örnekte, bu uygulama için uygulama kimliği ab603c56068041afb2f6832e2a17e237 olduğu).  SAML belirteçlerini Skype kimliği olarak açığa çıkarır `extension_skypeId`.

7.  Aktualizuje SE manifest tamamladığınızda, tıklayın **Kaydet** bildirimi kaydetmek için


## <a name="related-content"></a>İlgili içerik
* Daha fazla bilgi edinin [standart talep](active-directory-token-and-claims.md) Azure AD tarafından sağlanan. 
