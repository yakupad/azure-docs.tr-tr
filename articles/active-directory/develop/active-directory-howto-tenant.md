---
title: Azure AD kiracısı edinme | Microsoft Belgeleri
description: Uygulamaları kaydetmek ve oluşturmak üzere Azure Active Directory kiracısı edinme.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/23/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 824d7c44488e382aef9fd0640e6c46e9f4672898
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317512"
---
# <a name="how-to-get-an-azure-active-directory-tenant"></a>Azure Active Directory kiracısı edinme

Azure Active Directory'de (Azure AD) [kiracı](https://msdn.microsoft.com/library/azure/jj573650.aspx#Anchor_0), bir kuruluşun temsilcisidir. Bu, bir kuruluşun, Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmetine kaydolduğunda olduğu gibi, Microsoft ile bir ilişki oluşturduğunda aldığı ve sahip olduğu adanmış bir Azure AD hizmeti örneğidir. Her Azure AD kiracısı benzersizdir ve diğer Azure AD kiracılarından ayrıdır. 

Kiracı, bir şirket içindeki kullanıcıları ve bunlarla ilgili bilgileri (parolalarını, kullanıcı profili verilerini, izinlerini vb.) barındırır. Ayrıca bir kuruluşa ve kuruluşun güvenliğine ilişkin grupları, uygulamaları ve diğer bilgileri de içerir.

Azure AD kullanıcılarının uygulamanızda oturum açmasına olanak tanımak için uygulamanızı kendi kiracınıza kaydetmeniz gerekir. Azure AD kiracısı oluşturma ve bu kiracıda bir uygulama yayımlama **kesinlikle ücretsizdir** (öte yandan, kiracınızdan premium özellikler için ödeme yapmayı seçebilirsiniz). Hatta birçok geliştirici, deneme, geliştirme, hazırlama ve test etme amacıyla çeşitli kiracılar ve uygulamalar oluşturur.

## <a name="use-an-existing-azure-ad-tenant"></a>Mevcut bir Azure AD kiracısı kullanma

Birçok geliştiricinin, Azure AD kiracılarına bağlı hizmetler veya abonelikler (örneğin: Office 365 veya Azure abonelikleri) aracılığıyla zaten kiracıları vardır. Önceden bir kiracınız olup olmadığını denetlemek için, uygulamanızı yönetmek için kullanmak istediğiniz hesapla [Azure portalında](https://portal.azure.com) oturum açın ve hesap bilgilerinizin gösterildiği sağ üst köşeyi denetleyin. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görürsünüz. Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelirseniz, adınız, e-postanız, dizininiz ve kiracı kimliğiniz (GUID) ile etki alanınızı görürsünüz. Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır.

> [!TIP]
> Kiracı kimliğini bulmanız gerekiyorsa, bu bilgileri bulmanın birden çok yolu vardır. Kiracı kimliğini almak için hesap adınızın üzerine gelebilir veya Azure portalda **Azure Active Directory > Özellikler > Dizin Kimliği**’ni seçebilirsiniz.

Hesabınızla ilişkili mevcut bir kiracınız yoksa, hesap adınızın altında bir GUID görürsünüz ve siz [yeni bir kiracı oluşturmadan](#create-a-new-azure-ad-tenant) uygulamaları kaydetme gibi işlemler gerçekleştiremezsiniz.

## <a name="create-a-new-azure-ad-tenant"></a>Yeni Azure AD kiracısı oluşturma

Önceden bir Azure AD kiracınız yoksa veya yeni bir Azure AD kiracısı oluşturmak istiyorsanız, [Azure portalındaki](https://portal.azure.com) [dizin oluşturma deneyimini](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) kullanarak bunu yapabilirsiniz. İşlem yaklaşık bir dakika sürer ve sonunda, yeni oluşturulan kiracınıza gitmeniz istenir.
