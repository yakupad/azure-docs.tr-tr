---
title: 'Azure Active Directory etki alanı Hizmetleri: Özellikleri | Microsoft Docs'
description: Azure Active Directory etki alanı Hizmetleri özellikleri
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: maheshu
ms.openlocfilehash: 5dd3cde69c6aa36c3d9cb3060dc6deb59ff74a5a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214976"
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Özellikler
Aşağıdaki özellikler Azure AD etki alanı Hizmetleri yönetilen etki alanlarında kullanılabilir.

* **Basit Dağıtım deneyimi:** yalnızca birkaç tıklama kullanarak Azure AD dizininiz için Azure AD Etki Alanı Hizmetleri'ni etkinleştirebilirsiniz. Yönetilen etki alanınızı yalnızca bulut kullanıcı hesapları ve bir şirket içi Directory'den eşitlenen kullanıcı hesapları içerir.
* **Etki alanına katılma için destek:** yönetilen etki alanınızı sağlanmıştır Azure sanal ağındaki etki alanına katılma bilgisayarları kolayca. Windows istemci ve sunucu işletim sistemleri works sorunsuz bir şekilde Azure AD etki alanı Hizmetleri tarafından hizmet verilen etki alanları karşı etki alanına katılım deneyimi. Bu tür etki alanlarına yönelik tooling otomatik etki alanına katılma de kullanabilirsiniz.
* **Azure AD dizini başına tek bir etki alanı örnek:** her Azure AD dizini için tek bir Active Directory etki alanı oluşturabilirsiniz.
* **Özel adlara sahip etki alanları oluşturun:** özel adları (örneğin, ' contoso100.com') olan Azure AD Etki Alanı Hizmetleri'ni kullanarak etki alanları oluşturabilirsiniz. Her iki doğrulanmış veya doğrulanmamış etki alanı adlarını kullanabilirsiniz. İsteğe bağlı olarak, bir etki alanının yerleşik etki alanı sonekiyle oluşturabilirsiniz (diğer bir deyişle, ' *. onmicrosoft.com') Azure AD dizininizi tarafından sunulan.
* **Azure AD ile tümleşik:** yapılandırmak veya Azure AD etki alanı Hizmetleri çoğaltma yönetmek gerekmez. Kullanıcı hesapları, grup üyelikleri ve kullanıcı kimlik bilgilerini (Parolalar) Azure AD dizininizi Azure AD Etki Alanı Hizmetleri'nde otomatik olarak kullanılabilir. Yeni kullanıcıların, grupların veya özniteliklere yapılan değişiklikleri Azure AD kiracınıza veya şirket içi dizininizi Azure AD etki alanı Hizmetleri için otomatik olarak eşitlenir.
* **NTLM ve Kerberos kimlik doğrulaması:** NTLM ve Kerberos kimlik doğrulaması için destek ile Windows-Integrated kimlik doğrulamasını kullanan uygulamalar dağıtabilirsiniz.
* **Kurumsal kimlik bilgileri/parolalarınızı kullanın:** kullanıcıların Azure ad parolalarını Azure AD etki alanı Hizmetleri ile iş Kiracı. Kullanıcıların etki alanına katılma makinelere şirket kimlik bilgilerini kullanmak, etkileşimli olarak veya Uzak Masaüstü üzerinden oturum açın ve yönetilen etki alanına göre kimlik doğrulaması.
* **LDAP bağ & LDAP okuma desteği:** LDAP bağlamalar Azure AD etki alanı Hizmetleri tarafından hizmet verilen etki alanlarındaki kullanıcılar kimlik doğrulaması kullanan uygulamalar kullanabilirsiniz. Ayrıca, LDAP okuma işlemleri için sorgu kullanıcı/bilgisayar öznitelikleri dizinden kullanan uygulamalar ayrıca Azure AD Etki Alanı Hizmetleri'ne karşı çalışabilirsiniz.
* **Güvenli LDAP (LDAPS):** dizinine erişimi güvenli LDAP (LDAPS üzerinden) etkinleştirebilirsiniz. Güvenli LDAP erişim varsayılan olarak sanal ağ içinde kullanılabilir. Ancak, güvenli LDAP erişim ayrıca isteğe bağlı olarak internet üzerinden etkinleştirebilirsiniz.
* **Grup İlkesi:** kullanıcılar ve bilgisayarlar için tek bir yerleşik GPO'yu her kullanabileceğiniz kullanıcı hesapları ve etki alanına katılmış bilgisayarlar için güvenlik ilkeleri ile uyumluluğu zorlamak üzere kapsayıcıları gerekli. Ayrıca, kendi özel GPO oluşturma ve özel kuruluş birimlerine atamanız [grup ilkesini yönetmenize](active-directory-ds-admin-guide-administer-group-policy.md).
* **DNS yönetme:** 'AAD DC Yöneticiler' grubunun üyeleri, DNS Yönetim MMC ek bileşenini gibi bilinen DNS yönetim araçları kullanarak yönetilen etki alanınız için DNS yönetebilir.
* **Özel Kuruluş birimlerini (OU) oluşturun:** 'AAD DC Yöneticiler' grubunun üyeleri, yönetilen etki alanında özel OU'ları oluşturabilirsiniz. Bunlar Ekle/Kaldır böylece hizmet hesaplarını, bilgisayarlar, grupların vb. Bu özel OU içinde bu kullanıcıların özel OU'ları üzerinde tam yönetimsel ayrıcalıklara verilir.
* **Çok sayıda Azure genel bölgelerde kullanılabilir:** bkz [bölgeye göre Azure Hizmetleri](https://azure.microsoft.com/regions/#services/) Azure AD etki alanı Hizmetleri olduğu kullanılabilir olduğu Azure bölgelerini öğrenmek için sayfa.
* **Yüksek Kullanılabilirlik:** Azure AD etki alanı Hizmetleri etki alanınız için yüksek kullanılabilirlik sağlar. Bu özellik yüksek hizmeti çalışır durumda kalma süresi ve esnekliği hatalarına garanti sunar. Yerleşik durum teklifleri izleme hatalardan düzeltme başarısız örnekleri değiştirmek için ve etki alanınız için sürekli hizmet sağlamak için yeni örnekleri oluşturan dönen tarafından otomatik.
* **AD hesabı kilitleme koruması:** kullanıcı hesapları kilitli 30 dakika boyunca beş geçersiz parolaların 2 dakika içinde kullanılıyorsa. 30 dakika sonra otomatik olarak kilidi hesaplarıdır.
* **Tanıdık yönetim araçlarını kullanın:** yönetilen etki alanlarını yönetmek için Active Directory Yönetim Merkezi'ni veya Active Directory PowerShell gibi bilinen Windows Server Active Directory yönetim araçlarını kullanabilirsiniz.
