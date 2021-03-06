---
title: Azure Site Recovery yönetmek için rol tabanlı erişim denetimi kullanarak | Microsoft Docs
description: Bu makalede nasıl uygulanacağını ve Azure Site Recovery dağıtımlarınızı yönetmek için rol tabanlı erişim denetimi (RBAC) kullanın
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/06/2018
author: mayanknayar
ms.topic: conceptual
ms.author: manayar
ms.openlocfilehash: dfd880b6ff3a7e199ea259acc5e5ec59f89c897d
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919735"
---
# <a name="use-role-based-access-control-to-manage-site-recovery-access"></a>Site Recovery erişimi yönetmek için rol tabanlı erişim denetimi kullanma

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kendi sorumluluklarını ayırabilir ve belirli işlerini gerçekleştirmek için gereken şekilde kullanıcılara yalnızca belirli erişim izinleri verme.

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için 3 yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Site Recovery Katkıda Bulunanı](../role-based-access-control/built-in-roles.md#site-recovery-contributor) - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol etkinleştirebilen ve uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarma gibi durumda may olmak yönetme olağanüstü durum kurtarma yöneticileri için idealdir.
* [Site Recovery operatörü](../role-based-access-control/built-in-roles.md#site-recovery-operator) -bu rol, yürütme ve yük devretme ve yeniden çalışma işlemlerini yönetme izinlerine sahiptir. Bu role sahip bir kullanıcı etkinleştiremez veya çoğaltmayı devre dışı bırak, oluşturma veya kasa silme, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilirsiniz. Bu rol yük devretme sanal makinelerini bir olağanüstü durum kurtarma operatörü için idealdir veya uygulama sahipleri ve BT yöneticileri bir DR gibi bir gerçek veya sanal bir olağanüstü durumda tarafından istendiğinde uygulamaları detayına gidin. POST çözümleme olağanüstü DR operatörü yeniden koruma altına alabilir ve yeniden çalışma sanal makineler.
* [Site Recovery Okuyucusu](../role-based-access-control/built-in-roles.md#site-recovery-reader) - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol kullanan mevcut koruma durumunu izleyebilir ve gerekirse destek biletleri oluşturabilen BT izleme Yöneticisi için idealdir.

Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, bkz. nasıl [özel roller oluşturma](../role-based-access-control/custom-roles.md) azure'da.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Yeni sanal makineler için çoğaltmayı etkinleştirmek için gereken izinler
Yeni bir sanal makine Azure Site Recovery kullanılarak Azure'a çoğaltılırken, ilişkili kullanıcının erişim düzeyi kullanıcının Site Recovery için sağlanan Azure kaynakları için gerekli izinlere sahip olduğundan emin olmak doğrulanır.

Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için bir kullanıcı olması gerekir:
* Seçilen kaynak grubunda bir sanal makine oluşturma izni
* Seçilen sanal ağda bir sanal makine oluşturma izni
* Seçili depolama hesabına yazma izni

Bir kullanıcı yeni bir sanal makine çoğaltmayı tamamlamak için aşağıdaki izinler gerekiyor.

> [!IMPORTANT]
>İlgili izinler dağıtım modeli eklendiğinden emin olun (Resource Manager / Klasik) kaynak dağıtımı için kullanılır.

| **Kaynak Türü** | **Dağıtım modeli** | **İzni** |
| --- | --- | --- |
| İşlem | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klasik | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Ağ | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klasik | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Depolama | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klasik | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Kaynak Grubu | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

'Sanal makine Katılımcısı' ve 'Klasik sanal makine Katılımcısı' kullanmayı düşünün [yerleşik roller](../role-based-access-control/built-in-roles.md) için Resource Manager ve klasik dağıtım modelleri sırasıyla.

## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md): Azure portalında RBAC ile çalışmaya başlama.
* Erişim ile yönetmeyi öğrenin:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol tabanlı erişim denetimi sorunlarını giderme](../role-based-access-control/troubleshooting.md): yaygın sorunları çözme için öneriler alın.
