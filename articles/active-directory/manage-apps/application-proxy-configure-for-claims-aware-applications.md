---
title: Talep kullanan uygulamalar - Azure AD uygulama proxy'si | Microsoft Docs
description: Kullanıcılarınız tarafından güvenli uzaktan erişim için ADFS talep kabul şirket içi ASP.NET uygulamaları yayımlamak nasıl.
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
ms.date: 08/04/2017
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: 1618200ce3d96013f3d7b05db53163c993efc69a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161997"
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Talep kullanan uygulamalarda uygulama proxy'si ile çalışma
[Talep kullanan uygulamalar](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) yeniden yönlendirmesi için güvenlik belirteci hizmeti (STS) gerçekleştirin. STS bir belirteç karşılığında kullanıcı kimlik bilgilerini ister ve ardından kullanıcının uygulamaya yönlendirir. Bu yeniden yönlendirmeleri ile çalışmak uygulama proxy'si etkinleştirmek için birkaç yolu vardır. Dağıtımınızı talep kullanan uygulamalar için yapılandırmak için bu makaleyi kullanın. 

## <a name="prerequisites"></a>Önkoşullar
Talep kullanan uygulama yönlendirir STS Şirket ağınızın dışında kullanılabilir olduğundan emin olun. STS kullanılabilir bir proxy üzerinden gösterme ya da dış bağlantılara izin yapabilirsiniz. 

## <a name="publish-your-application"></a>Uygulamanızı yayımlama

1. Uygulamanızı bölümünde açıklanan yönergeleri göre yayımlamak [uygulama proxy'si ile uygulama yayımlama](application-proxy-publish-azure-portal.md).
2. Seçin ve Portalı'nda uygulama sayfası gidin **çoklu oturum açma**.
3. Seçerseniz **Azure Active Directory** olarak, **ön kimlik doğrulama yöntemi**seçin **Azure AD çoklu oturum açma devre dışı özelliğini** olarak, **iç Kimlik doğrulama yöntemini**. Seçerseniz **geçiş** olarak, **ön kimlik doğrulama yöntemi**, değişikliği gerekmez.

## <a name="configure-adfs"></a>ADFS yapılandırın

İki yoldan biriyle talep kullanan uygulamalar için ADFS yapılandırabilirsiniz. İlk özel etki alanlarını kullanmaktır. WS-Federasyon ile saniyedir. 

### <a name="option-1-custom-domains"></a>Seçenek 1: Özel etki alanları

Tüm iç URL'leri uygulamalarınız için tam olarak nitelenmiş etki alanı adlarını (FQDN) sonra yapılandırabileceğiniz [özel etki alanlarını](application-proxy-configure-custom-domain.md) uygulamalarınız için. Özel etki alanlarını iç URL'ler ile aynı olan dış URL'ler oluşturmak için kullanın. Ardından, dış URL'ler iç URL'nizde eşleştiğinde, kullanıcılarınızın şirket içi veya uzak olup STS yeniden yönlendirme çalışır. 

### <a name="option-2-ws-federation"></a>Seçenek 2: WS-Federasyon

1. ADFS Yönetimi'ni açın.
2. Git **bağlı olan taraf güvenleri**uygulama ara sunucusu ile yayımlıyorsa uygulama sağ tıklatın ve seçin **özellikleri**.  

   ![Bağlı olan taraf güvenleri sağ uygulama adı - ekran görüntüsü](./media/application-proxy-configure-for-claims-aware-applications/appproxyrelyingpartytrust.png)  

3. Üzerinde **uç noktaları** sekmesinde, altında **uç nokta türü**seçin **WS-Federasyon**.
4. Altında **güvenilen URL**, girdiğiniz uygulama proxy'si ile yapılandırılabilir.%nd;a;%nd;a;%nişlem URL'si girin **dış URL** tıklatıp **Tamam**.  

   ![Bir uç nokta - ekleyin güvenilen URL değeri - ekran görüntüsü Ayarla](./media/application-proxy-configure-for-claims-aware-applications/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Sonraki adımlar
* [Çoklu oturum açmayı etkinleştir](application-proxy-single-sign-on.md) talep kullanan olmayan uygulamalar için
* [Proxy uygulamaları ile etkileşim kurmak yerel istemci uygulamaları etkinleştirme](application-proxy-configure-native-client-application.md)


