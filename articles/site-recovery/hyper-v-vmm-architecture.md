---
title: Hyper-V çoğaltması Azure Site recovery'de ikincil site arası mimari | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile ikincil System Center VMM sitesine şirket içi Hyper-V VM’lerini çoğaltma mimarisine genel bir bakış sunulmaktadır.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 0319e4d1cdbd20c39772c659a3e5a1580d8b925b
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919657"
---
# <a name="hyper-v-replication-to-a-secondary-site"></a>İkincil bir siteye Hyper-V çoğaltma

Bu makalede, Azure portalından [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak, System Center Virtual Machine Manager (VMM) bulutlarındaki Hyper-V sanal makinelerini (VM) ikincil bir VMM sitesine çoğaltırken kullanılan bileşenler ve işlemler açıklanmaktadır.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik bir ikincil siteye Hyper-V çoğaltma için kullanılan bileşenleri üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure aboneliği | VMM konumları arasında çoğaltmayı düzenleyip yönetmek için Azure aboneliğinde bir Kurtarma Hizmetleri kasası oluşturun.
**VMM sunucusu** | Birincil ve ikincil VMM konumu gerekir. | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir.
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusu. | Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Hyper-V VM’leri** | Hyper-V ana bilgisayar sunucusunda. | Kaynak ana bilgisayar sunucusunda çoğaltmak istediğiniz en az bir VM olması gerekir.

**Şirket içinden şirket içine mimarisi**

![Şirket içinden şirket içine](./media/hyper-v-vmm-architecture/arch-onprem-onprem.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) anlık görüntü alınır.
2. Birer birer çoğaltılır, ikincil konumda VM üzerindeki sanal sabit diskleri olan.
3. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse, Hyper-V çoğaltma çoğaltma İzleyicisi bu değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) izler. Bu günlük dosyaları disklerle aynı klasörde yer alır. Her diskin ikincil konuma gönderilir bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma tamamlandığında VM anlık görüntüsü silinir ve değişiklik çoğaltması başlar.
5. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

- Tek bir makine üzerinden yük devredebilir veya kurtarma planları, birden çok makinenin yük devretmelerini düzenlemek üzere oluşturun.
- Şirket içi siteler arasında planlanmış veya planlanmamış bir yük devretme çalıştırabilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
    - Sonra ikincil konumdaki makineler ikincil bir siteye planlanmamış bir yük devretme gerçekleştirirseniz korumalı değildir.
    - Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
- İlk yük devretme çalıştıktan sonra kopya VM'deki iş yüküne erişmeye başlamak için kaydedin.
- Birincil konum yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz.
    - İkincil siteden birincil siteye çoğaltmaya başlamak için çoğaltmayı tersine çevirme, başlatma. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
    - Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.



## <a name="next-steps"></a>Sonraki adımlar


İzleyin [Bu öğreticide](hyper-v-vmm-disaster-recovery.md) VMM bulutlarının arasında Hyper-V çoğaltma işlemini etkinleştirmek istiyorsanız.
