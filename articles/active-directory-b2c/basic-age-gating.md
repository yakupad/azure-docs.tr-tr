---
title: Azure Active Directory B2C'de geçerlilik süresi geçişi kullanma | Microsoft Docs
description: Uygulamanızı kullanarak reşit olmayanların tanımlama hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9d24e37642a41e4d60b33f42a60d7e56cb4b35b5
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446733"
---
#<a name="using-age-gating-in-azure-ad-b2c"></a>Azure AD B2C'de geçerlilik süresi geçişi kullanma

>[!IMPORTANT]
>Bu özellik, özel Önizleme aşamasındadır.  Lütfen bkz. bizim [service blog](https://blogs.msdn.microsoft.com/azureadb2c/) olarak bu Ayrıntılar için kullanılabilir veya kişi olur AADB2CFeedback@microsoft.com.  Üretim dizinleri kullanmayın, bu yeni özellikleri kullanarak veri kaybına neden olabilir ve olabilir ki genel kullanıma oluncaya kadar davranış beklenmeyen değişiklikleri.  
>

##<a name="age-gating"></a>Yaş geçidi
Yaş geçidi reşit olmayanların uygulamanızdaki tanımlamak için Azure AD B2C kullanmanıza olanak tanır.  Kullanıcının uygulamada oturum açar veya kullanıcının yaş grubu ve ebeveyn izni durumlarını tanımlamak ek talep uygulamaya geri dönmek izin vermek seçebilirsiniz.  

>[!NOTE]
>Ebeveyn izni adlı bir kullanıcı özniteliği izlenen `consentProvidedForMinor`.  Bu özellik Graph API'si aracılığıyla güncelleştirebilir ve güncelleştirirken bu alanı kullanın `legalAgeGroupClassification`.
>

##<a name="setting-up-your-directory-for-age-gating"></a>Yaş geçidi için dizin ayarlama
Kullanıcı akışı yaş geçidi kullanmak için ek özellikler sağlamak için dizin yapılandırmanız gerekir. Bu işlem aracılığıyla yapılabilir `Properties` (özel Önizleme parçası kullanıyorsanız kullanıma sunulacak) menüden.  
1. Azure AD B2C uzantısı'nda tıklayarak **özellikleri** sol taraftaki menüden kiracınız için.
2. Altında **yaş geçidi** bölümünde, tıklayarak **yapılandırma** düğmesi.
3. İşlemin tamamlanmasını bekleyin ve dizininize yaş geçidi için ayarlanır.

##<a name="enabling-age-gating-in-your-user-flow"></a>Kullanıcı akışınızı yaş geçidi etkinleştirme
Yaş geçidi kullanmaya dizininize ayarladıktan sonra bu özellik Önizleme sürümü kullanıcı akışlarında sonra kullanabilirsiniz.  Bu özellik, kullanıcı akışları varolan türleri uyumsuz yaptığınız değişiklikler gerektirir.  Aşağıdaki adımlarla yaş geçidi sağlar:
1. Önizleme kullanıcı akışı oluşturun.
2. Oluşturulduktan sonra Git **özellikleri** menüsünde.
3. İçinde **yaş geçidi** bölümünde, geçiş özelliği etkinleştirmek için basın.
4. Ardından, nasıl reşit olmayanların tanımlayan kullanıcıları yönetmek istediğinizi seçebilirsiniz.

##<a name="what-does-enabling-age-gating-do"></a>Yaş geçidi etkinleştirme ne yapar?
Yaş geçidi kullanıcı akışınızı etkinleştirildikten sonra kullanıcı değişiklikleri karşılaşırsınız.  Oturum açma, kullanıcılar artık kendi doğum tarihi ve ülke ikametgahınızda kullanıcı akışı için yapılandırılmış kullanıcı öznitelikleri ile birlikte istenecektir.  Oturum açma, daha önce bir doğum tarihi ve ülke ikametgahınızda girmediniz kullanıcılar oturum açtıklarında bu bilgi sonraki istenecektir.  Bu iki değerden Azure AD B2C küçük bir kullanıcı olup olmadığını tanımlar ve güncelleştirme `ageGroup` alanın değeri olabilir `null`, `Undefined`, `Minor`, `Adult`, ve `NotAdult`.  `ageGroup` Ve `consentProvidedForMinor` alanları hesaplamak için kullanılan ardından `legalAgeGroupClassification`. 

##<a name="age-gating-options"></a>Yaş geçidi seçenekleri
Azure AD B2C blok reşit olmayanların ebeveyn izni olmadan varsa veya izin vermek ve bunları yapmanız gerekenler hakkında kararlar uygulama seçebilirsiniz.  

###<a name="allowing-minors-without-parental-consent"></a>Reşit olmayanların ebeveyn izni olmadan izin verme
Yukarı ya da oturum izin, oturum ve kullanıcı akışları veya her ikisi için izniniz olmadan reşit olmayanların uygulamanıza izin vermek seçebilirsiniz.  Ebeveyn izni olmadan reşit olmayanlar için bunlar normal ve Azure AD B2C ile bir kimlik belirteci sorunları olarak kaydolun veya oturum izin verilmez `legalAgeGroupClassification` talep.  Bu talep tercih edebilir, bu kullanıcıların deneyimini kullanarak gibi ebeveyn izni toplamak için bir deneyim giden (ve güncelleştirme `consentProvidedForMinor` alan).

###<a name="blocking-minors-without-parental-consent"></a>Reşit olmayanların ebeveyn izni olmadan engelleme
Yukarı ya da oturum izin, oturum ve kullanıcı akışları veya her ikisi için reşit olmayanların uygulamadan onayınız olmadan engellemeyi seçebilirsiniz.  Engellenen kullanıcılar Azure AD B2C'yi işlemek için iki seçenek vardır:
* Uygulamaya bir JSON gönder - Bu seçenek bir ikincil engellenen uygulama geri yanıt gönderir.
* Bir hata sayfası - kullanıcının uygulama erişemeyeceklerini bildiren bir sayfası gösterilecek Göster

##<a name="known-issues"></a>Bilinen sorunlar
###<a name="format-for-the-response-when-a-minor-is-blocked"></a>Küçük engellendiğinde, yanıt biçimi.
Yanıt şu anda doğru şekilde biçimlendirilmemiş, gelecek bir güncelleştirmede bu hatayı ele alınacaktır.

###<a name="deleting-specific-attributes-that-were-added-during-setup-can-make-your-directory-unable-to-use-age-gating"></a>Kurulum sırasında eklenen özel öznitelikler silme dizininize yaş geçidi kullanamaz hale getirebilirsiniz.
Kurulum yaş geçidi için dizin seçeneği ile yapılandırılan, `Properties`.  Ya da silerseniz `legalCountry` veya `dateOfBirth` Graph dizin artık yaş geçidi kullanabilirsiniz ve bu özellikleri yeniden oluşturulamıyor.

###<a name="list-of-countries-is-incomplete"></a>Ülkelerin listesi eksik
Şu anda için legalCountry ülkelerin listesi tam değil, gelecek bir güncelleştirmede ülkelerde rest ekleyeceğiz.