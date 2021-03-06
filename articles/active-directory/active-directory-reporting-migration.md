---
title: Azure portalında Azure Active Directory kullanıcı etkinlik raporları bulmak | Microsoft Docs
description: Azure portalında Azure Active Directory kullanıcı etkinlik raporları nerede olduğunu öğrenin.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: users-groups-roles
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f22219a0e2ff342e25a2efdeb319f389250ecfef
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231330"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Azure portalında etkinlik raporları Bul

Bu makalede, Azure portalında Azure Active Directory kullanıcı etkinlik raporları bulmak nasıl açıklanmaktadır.

## <a name="activity-and-integrated-app-reports"></a>Etkinlik ve tümleşik uygulama raporları

Azure portalında raporlama Bağlam tabanlı için varolan raporları tek bir görünümde birleştirilir. Tek bir API temel alınan veri görünümüne sağlar.

Bu görünüm görmek için **Azure Active Directory** dikey altında **etkinlik**seçin **denetim günlüklerini**.

![Denetim günlükleri](./media/active-directory-reporting-migration/482.png "Denetim günlükleri")

Aşağıdaki raporlar bu görünümde birleştirilir:

* Denetim raporu
* Parola sıfırlama etkinliği
* Parola sıfırlama kaydı etkinliği
* Self Servis Grup etkinliği
* Office365 grup adı değişikliği
* Hesap etkinlik sağlama
* Parola rollover durumu
* Hesap hazırlama hataları


Uygulama kullanım raporu geliştirilmiştir ve dahil **oturum açma işlemleri** görünümü. Bu görünüm görmek için **Azure Active Directory** dikey altında **etkinlik**seçin **oturum açma işlemleri**.

![Oturum açma işlemleri Görünüm](./media/active-directory-reporting-migration/483.png "oturum açma işlemleri görüntüleyin")

**Oturum açma işlemleri** görünümü, tüm kullanıcı oturum açma işlemleri içerir. Uygulama kullanım bilgilerini almak için bu bilgileri kullanın. Uygulama kullanım bilgilerini görüntülemek **kurumsal uygulamalar** genel bakış, **Yönet** bölümü.

![Kuruluş uygulamaları](./media/active-directory-reporting-migration/484.png "kurumsal uygulamalar")

## <a name="access-a-specific-report"></a>Belirli bir rapor erişim

Azure portalı tek bir görünüm sunmasına karşın özel raporları de bakabilirsiniz.

### <a name="audit-logs"></a>Denetim günlükleri

Azure portalında müşteri geri bildirimine yanıt Gelişmiş filtreleme istediğiniz verilere erişmek için kullanabilirsiniz. Kullanabileceğiniz bir filtredir bir *etkinlik kategorisi*, Azure AD'de oturum hangi etkinlik farklı türlerini listeler. Aradığınız ne için sonuçları daraltmak için bir kategori seçebilirsiniz.

Örneğin, yalnızca Self Servis parola sıfırlama için ilgili etkinlikleri de ilgileniyorsanız, seçebileceğiniz **Self Servis parola yönetimi** kategorisi. Gördüğünüz kategorileri çalıştığınız kaynak dayanır.  

![Filtre denetim günlüklerini sayfasındaki Kategori seçenekleri](./media/active-directory-reporting-migration/06.png "filtre denetim günlüklerini sayfasındaki Kategori seçenekleri")

Etkinlik kategorileri şunlardır:

- Çekirdek Dizin
- Self Servis Parola Yönetimi
- Self Servis Grup Yönetimi
- Hesap Sağlama

### <a name="application-usage"></a>Uygulama kullanımı

Altında tek bir uygulamayı veya tüm uygulamalar için uygulama kullanımı hakkındaki ayrıntıları görüntülemek için **etkinlik**seçin **oturum açma işlemleri**. Sonuçları daraltmak için kullanıcı adı veya uygulama adı filtre uygulayabilirsiniz.

![Filtre oturum açma olayları sayfası](./media/active-directory-reporting-migration/07.png "filtre oturum açma olayları sayfası")

### <a name="security-reports"></a>Güvenlik raporları

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD anormal etkinlik raporları

Azure AD anormal bir etkinliğin güvenlik raporları birleştirilmiş bir merkezi görünümünü sağlamak için. Bu görünüm, Azure AD algılamak ve raporlama tüm risk güvenlikle ilgili olayları gösterir.

Aşağıdaki tablo listeleri Azure AD anormal bir etkinliğin güvenlik raporları ve Azure portalında karşılık gelen risk olayı türleri.

| Azure AD anormal Etkinlik Raporu |  Kimlik koruması risk olay türü|
| :--- | :--- |
| Sızan kimlik bilgilerine sahip kullanıcılar | Sızdırılan kimlik bilgileri |
| Düzensiz oturum açma etkinliği | Alışılmadık konumlara imkansız seyahat |
| Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri | Bulaşma olan cihazlardan oturum açma işlemleri|
| Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri | Anonim IP adreslerinden oturum açma işlemleri |
| Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri | Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |
| - | Alışılmadık konumlardan oturum açma işlemleri |

Aşağıdaki Azure AD anormal bir etkinliğin güvenlik raporları olmayan risk olaylarını Azure portalında eklendi:

* Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri
* Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri

Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md).  


#### <a name="detected-risk-events"></a>Algılanan risk olayı

Azure portalında algılanan risk olayı hakkında raporlar üzerinde erişebilirsiniz **Azure Active Directory** dikey altında **güvenlik**. Algılanan risk olayı aşağıdaki raporlarda izlenir:   

- Risk altındaki kullanıcılar
- Riskli Oturum Açma İşlemleri

![Güvenlik raporları](./media/active-directory-reporting-migration/04.png "güvenlik raporları")

Güvenlik raporları hakkında daha fazla bilgi için bkz:

- [Azure Active Directory portalında Risk güvenlik raporu kullanıcılar](active-directory-reporting-security-user-at-risk.md)
- [Azure Active Directory portalında riskli oturum açma işlemleri raporu](active-directory-reporting-security-risky-sign-ins.md)


Görüntülemek için **uygulama kullanımı** üzerinde rapor **Azure Active Directory** dikey altında **Yönet**seçin **kurumsal uygulamalar**, ve ardından **oturum açma işlemleri**.


![Kuruluş uygulamaları oturum açma işlemleri raporu](./media/active-directory-reporting-migration/199.png)

## <a name="next-steps"></a>Sonraki adımlar

Raporlamaya genel bir bakış için bkz. [Azure Active Directory raporlama](active-directory-reporting-azure-portal.md).
