---
title: Süre sonu için Office 365 grupları Azure Active Directory'de | Microsoft Docs
description: Azure Active Directory'de Office 365 grupları için sona erme kurma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 656577211a992432366033326362c9892f7ba6ef
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861903"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Office 365 grupları için süre sonu ilkesi yapılandırma

Office 365 grupları yaşam döngüsü için süre sonu ilkesi ayarı artık yönetebilirsiniz. Azure Active Directory'de (Azure AD), yalnızca Office 365 gruplarının süre sonu ilkesi ayarlayabilirsiniz. 

Bir grubu süresi dolacak şekilde ayarladıktan sonra:
-   Grubun sahipleri Grup süre sonu yaklaştığında yenilemek için bildirim
-   Değil yenilenene herhangi bir grubu silindi
-   Grup sahipleri veya yönetici tarafından 30 gün içinde silinmiş herhangi bir Office 365 grubu döndürülebilir

> [!NOTE]
> Yapılandırma ve Office 365 grupları için süre sonu ilkesi kullanma taraftan süre sonu ilkesinin uygulandığı grupların tüm üyeleri için Azure AD Premium lisansına sahip olmanız gerekir.

İndirme ve Azure AD PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure Active Directory PowerShell grafik 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Roller ve izinler
Yapılandırma ve sona erme Office 365 grupları için Azure AD'de kullanabilecek rolleri verilmiştir.

Rol | İzinler
-------- | --------
Genel yönetici veya kullanıcı hesabı yöneticisi | Oluşturma, okuma, güncelleştirme veya Office 365 grupları süre sonu ilkesi ayarlarını silin<br>Herhangi bir Office 365 grubu yenileyebilirsiniz
Kullanıcı | Oldukları bir Office 365 grubu yenileyebilirsiniz<br>Sahip bir Office 365 grubunu geri yükleyebilirsiniz<br>Süre sonu ilkesi ayarlarını okuyabilirsiniz

Silinen bir grubu geri yüklemek için izinler hakkında daha fazla bilgi için bkz. [Azure Active Directory silinen bir Office 365 grubunda geri](../fundamentals/active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Grup süre sonu Ayarla

1. Açık [Azure AD yönetim merkezini](https://aad.portal.azure.com) Azure AD kiracınızda genel yönetici olan bir hesapla.

2. Seçin **grupları**, ardından **sona erme** süre sonu ayarlarını açın.
  
  ![Sona erme dikey penceresi](./media/groups-lifecycle/expiration-settings.png)

4. Üzerinde **sona erme** dikey penceresinde şunları yapabilirsiniz:

  * Grup ömrü gün olarak ayarlayın. Bir hazır değer ya da özel bir değer (31 gün veya daha fazla olmalıdır) seçebilirsiniz. 
  * Bir grup sahibi olduğunda burada yenileme ve süre sonu bildirimleri gönderilmesi gereken bir e-posta adresi belirtin. 
  * Office 365 grupları sona seçin. Süre sonu için etkinleştirebilirsiniz **tüm** Office 365 grupları seçebilirsiniz yalnızca etkinleştirmek **seçili** veya Office 365 grupları seçin **hiçbiri** tüm grupları için sona erme devre dışı bırakmak için .
  * Seçerek işiniz bittiğinde, ayarlarınızı kaydetmek **Kaydet**.


Bunun gibi e-posta bildirimleri 30 gün, 15 gün ve 1 gün grubun sona erme tarihinden önce Office 365 grup sahiplerine gönderilir.

![Sona erme e-posta bildirimi](./media/groups-lifecycle/expiration-notification.png)

Gelen **yenileme grubu** bildirim e-posta, Grup sahipleri can doğrudan erişmek erişim Paneli'nde Grup Ayrıntıları sayfası. Burada, kullanıcıların ereceği zaman, en son, yenilendi, açıklamasını ve gruba yenileme özelliği gibi Grup hakkında daha fazla bilgi alabilirsiniz. Grup sahibi rahatça içerik ve etkinlik kendi gruplarındaki görüntüleyebilmesi için Grup Ayrıntıları sayfası artık ayrıca Office 365 grubu kaynaklarına bağlantılar içerir.

Bir grubu süresi dolduğunda, bir gün sonra sona erme tarihini grubu silindi. Bunun gibi bir e-posta bildirimi geçerlilik süresi ve Office 365 Grup sonraki silinmesini hakkında bilgilendiren Office 365 grup sahiplerine gönderilir.

![Grup silme e-posta bildirimi](./media/groups-lifecycle/deletion-notification.png)

Grup silme işlemi, 30 gün içinde seçerek geri yüklenebilir **grubu geri yükleme** ya da açıklandığı gibi PowerShell cmdlet'lerini kullanarak [Azure Active Directory silinen bir Office 365 grubunda geri](../fundamentals/active-directory-groups-restore-azure-portal.md).
    
Grubun, geri belgeleri, SharePoint siteleri veya diğer kalıcı nesneler içeriyorsa, tam olarak grubu ve içeriğini geri yüklemek için 24 saat sürebilir.

> [!NOTE]
> * İlk süre sonlarını ayarlama, zaman aşımı aralığından daha eski olan tüm gruplar dolmasına 30 gün için ayarlanır. Bir gün içinde ilk yenileme bildirim e-posta gönderilir. 
>   Örneğin, A grubunun 400 gün önce oluşturuldu ve sona erme aralığını 180 gün olarak ayarlanır. Sona erme tarihi geçerli olduğunda, sahibi, yeniler sürece gruba silinmeden önce 30 gün vardır.
> * Şu anda yalnızca bir süre sonu ilkesi, bir kiracının Office 365 grupları için yapılandırılabilir.
> * Dinamik bir grup silinir ve geri, yeni bir grup olarak görülen ve yeniden kuralına göre doldurulur. Bu işlem, 24 saate kadar sürebilir.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Office 365 Grup süre sonu bir posta kutusuyla yasal tutmada nasıl çalışır
Bir grubun süresi dolmadan 30 gün sonra silinir olduğunda ve grubun verilerini uygulamalardan Planner, siteleri gibi veya ekipler kalıcı olarak silinir ancak yasal tutmada grubu posta kutusu saklanır ve kalıcı olarak silinmez. Yönetici verileri getirmek için posta kutusunu geri Exchange cmdlet'lerini kullanabilirsiniz. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Office 365 Grup süre sonu bekletme ilkesi ile nasıl çalışır?
Bekletme İlkesi, güvenlik ve uyumluluk Merkezi'nde yoluyla yapılandırılır. Bir grubun süresi dolmadan ve silindiğinde, Office 365 grupları için bekletme ilkesini ayarladıysanız, Grup posta kutusunda Grup konuşmalarını ve Grup sitesindeki dosyaları bekletme kapsayıcısında belirli tanımlanan tutma gün sayısı için korunur ilke. Kullanıcılar, süre dolduktan sonra grubun ya da içeriğini göremezsiniz ancak e-bulma aracılığıyla site ve posta kutusu verileri kurtarabilir.

## <a name="powershell-examples"></a>PowerShell örnekleri
Kiracınızda Office 365 grupları için sona erme ayarları yapılandırmak için PowerShell cmdlet'lerini nasıl kullanabileceğinizi örnekleri şunlardır:

1. PowerShell v2.0 Önizleme Modülü (2.0.0.137) yükleyin ve PowerShell komut isteminde oturum açın:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. New-AzureADMSGroupLifecyclePolicy sona erme ayarları yapılandırın: Bu cmdlet ile 365 gün kiracıdaki tüm Office 365 grupları için yaşam süresi ayarlar. Yenileme bildirimleri için Office 365 grupları sahipleri gönderilecek olmadan 'emailaddress@contoso.com'
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Get-AzureADMSGroupLifecyclePolicy mevcut İlke Al: Bu cmdlet yapılandırılmış geçerli Office 365 Grup süre sonu ayarlarını alır. Bu örnekte, görebilirsiniz:
  * İlke kimliği 
  * Kiracıdaki tüm Office 365 grupları için yaşam süresi 365 gün olarak ayarlanır
  * Yenileme bildirimleri için Office 365 grupları sahipleri gönderilecek olmadan 'emailaddress@contoso.com.'
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Mevcut ilke kümesi-AzureADMSGroupLifecyclePolicy güncelleştirin: Bu cmdlet, var olan bir ilkeyi güncelleştirmek için kullanılır. Aşağıdaki örnekte mevcut ilkedeki Grup ömrü 365 gün 180 gün olarak değiştirildi. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Add-AzureADMSLifecyclePolicyGroup ilkeye belirli gruplara ekleyin: Bu cmdlet bir grup yaşam döngüsü ilkesine ekler. Örneğin: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Mevcut ilke Remove-AzureADMSGroupLifecyclePolicy Kaldır: Bu cmdlet, Office 365 grubu sona erme ayarları siler ancak ilke Kimliği gereklidir. Bu, Office 365 grupları için sona erme devre dışı bırakır. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
Daha ayrıntılı olarak ilkesini yapılandırmak için aşağıdaki cmdlet'leri kullanılabilir. Daha fazla bilgi için bkz: [PowerShell belgeleri](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* Yeni-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Ekle-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler, Azure AD grupları hakkında ek bilgi sağlar.

* [Var olan grupları bakın](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Grup üyelerini Yönet](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliğini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Gruptaki kullanıcılar için dinamik kurallarını yönetme](groups-dynamic-membership.md)
