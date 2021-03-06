---
title: Şirket içinden Azure'a çoğaltma için ağ arabirimlerini Azure Site recovery'de yönetme | Microsoft Docs
description: Şirket içi Azure Site Recovery ile Azure'a çoğaltma için ağ arabirimleri yönetme işlemi açıklanır
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 75220d964f3e1208c85d34c1da97fab32044e62b
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917117"
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-replication"></a>Şirket içinden Azure'a çoğaltma için sanal makine ağ arabirimleri yönetme

Azure sanal makineler'de (VM) bağlı en az bir ağ arabirimi olması gerekir. Birçok ağ arabirimleri VM boyutu destekler bağlı olarak olabilir.

Varsayılan olarak, bir Azure sanal makinesine bağlı ilk ağ arabiriminin birincil ağ arabirimi olarak tanımlanır. Sanal makinedeki diğer tüm ağ arabirimleri, ikincil ağ arabirimlerine ' dir. Ayrıca varsayılan olarak, sanal makineden giden tüm trafiği birincil ağ arabiriminin birincil IP yapılandırması için atanan IP adresi kullanıma gönderilir.

Bir şirket içi ortamda sanal makineler veya sunucuları farklı ağ ortamında birden çok ağ arabirimine sahip olabilir. Farklı ağlarda genellikle, yükseltme ve Bakım internet erişimi gibi belirli işlemlerin gerçekleştirilmesi için kullanılır. Geçiş ya da Azure'a bir şirket içi ortama yük devretmeyi aynı sanal makine ağ arabirimleri tüm aynı sanal ağa bağlanması gereken aklınızda bulundurun.

Varsayılan olarak, Azure Site Recovery, birçok şirket içi sunucunun bağlı olarak bir Azure sanal makinesinde arabirimleri ağ olarak oluşturur. Çoğaltılmış sanal makine ayarları altında ağ arabirimi ayarlarını düzenleyerek geçişi veya yük devretme sırasında yedekli ağ arabirimlerini oluşturma önleyebilirsiniz.

## <a name="select-the-target-network"></a>Hedef ağ seçin

VMware ve fiziksel makineler için ve Hyper-V (olmadan System Center Virtual Machine Manager) sanal makineler için tek tek sanal makineler için hedef sanal ağ da belirtebilirsiniz. Virtual Machine Manager ile yönetilen Hyper-V sanal makinelerini için [ağ eşlemesini](site-recovery-network-mapping.md) kaynak Virtual Machine Manager sunucusundaki VM ağlarını eşlemek ve hedef Azure ağları.

1. Altında **çoğaltılan öğeler** bir kurtarma Hizmetleri Kasası'nda çoğaltılan bu öğe için ayarlara erişmek için herhangi bir çoğaltılan öğeyi seçin.

2. Seçin **işlem ve ağ** çoğaltılan öğe için ağ ayarlarını erişmek için sekmesinde.

3. Altında **ağ özellikleri**, bulunan ağ arabirimleri listesinden bir sanal ağ seçin.

    ![Ağ ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Hedef ağ değiştirme, tüm ağ arabirimleri, belirli bir sanal makine için etkiler.

Virtual Machine Manager Bulutları için ağ eşlemesini değiştirmek için tüm sanal makineleri ve ağ arabirimlerini etkiler.

## <a name="select-the-target-interface-type"></a>Hedef arabirim türü seçin

Altında **ağ arabirimleri** bölümünü **işlem ve ağ** bölmesinde, görüntülemek ve ağ arabirimi ayarları düzenleyin. Hedef ağ arabirim türü de belirtebilirsiniz.

- A **birincil** ağ arabirimi yük devretme için gereklidir.
- Seçilen diğer tüm ağ arabirimleri, varsa **ikincil** ağ arabirimleri.
- Seçin **kullanmayın** oluşturma yük devretme sırasında bir ağ arabirimi dışlanacak.

Çoğaltmayı etkinleştirirken varsayılan olarak, Site Recovery tüm algılanan ağ arabirimleri üzerinde şirket içi sunucunun seçer. Bir olarak işaretler **birincil** ve tüm diğer olarak **ikincil**. Şirket içi sunucuya eklendi sonraki arabirimlerden işaretlenmiş **kullanmayın** varsayılan olarak. Daha fazla ağ arabirimi eklerken, doğru Azure sanal makine hedef boyutu gerekli ağ arabirimlerinin uyum sağlamak için seçildiğinden emin olun.

## <a name="modify-network-interface-settings"></a>Ağ arabirimi ayarlarını değiştirme

Çoğaltılan öğenin ağ arabirimleri için IP adresi ve alt ağ değiştirebilirsiniz. Bir IP adresi belirtilmezse, Site Recovery ağ arabirimi yük devretme sırasında alt ağdan sonraki kullanılabilir IP adresi atar.

1. Ağ arabirimi ayarlarını açmak için herhangi bir mevcut ağ arabirimi seçin.

2. İstenen alt ağ, kullanılabilir alt ağlar listesinden seçin.

3. İstenen IP adresini (gerektiği gibi) girin.

    ![Ağ arabirimi ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Seçin **Tamam** düzenlenmesini tamamlamayı ve dönmek için **işlem ve ağ** bölmesi.

5. Diğer ağ arabirimleri için 1-4 arası adımları yineleyin.

6. Seçin **Kaydet** tüm değişiklikleri kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
  [Daha fazla bilgi edinin](../virtual-network/virtual-network-network-interface-vm.md) Azure sanal makineleri için ağ arabirimleri hakkında.
