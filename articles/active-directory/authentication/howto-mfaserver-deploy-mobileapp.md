---
title: Azure MFA Sunucusu Mobil Uygulama Web Hizmeti | Microsoft Belgeleri
description: MFA sunucusunu Microsoft Authenticator Uygulaması ile kullanıcılara anında iletme bildirimleri göndermek için yapılandırın.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: d31cf6569f642d2c7b61abe25148f5cfb199f0a2
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159449"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile mobil uygulama kimlik doğrulamasını etkinleştirme

Microsoft Authenticator uygulaması ek bir bant dışı doğrulama seçeneği sunar. Oturum açma sırasında kullanıcıya otomatik telefon çağrısı yapmak veya SMS göndermek yerine, Azure Multi-Factor Authentication, kullanıcının akıllı telefonu ya da tabletindeki Microsoft Authenticator uygulamasına anında iletme bildirimi gönderir. Kullanıcının oturum açma işlemini tamamlamak için uygulamada **Doğrula** seçeneğine dokunması (ya da PIN’i girip “Kimliği Doğrula” seçeneğine dokunması ) yeterlidir.

Şebeke sinyal gücünün güvenilir olmadığı durumlarda iki aşamalı doğrulama için bir mobil uygulama kullanmak tercih edilir. Uygulamayı bir OATH belirteci oluşturucu olarak kullanıyorsanız ağ veya İnternet bağlantısı gerekmez.

> [!IMPORTANT]
> Azure Multi-Factor Authentication Sunucusu v8.x veya üzeri yüklüyse aşağıdaki adımları gerçekleştirmeniz gerekmez. [Mobil uygulamayı yapılandırma](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) bölümündeki adımlar izlenerek mobil uygulama kimlik doğrulaması ayarlanabilir.

## <a name="requirements"></a>Gereksinimler

Microsoft Authenticator uygulamasını kullanmak için Azure Multi-Factor Authentication Sunucusu v8.x veya üzerini çalıştırıyor olmanız gerekir

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma

1. Multi-Factor Authentication Sunucusu konsolunda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesindeki **Kullanıcıların yöntemi seçmesine izin ver** bölümünden **Mobil Uygulama**’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. **Kullanıcıların Mobil Uygulamaları etkinleştirmesine izin ver** kutusunu işaretleyin.
3. **Kullanıcı Kaydına İzin Ver** kutusunu işaretleyin.
4. **Mobil Uygulama** simgesine tıklayın.
5. **Hesap adı** alanına bu hesabın mobil uygulamasında görüntülenecek şirket veya kuruluş adını girin.
   ![MFA Sunucusu yapılandırması Mobil Uygulama ayarları](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ve üçüncü taraf VPN’ler ile gelişmiş senaryolar](howto-mfaserver-nps-vpn.md).
