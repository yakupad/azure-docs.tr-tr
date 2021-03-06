---
title: Azure AD Connect sistem durumu ve kullanıcı gizliliğini | Microsoft Docs
description: Bu belgede, Azure AD Connect Health ile kullanıcı gizliliğini açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: billmath
ms.openlocfilehash: 5fedbac439636b56da217e7babd30820bce7b342
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38442575"
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Kullanıcı gizliliği ve Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makalede, Azure AD Connect Health ve kullanıcı gizlilik ile ilgilidir.  Azure AD Connect ve kullanıcı gizliliği hakkında bilgi için bkz [burada](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="user-privacy-classification"></a>Kullanıcı gizliliği sınıflandırma
Azure AD Connect Health denk içine **veri işlemcisi** GDPR sınıflandırması kategorisidir. Bir veri işlemcisi işlem hattı anahtar iş ortakları ve son tüketici hizmeti veri işleme hizmetleri sağlar. Azure AD Connect Health, kullanıcı verilerini oluşturmaz ve bağımsız hangi kişisel veriler üzerinde toplanır ve nasıl kullanılacağını denetleyemez. Veri alma, toplama, analiz ve raporlama Azure AD Connect Health mevcut şirket içi verileri temel alır. 

## <a name="data-retention-policy"></a>Veri saklama ilkesi
Azure AD Connect Health raporları oluşturmaz, analizler gerçekleştirin veya 30 gün ayrıntılı bilgiler sağlar. Bu nedenle, Azure AD Connect Health depolamaz, işlem veya her 30 gün saklanması. Bu tasarım, GDPR düzenlemeler, Microsoft gizlilik uyumluluk düzenlemelerini ve Azure AD veri saklama ilkeleri ile uyumludur. 

Etkin sunucularıyla **sistem sağlığı hizmeti verileri güncel değil** **hata** hiçbir veri bu zaman aralığı sırasında Connect Health ulaştı 30 gün arka arkaya kullanılmaması önermek için uyarılar. Bu sunucular devre dışı ve Connect Health Portalı'nda gösterilmez. Sunucuları yeniden etkinleştirmek için öncelikle kaldırmanız ve [health aracısını yeniden](active-directory-aadconnect-health-agent-install.md). Lütfen bu için uygulanmaz unutmayın **uyarıları** aynı uyarı türüne sahip. Uyarılar, kısmi veri için uyarılırsınız sunucudan eksik olduğunu gösterir. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Veri toplama ve Azure AD Connect Health izleme devre dışı bırak
Azure AD Connect Health, izlenen her bir sunucunun veya izlenen bir hizmet örneği için veri toplamayı durdurmak sağlar. Örneğin, Azure AD Connect Health'i kullanma izlenen bireysel ADFS (Active Directory Federasyon Hizmetleri) sunucuları için veri toplamayı durdurabilirsiniz. Ayrıca, Azure AD Connect Health'i kullanma izlenmekte olan tüm ADFS örneği için veri toplamayı durdurabilirsiniz. Bunu yapmak seçtiğinizde, karşılık gelen sunucuları Azure AD Connect Health Portalı'ndan veri toplamayı durdurduktan sonra silinir. 

>[!IMPORTANT]
> Azure AD genel yönetici ayrıcalıkları veya RBAC katkıda bulunan rolü silme izlenen sunucular için Azure AD Connect Health gerekir.
>
> Bir sunucu veya hizmet örneği, Azure AD Connect Health kaldırılıyor, ters çevrilebilir bir eylem değil. 

### <a name="what-to-expect"></a>Neler?
Veri toplama ve izlenen tek bir sunucu veya izlenen bir hizmet örneği için izleme durdurursanız, aşağıdakilere dikkat edin:

- İzlenen bir hizmet örneği silme işleminin örnek portalı Azure AD Connect Health izleme hizmeti listeden kaldırılır. 
- İzlenen bir sunucuya veya izlenen bir hizmet örneği silme işleminin olmayan sistem durumu aracısı kaldırılabilir veya sunucularınızdan kaldırıldı. Durum Aracısı, verileri Azure AD Connect Health için göndermek için yapılandırılır. Daha önce izlenen sunucularda sistem durumu aracısı el ile kaldırmanız gerekir.
- Durum Aracısı, bu adımı gerçekleştirmeden önce kaldırmadıysanız, kaldırmadıysanız sunucularda sistem durumu aracısı ile ilgili hata olayları görebilirsiniz.
- İzlenen hizmet örneğine ait tüm veriler, Microsoft Azure veri bekletme ilkesi uyarınca silinir.

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Veri toplama ve izlenen bir hizmet örneği için izleme devre dışı bırak
Bkz: [bir hizmet örneği Azure AD Connect Health kaldırma](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Veri toplama ve izlenen bir sunucu için izleme devre dışı bırak
Bkz: [Azure AD Connect Health bir sunucuyu kaldırmak nasıl](active-directory-aadconnect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-all-monitored-services-in-azure-ad-connect-health"></a>Veri toplama ve Azure AD Connect Health, tüm izlenen hizmetler için izleme devre dışı bırak
Azure AD Connect Health veri koleksiyonunu durdurmak için bir seçenek de sağlar **tüm** Hizmetleri kiracıda kayıtlı. Dikkatli ve eyleme girişmeden önce tüm genel yöneticilere tam onaylanmasını öneririz. İşlemi başladıktan sonra Connect Health hizmeti almak, işlenmesi ve tüm hizmetlerinizin herhangi bir veri raporlama durdurur. Connect Health hizmetine mevcut veriler en fazla 30 gün boyunca saklanır.
Belirli sunucu veri koleksiyonunu durdurmak istiyorsanız, Lütfen belirli sunuculara silinmesini adımları izleyin. Tenant-Wise veri toplamayı durdurmak için veri toplama işlemini durdurun ve kiracının tüm hizmetleri silmek için aşağıdaki adımları izleyin.

1.  Tıklayarak **genel ayarlar** ana dikey yapılandırmasında altında. 
2.  Tıklayarak **veri toplamayı Durdur** dikey pencerenin üst kısmındaki düğmesi. İşlem başladıktan sonra Kiracı yapılandırma ayarlarının diğer seçenekleri devre dışı bırakılır.  
 
 ![Veri toplamayı Durdur](./media/active-directory-aadconnect-health-gdpr/gdpr4.png)
  
3.  Veri koleksiyonları durdurarak etkilenen eklenen hizmet listesini sağlayın. 
4.  Etkinleştirmek için tam Kiracı adını girin **Sil** eylem düğmesi
5.  Tıklayarak **Sil** tüm hizmetleri silinmesini tetiklemek için. Connect Health alma, işleme, eklenen hizmetlerinizden gönderilen tüm verileri raporlama durdurur. Tüm işlemi 24 saate kadar sürebilir. Bu adım, ters çevrilebilir olmadığına dikkat edin. 
6.  İşlem tamamlandıktan sonra kayıtlı tüm Connect Health Hizmetleri artık tarafından görülmez. 

 ![Sonra veri toplama durduruldu](./media/active-directory-aadconnect-health-gdpr/gdpr5.png)

## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Veri toplama ve Azure AD Connect Health izlemeyi yeniden etkinleştirin
Azure AD Connect Health daha önce silinmiş izlenen bir hizmet için izleme yeniden etkinleştirmek için öncelikle kaldırmanız ve [health aracısını yeniden](active-directory-aadconnect-health-agent-install.md) tüm sunucularda.

### <a name="re-enable-data-collection-and-monitoring-for-all-monitored-services"></a>Veri toplama ve tüm izlenen Hizmetleri için izlemeyi yeniden etkinleştirin

Azure AD Connect Health uygulamasındaki tenant-Wise veri koleksiyonu devam ettirilebilir. Dikkatli ve eyleme girişmeden önce tüm genel yöneticilere tam onaylanmasını öneririz.

>[!IMPORTANT]
> Eylem 24 saate kadar devre dışı bıraktıktan sonra aşağıdaki adımları kullanılabilir.
> Veri toplama etkinleştirildikten sonra sunulan içgörü ve Connect Health veri izleme önce toplanan herhangi bir eski veri gösterilmez. 

1.  Tıklayarak **genel ayarlar** ana dikey yapılandırmasında altında. 
2.  Tıklayarak **veri toplamayı etkinleştir** dikey pencerenin üst kısmındaki düğmesi. 
 
 ![Veri toplamayı etkinleştirme](./media/active-directory-aadconnect-health-gdpr/gdpr6.png)
 
3.  Etkinleştirmek için tam Kiracı adını girin **etkinleştirme** düğmesi.
4.  Tıklayarak **etkinleştirme** Connect Health hizmetine veri koleksiyonunun izni düğmesi. Değişiklik kısa bir süre içinde uygulanır. 
5.  İzleyin [yükleme işlemi](active-directory-aadconnect-health-agent-install.md) aracıyı izlenecek sunucuları ve hizmetleri yeniden yüklemek için portalda mevcut olacaktır.  


## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
* [Azure AD Connect ve kullanıcı gizliliği](../../active-directory/connect/active-directory-aadconnect-gdpr.md)

