---
title: Azure Active Directory’de izinleri yönetme | Microsoft Docs
description: Azure Active Directory'deki izinleri ve bunların kullanımını öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 6c0dc122-2cd8-4d70-be5a-3943459d308e
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/25/2018
ms.author: celested
ms.reviewer: jesakowi, justhu
ms.custom: aaddev
ms.openlocfilehash: 3b0fbbe466f51a6216716d274f238497a8a79294
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441643"
---
# <a name="permissions-in-azure-active-directory"></a>Azure Active Directory'deki izinler

Azure Active Directory (Azure AD), hem OAuth hem de OpenID Connect (OIDC) akışlarında izinleri yaygın olarak kullanır. Uygulamanız Azure AD'den bir erişim belirteci aldığında, bu belirteç uygulamanızın belirli bir kaynakla ilgili olarak sahip olduğu izinleri açıklayan talepler içerir. Kapsamlar olarak da bilinen izinler, kaynağın yalnızca belirteçte uygulamanın çağırdığı API için uygun izni içerip içermediğini denetlemesi gerektiğinden kaynağın yetkilendirmesini kolaylaştırır. 

## <a name="types-of-permissions"></a>İzin türleri

Azure AD iki tür izin tanımlar: 
* **Temsilci izinleri** - Oturum açmış kullanıcının olduğu uygulamalar tarafından kullanılır. Bu uygulamalar için, kullanıcı veya yönetici uygulamanın istediği onayları verir ve uygulamaya API çağrıları yaparken oturum açmış kullanıcı adına işlem yapması için temsilci izni verilir. API'yi bağlı olarak, kullanıcı doğrudan API'ye onay veremeyebilir ve bunun yerine [bir yöneticinin "yönetici onayı" sağlaması gerekebilir.](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent)
* **Uygulama izinleri** - Oturum açmış kullanıcı olmadan çalıştırılan uygulamalar tarafından kullanılır; örneğin, arka plan hizmetleri veya daemon programları olarak çalıştırılan uygulamalar böyledir. Uygulama izinleri yalnızca [yönetici tarafından verilebilir](/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant) çünkü normalde bunlar güçlü izinlerdir ve kullanıcı sınırlarının ötesindeki verilere veya aslında yöneticilerin kullanımıyla kısıtlanmış olabilecek verilere erişim izin verebilir. 

Etkili izinler, uygulamanızın API istekleri yaparken sahip olacağı izinlerdir. 

* Temsilci izinleri için, uygulamanızın etkili izinleri uygulamaya verilmiş olan (onay yoluyla) temsilci izinleriyle o anda oturum açmış olan kullanıcının ayrıcalıklarının en düşük ayrıcalıklı kesişimi olacaktır. Uygulamanızın ayrıcalıkları hiçbir zaman oturum açmış kullanıcının ayrıcalıklarından fazla olamaz. Kuruluşların içinde, oturum açmış kullanıcının ayrıcalıkları ilkeyle ya da bir veya birden çok yönetici rolü üyeliğiyle belirlenebilir. Yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md).
    Örneğin, Microsoft Graph'te uygulamanıza `User.ReadWrite.All` temsilci izni verildiğini varsayalım. Adından da anlaşıldığı gibi bu izin uygulamanıza kuruluştaki her kullanıcının profilini okuma ve güncelleştirme izni verir. Oturum açmış kullanıcının bir genel yönetici olması durumunda, uygulamanız kuruluştaki her kullanıcının profilini güncelleştirebilir. Öte yandan oturum açmış kullanıcı bir yönetici rolünde değilse, uygulamanız yalnızca oturum açmış kullanıcının profilini güncelleştirebilir. Kuruluştaki diğer kullanıcıların profillerini güncelleştiremez çünkü adına çalışma iznine sahip olduğu kullanıcı söz konusu ayrıcalıklara sahip değildir.
* Uygulama izinleri için, uygulamanızın etkili izinleri izin tarafından belirtilen tüm ayrıcalık düzeylerini kapsar. Örneğin, `User.ReadWrite.All` uygulama iznine sahip bir uygulama kuruluştaki her kullanıcının profilini güncelleştirebilir. 

## <a name="permission-attributes"></a>İzin öznitelikleri
Azure AD'deki izinlerin bir dizi özelliği vardır ve bu özellikler kullanıcıların, yöneticilerin veya uygulama geliştiricilerin iznin ne erişimi verdiği konusunda bilinçli kararlar almasına yardımcı olur. 

> [!NOTE]
> Azure portalını veya PowerShell'i kullanarak Azure AD Uygulaması veya Hizmet Sorumlusunun kullanıma sunduğu izinleri görüntüleyebilirsiniz. Microsoft Graph tarafından kullanıma sunulan izinleri görüntülemek için bu betiği deneyin.
> ```powershell
> Connect-AzureAD
> 
> # Get OAuth2 Permissions/delegated permissions
> (Get-AzureADServicePrincipal -filter "DisplayName eq 'Microsoft Graph'").OAuth2Permissions
> 
> # Get App roles/application permissions
> (Get-AzureADServicePrincipal -filter "DisplayName eq 'Microsoft Graph'").AppRoles
> ```

| Özellik adı | Açıklama | Örnek | 
| --- | --- | --- |
| `ID` | Bu izni benzersiz olarak tanımlayan GUID değeridir. | 570282fd-fa5c-430d-a7fd-fc8dc98a9dca | 
| `IsEnabled` | Bu iznin kullanılabilir olup olmadığını gösterir. | true | 
| `Type` | Bu iznin kullanıcı onayı veya yönetici onayı gerektirip gerektirmediğini gösterir. | Kullanıcı | 
| `AdminConsentDescription` | Yönetici onayı deneyimleri sırasında yöneticilere gösterilen açıklamadır | Uygulamanın kullanıcı posta kutularındaki e-postayı okumasına izin verir. | 
| `AdminConsentDisplayName` | Yönetici onayı deneyimi sırasında yöneticilere gösterilen kolay addır. | Kullanıcı postasını okuma | 
| `UserConsentDescription` | Kullanıcı onayı deneyimi sırasında kullanıcılara gösterilen açıklamadır. |  Uygulamanın, posta kutunuzdaki e-postalarınızı okumasına izin verir. | 
| `UserConsentDisplayName` | Kullanıcı onayı deneyimi sırasında kullanıcılara gösterilen kolay addır. | Postalarınızı okuma | 
| `Value` | OAuth 2.0 yetkilendirme akışları sırasında izni tanımlamak için kullanılan dizedir. `Value`, tam izin adı oluşturmak için Uygulama Kimliği URI dizesiyle birleştirilebilir. | `Mail.Read` | 

## <a name="types-of-consent"></a>Onay türleri
Azure AD'deki uygulamalar gerekli kaynaklara veya API'lere erişim kazanmak için onaya bağımlıdır. Uygulamanızın başarılı olabilmesi için tanıyor olması gereken çeşitli onay türleri vardır. İzinleri tanımlıyorsanız, kullanıcıların uygulamanıza veya API'nize nasıl erişim kazanacağını da anlamalısınız.

* **Statik kullanıcı onayı** - Uygulamanızın etkileşimli çalışmak istediği kaynağı belirttiğinizde [OAuth 2.0 yetkilendirme akışı](/azure/active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) sırasında otomatik olarak gerçekleşir. Statik kullanıcı onayı senaryosunda, uygulamanızın gereken tüm izinleri Azure portalındaki uygulama yapılandırmasında zaten belirtmiş olması gerekir. Kullanıcı (veya uygun olduğunda yönetici) bu uygulamaya onay vermezse, Azure AD kullanıcının şu anda onay vermesini ister. 

    Statik bir API kümesine erişim isteyen bir Azure AD uygulamasını kaydetme hakkında daha fazla bilgi edinin.
* **Dinamik kullanıcı onayı** - v2 Azure AD uygulama modelinin bir özelliğidir. Bu senaryoda, uygulamanız [v2 uygulamaları için OAuth 2.0 yetkilendirme akışı](/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent) içinde ihtiyacı olan bir dizi izin ister. Kullanıcı henüz onaylamadıysa, bu aşamada onaylaması istenir. [Dinamik onay hakkında daha fazla bilgi edinin](/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent).

    > [!NOTE]
    > Dinamik onay kullanışlı olabilir ama yönetici onayı gerektiren izinlerde çok zorluk çıkarabilir çünkü yönetici onayı deneyimi, onay zamanında söz konusu izinleri bilmiyor olacaktır. Ayrıcalıklı yönetici izinlerine ihtiyacınız varsa, uygulamanızın bunları Azure Portal'da kaydetmesi gerekir.
  
* **Yönetici onayı** - Uygulamanızın bazı yüksek ayrıcalıklı izinlere erişmeye ihtiyacı olması durumunda gereklidir. Yönetici onayı, uygulamalara veya kullanıcılara kuruluşunuzun yüksek ayrıcalıklı verilerine erişme yetkisi verilmeden önce yöneticilerin bazı ek denetimler yapabilmesini sağlar. [Yönetici onayı verme hakkında daha fazla bilgi edinin](/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="client-best-practices"></a>İstemci en iyi uygulamaları

- Yalnızca uygulamanızın ihtiyacı olan izinleri isteyin. Aşırı fazla izinleri olan uygulamaların güvenliği aşıldığında, kullanıcı verilerini açığa çıkarma riski doğar.
- Uygulamanızın desteklediği senaryoya göre temsilci izinleri ve uygulama izinleri arasında seçim yapın. 
    - Kullanıcı adına çağrı yapılıyorsa her zaman temsilci izinlerini kullanın.
    - Uygulama izinlerini yalnızca uygulama etkileşimli değilse ve herhangi bir kullanıcının adına çağrı yapmıyorsa kullanın. Uygulama izinleri yüksek ayrıcalıklara sahiptir ve yalnızca gerçekten gerekli olduğunda kullanılmalıdır.
- v2.0 uç noktasını temel alan bir uygulama kullandığınızda, yönetici onayı gibi senaryoların doğru şekilde çalışması için statik izinleri (uygulama kaydınızda belirtilenler) çalışma zamanında istenen dinamik izinlerin (kodda belirtilen ve yetkilendirme isteğinizde sorgu parametresi olarak gönderilen) üst kümesi olarak belirleyin.

### <a name="resourceapi-best-practices"></a>Kaynak/API en iyi yöntemleri

- API'leri kullanıma sunan kaynaklar, korudukları verilere veya eylemlere özgü izinler tanımlamalıdır. Bu en iyi yöntemin uygulanması, istemcilerin ihtiyaçları olmayan verilere erişmemesini ve kullanıcıların hangi verilere onay verdikleri konusunda bilgi sahibi olmasını sağlamaya yardımcı olur.
- Kaynaklar `Read` ve `ReadWrite` izinlerini ayrı ayrı ve açıkça tanımlamalıdır.
- Kaynaklar, kullanıcı sınırlarının ötesinde veri erişimine olanak tanıyan tüm izinleri `Admin` izinleri olarak işaretlemelidir.
- Kaynakların `Subject.Permission[.Modifier]` adlandırma modelini kullanması gerekir:
    - `Subject`, kullanılabilir durumdaki veri türünü belirtir,
    - `Permission`, kullanıcının bu verilerle gerçekleştirebileceği eylemi belirtir ve 
    - `Modifier`, isteğe bağlı olarak başka bir iznin özelleştirmelerini açıklamak için kullanılır. 
    
    Örnek: 
    * Mail.Read - Kullanıcıların postayı okumasına izin verir.
    * Mail.ReadWrite - Kullanıcıların postayı yazmasına veya okumasına izin verir.
    * Mail.ReadWrite.All - Yöneticinin veya kullanıcının kuruluştaki tüm postaya erişmesine izin verir.





