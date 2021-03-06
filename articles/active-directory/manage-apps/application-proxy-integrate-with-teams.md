---
title: Erişim takımlar Azure AD uygulama proxy'si uygulamalarında | Microsoft Docs
description: Şirket içi uygulamanızı Microsoft Teams erişmek için Azure AD uygulama proxy'si kullanın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: b2d240e6339902ce24061b5db3ad1b8c2915eb59
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303903"
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Şirket içi uygulamalarınızı Microsoft Teams erişme

Azure Active Directory Uygulama Ara sunucusu, çoklu oturum açma, nerede olursa olsun, şirket içi uygulamalara sağlar. Microsoft Teams işbirliği çabalarınız tek bir yerde kolaylaştırır. İki birlikte tümleştirme, kullanıcılarınızın kendi Ekip arkadaşları hiçbir durumda ile üretken olabileceği anlamına gelir. 

Kullanıcılarınızın kendi takımlar kanalları bulut uygulamalarını ekleyebilirsiniz [sekmeleri kullanarak](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), ancak ne şirket içi SharePoint siteleri veya olan planlama aracı barındırılan? Uygulama proxy'si çözümüdür. Bunlar her zaman uygulamalarını uzaktan erişmek için kullandıkları aynı dış URL'ler kullanarak kendi kanallara uygulama proxy'si aracılığıyla yayımlanan uygulamalar ekleyebilirsiniz. Ve uygulama proxy'si Azure Active Directory kimlik doğrulaması için çoklu oturum açma deneyimini kullanıcılarınızın alırsınız.


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>Uygulama Ara sunucusu Bağlayıcısı'nı yüklemek ve uygulamanızı yayınlama

Henüz yapmadıysanız [bağlayıcı yükleyip kiracınız için uygulama proxy'si yapılandırmanıza](application-proxy-enable.md). Ardından, [şirket içi uygulamanızı yayımlamak](application-proxy-publish-azure-portal.md) uzaktan erişim için. Uygulama yayımlarken uygulama takıma eklemek için kullanıldığından dış URL'yi not edin.

Zaten varsa yayımlanan uygulamalarınızı ancak bunların dış URL'ler hatırlama bunları içinde aramak [Azure portal](https://portal.azure.com). Oturum açın ve ardından gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** > uygulamanızı seçin >  **Uygulama proxy'si**.

## <a name="add-your-app-to-teams"></a>Uygulamanızı takıma ekleme

Uygulama proxy'si aracılığıyla uygulama yayımladıktan sonra bunlar bir sekmede doğrudan kendi takımlar kanalları olarak ekleyebilir ve sonra uygulama için kullanılacak ekipteki herkesin kullanılabilir biliyorsanız, kullanıcılarınızın olanak tanır. Bunları aşağıdaki üç adımı izleyin vardır:

1. Bu uygulamayı eklemek ve seçmek için istediğiniz takımlar kanal gidin **+** bir sekme eklemek için.

   ![Sekme Ekle seçin](./media/application-proxy-integrate-with-teams/add-tab.png)

2. Seçin **Web sitesi** sekmesini seçenekleri.

   ![Web sitesi ekleme](./media/application-proxy-integrate-with-teams/website.png)

3. Sekme bir ad verin ve uygulama proxy'si dış URL'sine ayarlayın. 

   ![Sekme adı ve URL yapılandırın](./media/application-proxy-integrate-with-teams/tab-name-url.png)

Bir ekibin üyesi sekmesi ekler sonra herkesin kanal görüntülersiniz. Uygulama erişimi olan tüm kullanıcıların tek oturum açma Microsoft Teams için kullandıkları kimlik bilgileriyle erişin. Uygulama erişimi olmayan tüm kullanıcılar takımlar sekmesinde görebilirsiniz, ancak bunları izinleri şirket içi uygulama ve uygulamanın Azure portal yayımlanan sürümü için size kadar engellenir. 

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [şirket içi SharePoint siteleri yayımlamak](application-proxy-integrate-with-sharepoint-server.md) uygulama proxy'si ile.
- Kullanmak için uygulamalarınızı yapılandırmanızı [özel etki alanlarını](application-proxy-configure-custom-domain.md) kendi dış URL. 
