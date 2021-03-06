---
title: Fiziksel sunucuya Azure Site recovery'de Azure'a çoğaltma mimarisi | Microsoft Docs
description: Bu makalede Azure Site Recovery hizmeti ile şirket içi fiziksel sunucuları Azure'a çoğaltırken kullanılan bileşenlere ve genel bir bakış sağlar.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: d4c0b3b8ef778a01c365d34734019d2fa11a2343
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920949"
---
# <a name="physical-server-to-azure-replication-architecture"></a>Fiziksel sunucuya Azure çoğaltması mimarisi

Bu makalede kullanılan çoğaltma, yük devretme ve bir şirket içi siteyle Azure arasındaki fiziksel Windows ve Linux sunucularını kurtarma işlemlerini ve mimarisini kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmeti.


## <a name="architectural-components"></a>Mimari bileşenler

Aşağıdaki tablo ve grafik azure'a fiziksel sunucu çoğaltma için kullanılan bileşenleri üst düzey bir görünümünü sağlar.  

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabını ve Azure ağı. | Şirket içi vm'lerden çoğaltılan veriler depolama hesabında depolanır. Yük devretme şirket içinden Azure'a çalıştırdığınızda çoğaltılan verilerle azure Vm'leri oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu** | Tek bir fiziksel makine şirket içinde veya tüm şirket içi Site Recovery bileşenlerini çalıştıran VMware VM dağıtılır. VM yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusunda çalışır. | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
 **İşlem sunucusu**:  | Varsayılan yapılandırma sunucusu ile birlikte yüklenir. | Çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> İşlem sunucusu, çoğaltmak istediğiniz sunucularda ayrıca Mobility hizmetini yükler.<br/><br/> Dağıtımınız büyüdükçe, daha büyük çoğaltma trafiği hacimlerini idare etmek ayrı, ek işlem sunucuları ekleyebilirsiniz.
 **Ana hedef sunucu** | Varsayılan yapılandırma sunucusu ile birlikte yüklenir. | Azure’dan yeniden çalışma sırasında çoğaltma verilerini işler.<br/><br/> Büyük dağıtımlar için yeniden çalışma için bir ek, ayrı bir ana hedef sunucusu ekleyebilirsiniz.
**Çoğaltılmış sunucuları** | Mobility hizmetinin Çoğalttığınız her sunucusuna yüklenir. | İşlem sunucusundan otomatik yüklemeye izin ver öneririz. Alternatif olarak hizmetini el ile yükleme veya bir otomatik dağıtım yöntemi gibi System Center Configuration Manager'ı kullanın.

**Fiziksel-Azure arası mimari**

![Bileşenler](./media/physical-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Çoğaltma işlemi

1. Şirket içi ve Azure bileşenlerini dahil olmak üzere dağıtım işlemini ayarladığınız. Kurtarma Hizmetleri Kasası'nda çoğaltma kaynağını ve hedef yapılandırma sunucusunu ayarlar, bir çoğaltma ilkesi oluşturma ve çoğaltmayı etkinleştirme belirtin.
2. Çoğaltma İlkesi ve bir başlangıç kopyasını sunucu verilerini makineler çoğaltma Azure depolamaya çoğaltılır.
3. Delta değişikliklerinin azure'a çoğaltılması ilk çoğaltma sonlandırıldıktan sonra başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
    - Makinelerin yapılandırma sunucusuna HTTPS 443 numaralı bağlantı noktasında gelen çoğaltma yönetimi için iletişim.
    - Makineleri Gönder çoğaltma verilerini HTTPS 9443 numaralı bağlantı noktası işlem sunucusunda gelen (değiştirilebilir).
    - Yapılandırma sunucusu, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma yönetimini düzenler.
    - İşlem sunucusu kaynak makinelerden gelen verileri alır, iyileştirip şifreler ve 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Bu özellik, makinelerin aynı iş yükünü çalıştırdığı ve tutarlı olmasının gerektiği durumlarda kullanışlıdır.
4. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute [genel eşliğini](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.


**Fizikselden Azure'a çoğaltma işlemi**

![Çoğaltma işlemi](./media/physical-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

Gerektiği şekilde çoğaltma ayarlanır ve her şeyin beklendiği gibi çalıştığını denetlemek için olağanüstü durum kurtarma tatbikatı (yük devretme testi) çalıştırdıktan sonra Yük devretme ve yeniden çalışma çalıştırabilirsiniz. Şunlara dikkat edin:

- Planlanan yük devretme desteklenmez.
- Geri bir şirket içi VMware VM'ye başarısız olmalıdır. Başka bir deyişle, hatta şirket içi fiziksel sunucuları Azure'a çoğaltırken, bir şirket içi VMware altyapısı gerekir.
- Tek bir makine üzerinden yük devredebilir veya birden çok makinede yük birlikte devretmek için kurtarma planları oluşturun.
- Bir yük devretme çalıştırdığınızda, Azure Vm'leri, Azure depolama alanında çoğaltılan verileri oluşturulur.
- İlk yük devretmeyi tetiklemeden sonra Azure VM üzerindeki iş yüküne erişmeye başlamak üzere yürütürsünüz.
- Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz.
- Yeniden çalışma altyapısı ayarlarsınız, gerek dahil olmak üzere:
    - **Azure'da geçici işlem sunucusu**: Azure'dan yeniden çalışma için bir Azure VM'yi azure'dan çoğaltmayı düzenlemek için bir işlem sunucusu olarak davranacak şekilde ayarlarsınız. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
    - **VPN bağlantısı**: yeniden çalışma için bir VPN bağlantısı (veya Azure ExpressRoute) Azure ağından şirket içi siteye gerekir.
    - **Ayrı bir ana hedef sunucusu**: varsayılan olarak, yapılandırma sunucusunda şirket içi VMware VM ile yüklenen ana hedef sunucusu yeniden çalışma işler. Bununla birlikte, geri büyük hacimli trafikte başarısız gerekiyorsa, bu amaç için ayrı şirket içi ana hedef sunucusu ayarlamanız ayarlamanız gerekir.
    - **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Şirket içinden Azure'a çoğaltma ilkenizi oluşturduğunuzda otomatik olarak oluşturuldu.
    - **VMware altyapısı**: yeniden çalışma için bir VMware altyapınız olmalıdır. Bir fiziksel sunucuda yeniden çalışamazsınız.
- Bileşenleri hazır olduktan sonra yeniden çalışma üç aşamada gerçekleşir:
    - 1. Aşama: Azure Vm'lerini yeniden koruma, böylece bunlar Azure'dan çoğaltmak şirket içi VMware Vm'lerini geri dönün.
    - 2. Aşama: şirket içi siteye yük devretme çalıştırın.
    - 3. Aşama: iş yüklerini geri başarısız olduktan sonra çoğaltmayı yeniden etkinleştirin.

**Azure'dan VMware**

![Yeniden çalışma](./media/physical-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Sonraki adımlar

İzleyin [Bu öğreticide](physical-azure-disaster-recovery.md) Azure'a çoğaltma fiziksel sunucu etkinleştirmek için.
