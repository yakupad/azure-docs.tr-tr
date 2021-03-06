---
title: Akıllı kilitleme Azure AD kullanarak bir deneme yanılma saldırılarını önleme
description: Azure Active Directory akıllı kilitleme Kuruluşunuz parolalar tahmin etmeye çalışırken deneme yanılma saldırılarına karşı korumaya yardımcı olur.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: b0fded9f5543d151091955c0b0d645bf9db16b7d
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158592"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory akıllı kilitleme

Akıllı kilitleme da bulut zekasından kullanıcılarınızın parolaları tahmin veya almak için deneme yanılma yöntemleri kullanmak için çalışan bir kötü aktörleri kullanıma kilitlemek için kullanır. O gösterimi geçerli kullanıcılardan gelen oturum açma tanıyabilir ve bunları farklı olanları saldırganlar ve bilinmeyen diğer kaynakları ele almanız. Akıllı kilitleme saldırganlar, izin vermek, kullanıcılarınızın hesaplarına erişmek ve üretken devam ederken kullanıma kilitler.

Varsayılan olarak, akıllı kilitleme oturum açma denemesi on girişimi başarısız olduktan sonra bir dakika hesabından kilitler. Sonraki başarısız oturum açma girişimleri, ilk ve sonraki denemeler de daha uzun bir dakika sonra yeniden hesabı kilitler.

Akıllı kilitleme her zaman doğru karışımını güvenlik ve kullanılabilirlik sunan bu varsayılan ayarları olan tüm Azure AD müşterileri için açıktır. Akıllı kilitleme ayarları, kuruluşunuza özgü değerlerle özelleştirmesini kullanıcılarınız için Azure AD temel veya daha yüksek lisansı gerektirir.

Akıllı kilitleme, saldırganlar tarafından kilitli şirket içi Active Directory hesapları korumak için parola karması eşitleme veya doğrudan kimlik doğrulaması'nı kullanarak, karma dağıtımlar ile tümleştirilebilir. Şirket içi Active Directory ulaşmadan önce akıllı kilitleme ilkeleri Azure AD'de uygun şekilde ayarlayarak saldırıları filtrelenebilen.

Kullanırken [geçişli kimlik doğrulaması](../connect/active-directory-aadconnect-pass-through-authentication.md), emin olmanız gerekir:

   * Azure AD kilitleme eşiği **daha az** Active Directory hesap kilitleme eşik değerinden. Böylece Active Directory hesap kilitleme eşiği en az iki veya üç kez Azure AD kilitleme eşik değerinden daha uzun değerleri ayarlayın. 
   * Azure AD kilitleme süresi **saniye** olduğu **uzun** Active Directory süreden sonra hesap kilitleme sayacını sıfırla daha **dakika**.

> [!IMPORTANT]
> Şu anda bunlar akıllı kilitleme özelliğini tarafından kilitlenmiş, bir yönetici kullanıcıların bulut hesapları kilidi açılamıyor. Yönetici, kilitleme süresi dolmak üzere beklemeniz gerekir.

## <a name="verify-on-premises-account-lockout-policy"></a>Şirket içi hesap kilitleme ilkesi doğrulayın

Şirket içi Active Directory hesap kilitleme ilkesi doğrulamak için aşağıdaki yönergeleri kullanın:

1. Grup İlkesi Yönetimi Aracı'nı açın.
2. Örneğin, kuruluşunuzun hesap kilitleme ilkesi içeren bir Grup İlkesi düzenleme **varsayılan etki alanı ilkesi**.
3. Gözat **Bilgisayar Yapılandırması** > **ilkeleri** > **Windows ayarları** > **güvenlik ayarları**   >  **Hesap ilkeleri** > **hesap kilitleme ilkesi**.
4. Doğrulayın, **hesap kilitleme eşiği** ve **sonra hesap kilitleme sayacını sıfırla** değerleri.

![Bir Grup İlkesi nesnesi kullanarak şirket içi Active Directory hesap kilitleme ilkesi](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD akıllı kilitleme değerleri yönetme

Kuruluş gereksinimlerinize bağlı olarak, akıllı kilitleme değerleri özelleştirilmiş gerekebilir. Akıllı kilitleme ayarları, kuruluşunuza özgü değerlerle özelleştirmesini kullanıcılarınız için Azure AD temel veya daha yüksek lisansı gerektirir.

Denetleyin veya kuruluşunuz için akıllı kilitleme değerleri değiştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com), tıklayın **Azure Active Directory**, ardından **kimlik doğrulama yöntemleri**.
1. Ayarlama **kilitleme eşiği**bağlı olarak kaç başarısız oturum açma hesabınız, ilk kilitlenmeden önce izin verilir. Varsayılan değer 10'dur.
1. Ayarlama **kilitleme süresini saniye cinsinden**, her kilitleme saniye cinsinden uzunluğu. 60 saniye (bir dakika) varsayılandır.

> [!NOTE]
> İlk kilitleme ayrıca başarısız olduktan sonra oturum, hesabı yeniden kilitler durumunda. Bir hesabın tekrar tekrar kilitler, kilitleme süresi artar.

![Azure portalında Azure AD akıllı kilitleme ilkesi özelleştirme](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Sonraki adımlar

[Azure AD kullanarak kuruluşunuzdaki yanlış parolalar yasaklamak öğrenin.](howto-password-ban-bad.md)

[Self Servis parola sıfırlamayı kullanıcılara kendi hesaplarının kilidini açmak izin verecek şekilde yapılandırın.](quickstart-sspr.md)