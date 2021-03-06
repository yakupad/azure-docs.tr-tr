---
title: Güvenli Azure VPN ağ geçidi için RADIUS kimlik doğrulamasını NPS sunucusu ile multi-Factor Authentication | Microsoft Docs
description: Açıklar Azure ağ geçidi RADIUS kimlik doğrulaması için çok faktörlü kimlik doğrulamasını NPS sunucusu ile tümleştirme.
services: vpn-gateway
documentationcenter: na
author: ahmadnyasin
manager: willchen
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/13/2018
ms.author: genli
ms.openlocfilehash: 70c760cd0cb571cc95250ab793829b060341e0ed
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056587"
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Azure VPN ağ geçidi RADIUS kimlik doğrulaması için çok faktörlü kimlik doğrulamasını NPS sunucusu ile tümleştirme 

Makale, ağ ilkesi sunucusu (NPS), noktadan siteye VPN bağlantıları için multi-Factor Authentication (MFA) sunmak için Azure VPN ağ geçidi RADIUS kimlik doğrulaması ile tümleştirmeyi açıklamaktadır. 

## <a name="prerequisite"></a>Önkoşul

Mfa'yı etkinleştirmek için kullanıcıların Azure Active ya da şirket içinden eşitlenen veya Bulut ortamı Directory'de (Azure AD) olması gerekir. Ayrıca, kullanıcı zaten otomatik kayıt işlemi için mfa'yı tamamlamış olmanız gerekir.  Daha fazla bilgi için [hesabım için iki aşamalı doğrulama ayarlayın](../active-directory/user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="detailed-steps"></a>Ayrıntılı adımlar

### <a name="step-1-create-a-virtual-network-gateway"></a>1. adım: sanal ağ geçidi oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sanal ağ geçidini barındıran sanal ağında seçin **alt ağlar**ve ardından **ağ geçidi alt ağı** bir alt ağ oluşturmak için. 

    ![Ağ geçidi alt ağı ekleme hakkında görüntü](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Aşağıdaki ayarlar belirterek bir sanal ağ geçidi oluşturun:

    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: seçin **rota tabanlı**.
    - **SKU**: gereksinimlerinize göre bir SKU türüne göre seçin.
    - **Sanal ağ**: ağ geçidi alt ağı oluşturduğunuz sanal ağı seçin.

        ![Sanal ağ geçidi ayarlarını hakkında görüntü](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>2. adım, Azure MFA için NPS yapılandırma

1. NPS sunucu üzerinde [Azure MFA için NPS uzantısını yükleme](../active-directory/authentication/howto-mfa-nps-extension.md#install-the-nps-extension).
2. NSP konsolunu açın, sağ **RADUIS istemcileri**ve ardından **yeni**. Aşağıdaki ayarlar belirterek RADUIS istemci oluşturun:

    - **Kolay ad**: herhangi bir ad yazın.
    - **Adres (IP veya DNS)**: 1. adımda oluşturduğunuz ağ geçidi alt ağı girin.
    - **Paylaşılan gizli diziyi**: herhangi bir gizli anahtar yazın ve daha sonra kullanmak üzere unutmayın.

    ![Görüntünün RADUIS istemci ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  Üzerinde **Gelişmiş** sekmesinde, satıcı adı kümesine **RADIUS standart** emin olun **ek seçenekler** onay kutusu seçilmez.

    ![Görüntünün RADUIS istemci Gelişmiş ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Git **ilkeleri** > **ağ ilkeleri**, çift **Microsoft Routing ve Uzaktan erişim sunucusuna bağlantılarda** ilkesi seçin  **Erişim ver**ve ardından **Tamam**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>3. adım yapılandırma sanal ağ geçidi

1. Oturum [Azure portalında](https://portal.azure.com).
2. Oluşturduğunuz sanal ağ geçidi'ni açın. Ağ geçidi türü ayarlandığından emin olun **VPN** ve VPN türünde **rota tabanlı**.
3. Tıklayın **noktası site yapılandırmasını** > **Şimdi Yapılandır**ve ardından aşağıdaki ayarları belirtin:

    - **Adres havuzu**: 1. adımda oluşturduğunuz ağ geçidi alt ağı girin.
    - **Kimlik doğrulama türü**: seçin **RADIUS kimlik doğrulaması**.
    - **Sunucu IP adresi**: NPS sunucusunun IP adresini yazın.

    ![Site ayarları noktasına hakkında görüntü](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md)
- [Mevcut NPS altyapınızı Azure Multi-Factor Authentication ile tümleştirme](../active-directory/authentication/howto-mfa-nps-extension.md)
