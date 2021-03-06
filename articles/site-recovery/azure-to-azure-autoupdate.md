---
title: Azure'dan Azure'a olağanüstü durum kurtarma mobilite hizmetinin otomatik güncelleştirmesi | Microsoft Docs
description: Azure Site Recovery kullanarak Azure Vm'lerine çoğaltırken Mobility hizmeti, otomatik güncelleştirme genel bir bakış sağlar.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: 81dbb61d696da84febc89563f946581315fdf527
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921440"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Azure'dan Azure'a çoğaltma Mobility hizmetini otomatik güncelleştirme

Azure Site Recovery, burada mevcut özelliklere yönelik iyileştirmeler veya yenilerini eklenir ve bilinen sorunlar varsa sabit bir aylık bir yayın temposudur sahiptir. Bu hizmetle geçerli kalmasını sağlamak için aylık bu düzeltme eklerinin dağıtımını için planlama gerektiğini anlamına gelir. Yükseltmeye ilişkili üzerinden baş önlemek için kullanıcıların bunun yerine güncelleştirmeleri bileşenlerini yönetmek Site Recovery izin vermeyi seçebilirsiniz. Ayrıntılarıyla açıklandığı gibi [mimari başvurusu](azure-to-azure-architecture.md) için çoğaltma etkin olan bir Azure sanal makineler çoğaltılırken tüm Azure sanal makinelerde Azure'dan Azure'a olağanüstü durum kurtarma için Mobility hizmeti yüklü başka bir bölge. Mobility hizmeti uzantısı her yeni sürümle birlikte, otomatik güncelleştirme etkinleştirdikten sonra güncelleştirilir. Bu belge aşağıdaki ayrıntıları:

- Otomatik Güncelleştirme nasıl çalışır?
- Otomatik güncelleştirmeleri etkinleştir
- Sık karşılaşılan sorunlar ve sorun giderme
 
## <a name="how-does-automatic-update-work"></a>Otomatik Güncelleştirme nasıl çalışır

Güncelleştirmelerini yönetmek Site Recovery izin sonra (Azure Hizmetleri tarafından kullanılır) bir genel runbook kasa ile aynı abonelikte oluşturulan bir Otomasyon hesabı aracılığıyla dağıtılır. Bir Otomasyon hesabı, belirli bir kasa için kullanılır. Runbook, her VM için otomatik güncelleştirmeleri üzerinde etkin bir kasada denetler ve varsa yeni bir sürüme yükseltme Mobility hizmeti uzantısı başlatır. Runbook recurrs günlük olarak saat 12: 00'da çoğaltılan sanal makinenin coğrafi saat dilimine göre varsayılan zamanlaması. Runbook zamanlama ayrıca Otomasyon hesabı kullanıcı tarafından gerekirse değiştirilebilir. 

> [!NOTE]
> Otomatik güncelleştirmeleri etkinleştirme, Azure sanal makinelerinizin yeniden başlatılmasını gerektirmez ve devam eden çoğaltma etkilemez.

> [!NOTE]
> Otomasyon hesabı tarafından kullanılan işleri için iş çalıştırma zamanı dahil edilen ücretsiz birimler için bir Otomasyon hesabı olarak ay ve varsayılan olarak 500 dakika kullanılan dakika sayısını göre faturalandırılır. İşin günlük tutarlardan yürütülmesi bir **hakkında bir dakika için birkaç saniye** ve **ücretsiz krediler kapsamdaki**.

Ücretsiz BİRİMLER (aylık) dahil ** fiyat iş çalıştırma zamanı 500 dakika ₹0.14 / dakika

## <a name="enable-automatic-updates"></a>Otomatik güncelleştirmeleri etkinleştir

Site Recovery, aşağıdaki yollarla güncelleştirmeleri yönetmek izin vermeyi seçebilirsiniz:-

- [Etkinleştirme çoğaltma adımının bir parçası](#as-part-of-the-enable-replication-step)
- [İki durumlu uzantısı kasa içinde ayarlarını güncelleştirme](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Etkinleştirme çoğaltma adımının bir parçası:

Etkinleştirdiğinizde çoğaltma bir sanal makine için başlangıç ya da [sanal makine görünümünden](azure-to-azure-quickstart.md), veya [kurtarma Hizmetleri kasasından](azure-to-azure-how-to-enable-replication.md), Site Recovery ya da izin vermeyi seçtiğiniz seçeneği alırsınız Site Recovery uzantısı için veya aynı el ile yönetmek için güncelleştirmeleri yönetme.

![Çoğaltma otomatik güncelleştirme etkinleştir](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>İki durumlu uzantısı kasa içinde ayarlarını güncelleştirme

1. Kasa içinde gidin **Yönet**-> **Site Recovery altyapısı**
2. Altında **Azure sanal makineleri**-> **uzantı güncelleştirme ayarları**, izin vermek isteyip istemediğinizi seçmek için Değiştir'i tıklatın *güncelleştirmeleri yönetmek için ASR* veya *el ile yönetmeniz*. **Kaydet**’e tıklayın.

![iki durumlu autuo güncelleştirme kasası](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Seçeneğini belirlediğinizde *ASR yönetmek için izin*, tüm sanal makinelere karşılık gelen kasayı ayarı uygulanır.


> [!Note] 
> Her iki seçenek, güncelleştirmeleri yönetmek için kullanılan Otomasyon hesabının bildirir. İlk kez bir kasada bu özelliği etkinleştirmek, yeni bir Otomasyon hesabı oluşturulur. Tüm sonraki etkinleştir çoğaltmalar aynı kasaya daha önce oluşturulmuş bir kullanır.

## <a name="common-issues--troubleshooting"></a>Sık karşılaşılan sorunlar ve sorun giderme

Otomatik Güncelleştirmeler ile'ilgili bir sorun varsa, aynı 'Yapılandırma sorunlarını' kasa panosunda altında bildirilir. 

Otomatik güncelleştirmeleri etkinleştir çalışıldı ve başarısız durumunda, aşağıdaki sorun giderme için bkz.

**Hata**: bir Azure farklı çalıştır hesabı (hizmet sorumlusu) oluşturma ve hizmet sorumlusuna katkıda bulunan rolü verme izniniz yok. 
- Önerilen eylem: oturum açmış hesabın 'Katkıda bulunan' atandığından emin olun ve işlemi yeniden deneyin. Başvurmak [bu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) belge doğru izinleri atama hakkında daha fazla bilgi için.
 
Otomatik Güncelleştirmeler üzerinde etkin bir kez sorunların çoğu Site Recovery hizmeti tarafından taşınarak ve tıklayarak gerektirir '**onarım**' düğmesi.

![Onarma düğmesi](./media/azure-to-azure-autoupdate/repair.png)

Onarma düğmesi kullanılamaz durumda uzantı ayarları bölmesi altında görüntülenen hata iletisini inceleyin.

 - **Hata**: Farklı Çalıştır hesabının kurtarma Hizmetleri kaynağına erişim izni yok.

    **Önerilen eylem**: silin ve ardından [farklı çalıştır hesabını yeniden oluşturma](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account) veya Otomasyon farklı çalıştır hesabı Azure Active Directory uygulamasının kurtarma Hizmetleri kaynağına erişimi olduğundan emin olun.

- **Hata**: Farklı Çalıştır hesabı bulunamadı. Ya da bunlardan biri silinmiş veya oluşturulmamış: Azure Active Directory uygulaması, hizmet sorumlusu, rol, Otomasyon sertifikası varlığı, Otomasyon bağlantısı varlığı; ya da parmak izi sertifika ve bağlantı arasındaki aynı değil. 

    **Önerilen eylem**: silme ve [sonra farklı çalıştır hesabını yeniden oluşturun](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account).
