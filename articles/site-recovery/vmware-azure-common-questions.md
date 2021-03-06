---
title: Sık sorulan sorular - Vmware'den Azure Site Recovery ile Azure'a çoğaltma | Microsoft Docs
description: Şirket içi VMware Vm'lerini Azure Site Recovery kullanarak Azure'a çoğalttığınızda, bu makalede, sık sorulan sorular özetler.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.date: 07/19/2018
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: e8d30ae6cde7c787f1aa950506e0eb74bac0c12d
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238817"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Sık sorulan sorular - Vmware'den Azure'a çoğaltma

Bu makalede, şirket içi VMware Vm'lerini Azure'a çoğaltırken görüyoruz yaygın soruların yanıtları sağlanır. Bu makaleyi okuduktan sonra sorularınız varsa gönderin [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Genel
### <a name="how-is-site-recovery-priced"></a>Site Recovery nasıl fiyatlandırılır?
Gözden geçirme [Azure Site Recovery fiyatlandırma](https://azure.microsoft.com/en-in/pricing/details/site-recovery/) ayrıntıları.

### <a name="how-do-i-pay-for-azure-vms"></a>Azure Vm'leri için nasıl ödeme yapabilirim?
Çoğaltma sırasında veriler Azure depolama alanına çoğaltılır ve herhangi bir VM değişiklik ödeme yapmayın. Azure'a yük devretme çalıştırdığınızda, Site Recovery, Azure Iaas sanal makineleri otomatik olarak oluşturur. Ardından Azure'da kullandığınız işlem kaynakları için faturalandırılırsınız.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>VMware ile Azure'a çoğaltma için neler yapabilirim?
- **Olağanüstü durum kurtarma**: tam olağanüstü durum kurtarması da ayarlayabilirsiniz. Bu senaryoda, şirket içi VMware Vm'lerini Azure depolama alanına çoğaltın. Şirket içi altyapınızı kullanılamıyorsa, daha sonra Azure'a devredebilir. Yük devretme, çoğaltılan verileri kullanarak Azure Vm'leri oluşturulur. Şirket içi veri merkezinizi tekrar kullanılabilir hale gelene kadar uygulamaları ve iş yüklerini Azure vm'lerinde erişebilirsiniz. Ardından, Azure'dan şirket içi sitenize başarısız olabilir.
- **Geçiş**: şirket içi VMware Vm'leri Azure'a geçirmek için Site RECOVERY'yi kullanabilirsiniz. Bu senaryoda, şirket içi VMware Vm'lerini Azure depolama alanına çoğaltın. Ardından, şirket içinden Azure'a yük devretme. Yük devretme sonrasında kullanılabilir ve Azure vm'lerinde çalışan iş yükleri ve uygulamalar.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Azure'da ne yapmalıyım?
Bir Azure aboneliği, bir kurtarma Hizmetleri kasası, bir depolama hesabı ve sanal ağ gerekir. Kasa, depolama hesabı ve ağ aynı bölgede olması gerekir.

### <a name="what-azure-storage-account-do-i-need"></a>Hangi Azure depolama hesabı ihtiyacım var?
Bir LRS veya GRS depolama hesabı gerekir. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Premium depolama desteklenmiyor.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Hesabımdaki Azure sanal makineler oluşturmak için izinler gerekiyor mu?
Bir abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz çoğaltma izinleri sahip. Değilseniz, bir Azure VM kaynak grubu ve Site Recovery yapılandırırken belirttiğiniz sanal ağ oluşturmak için izinler ve seçili depolama hesabına yazma izni gerekir. [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>Şirket içi 

### <a name="what-do-i-need-on-premises"></a>Ne şirket içi gerekiyor?
Şirket içi Site Recovery bileşenleri, tek bir VMware VM'de yüklü gerekir. En az bir ESXi konağı ile de bir VMware altyapısı gerekir ve bir vCenter sunucusu önerilir. Ayrıca, çoğaltma için bir veya daha fazla VMware Vm'lerini gerekir. [Daha fazla bilgi edinin](vmware-azure-architecture.md) VMware-Azure arası mimari hakkında.

Şirket içi yapılandırma sunucusu aşağıdaki iki yöntemden biri ile dağıtılabilir

1. Yapılandırma sunucusu önceden yüklü bir VM şablonu kullanarak dağıtın. [Buradan daha fazla okuma](vmware-azure-tutorial.md#download-the-vm-template).
2. Kurulum, tercih ettiğiniz bir Windows Server 2016 makinesinde kullanarak dağıtın. [Buradan daha fazla okuma](physical-azure-disaster-recovery.md#set-up-the-source-environment).

Başlarken, kendi Windows Server makinelerinde koruma amacı, koruma etkinleştirme, yapılandırma sunucusunu dağıtma adımları keşfetmek için seçin **Azure'a > sanallaştırılmamış/diğer**.

### <a name="where-do-on-premises-vms-replicate-to"></a>Burada şirket içi Vm'leri için çoğaltma?
Verileri Azure depolama alanına çoğaltır. Bir yük devretme çalıştırdığınızda, Site Recovery depolama hesabından Azure Vm'leri otomatik olarak oluşturur.

### <a name="what-apps-can-i-replicate"></a>Hangi uygulamaların çoğaltabilirim?
Herhangi bir uygulamayı veya ile uyumlu bir VMware VM'de çalışan iş yüklerini çoğaltabilirsiniz [çoğaltma gereksinimlerini](vmware-physical-azure-support-matrix.md##replicated-machines). Site kurtarma uygulamayla tutarlı çoğaltma için destek sağlar, böylece uygulamalar üzerinde başarısız oldu ve akıllı bir duruma başarısız oldu. Site Recovery, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın bir tümleştirmede çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Siteden siteye VPN ile azure'a çoğaltabilir miyim?
Site kurtarma verileri, şirket içinden genel bir uç nokta veya ExpressRoute genel eşlemesi kullanarak Azure depolama alanına çoğaltır. Siteden siteye VPN ağ üzerinden çoğaltma desteklenmez.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>ExpressRoute kullanarak azure'a çoğaltabilir miyim?
Evet, ExpressRoute Vm'lerini Azure'a çoğaltma için kullanılabilir. Site Recovery genel bir uç nokta bir Azure depolama hesabına veri çoğaltır ve kurmanız gerekecektir [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) Site Recovery çoğaltması için. Bir Azure sanal ağı için sanal makineleri yük devretme sonra erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>VPN üzerinden neden çoğaltma yapamaz?

Azure'a çoğalttığınızda, çoğaltma trafiği ortak uç noktalar Azure depolama hesabının ulaştığında, bu nedenle, yalnızca ExpressRoute (genel eşdüzey hizmet sağlama) ile genel internet üzerinden çoğaltma yapabilirsiniz ve VPN çalışmaz. 



## <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılmış sanal makine gereksinimleri nelerdir?

Çoğaltma için bir VMware VM, desteklenen bir işletim sistemi çalıştırmalıdır. Ayrıca, VM, Azure Vm'leri için gereksinimleri karşılaması gerekir. [Daha fazla bilgi edinin](vmware-physical-azure-support-matrix.md##replicated-machines) destek matrisi içinde.

## <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?
VMware Vm'lerini Azure'a çoğaltırken çoğaltma sürekli olarak yapılır.

## <a name="can-i-extend-replication"></a>Ben çoğaltma uzatabilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Bu özelliği isteği [geri bildirim Forumu](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

## <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilirim?
Bu özellik desteklenmez. Bu özelliği isteği [geri bildirim Forumu](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Diskleri çoğaltmanın dışında tutabilirsiniz?
Evet, diskleri çoğaltmadan hariç tutabilirsiniz. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Sanal makineleri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler çoğaltılabilir. İşletim sistemi diskinin bir temel disk olması gerekir.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Çoklu VM tutarlılığı için çoğaltma gruplarını kullanıyorsanız, var olan bir çoğaltma grubuna yeni bir sanal makine ekleyebilirim?
Evet, bunlar için çoğaltmayı etkinleştirdiğinizde, mevcut bir çoğaltma grubuna yeni VM'ler ekleyebilirsiniz. Çoğaltma başlatılır ve var olan VM'ler için bir çoğaltma grubu oluşturamazsınız sonra mevcut bir çoğaltma grubuna VM ekleyemezsiniz.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Ekleme veya diskleri yeniden boyutlandırma çoğaltılan VM'ler değiştirebiliyorum?

Azure'a VMware çoğaltma için disk boyutunu değiştirebilirsiniz. Yeni diskler eklemek istiyorsanız disk ekleyin ve VM için korumayı etkinleştirmeniz gerekir.

## <a name="configuration-server"></a>Yapılandırma sunucusu

### <a name="what-does-the-configuration-server-do"></a>Yapılandırma sunucusu ne yapar?
Yapılandırma sunucusu da dahil olmak üzere, Site Recovery bileşenlerini şirket içi çalıştırır: 
- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Bu çoğaltma verilerini alıp; Bu, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir; ve Azure depolama., işlem sunucusu ayrıca Mobility hizmetini şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir ve çoğaltmak istediğiniz Vm'lere yükler gönderir.
- Ana hedef sunucu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

### <a name="where-do-i-set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarladıktan nereden ayarlayabilirim?
İçin yapılandırma sunucusunu tek yüksek oranda kullanılabilir şirket içi VMware VM ihtiyacınız var.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Yapılandırma sunucusu için gereksinimler nelerdir?

Gözden geçirme [önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>El ile bir şablonu kullanmak yerine yapılandırma sunucusunu ayarlayabilirim?
OVF şablonu için en son sürümünü kullanmanızı öneririz [yapılandırma sunucusu VM'si oluşturma](vmware-azure-deploy-configuration-server.md). Şunları yapamazsınız herhangi bir nedenle Örneğin, VMware sunucusuna erişimi yoksa, şunları yapabilirsiniz [birleşik kurulum dosyasını indirirsiniz](physical-azure-set-up-source.md) Portal'dan ve bir sanal makine üzerinde çalıştırın. 

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Yapılandırma sunucusu, birden fazla bölgeye çoğaltabilir miyim?
Hayır. Bunu yapmak için her bölgede bir yapılandırma sunucusu ayarlamanız gerekir.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Azure'da bir yapılandırma sunucusu ana bilgisayar?
Olası, şirket içi VMware altyapınızı ve Vm'leri ile iletişim kurmak yapılandırma sunucusunu çalıştıran Azure VM gerekir. Ek yükü, büyük olasılıkla uygun değildir.


### <a name="where-can-i-get-the-latest-version-of-the-configuration-server-template"></a>Yapılandırma sunucusu şablonunun en son sürümünü nereden alabilirim?
En son sürümü [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

### <a name="how-do-i-update-the-configuration-server"></a>Yapılandırma sunucusu nasıl güncelleştirebilirim?
Güncelleştirme paketlerini yükleyin. En son güncelleştirme bilgileri bulabilirsiniz [wiki güncelleştirmeleri sayfası](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

## <a name="mobility-service"></a>Mobility hizmeti

### <a name="where-can-i-find-the-mobility-service-installers"></a>Mobility hizmeti yükleyicileri nerede bulabilirim?
Yükleyicileri tutulur **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** yapılandırma sunucusundaki klasör.

## <a name="how-do-i-install-the-mobility-service"></a>Mobility hizmeti nasıl yüklerim?
Çoğaltmak istediğiniz her sanal makinede kullanarak yüklediğiniz bir [gönderme yüklemesi](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery), veya el ile yükleme [UI](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui), veya [PowerShell kullanarak](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt). Alternatif olarak, bir dağıtım aracı gibi kullanarak dağıtabileceğiniz [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md), veya [Azure Otomasyonu ve DSC](vmware-azure-mobility-deploy-automation-dsc.md).



## <a name="security"></a>Güvenlik

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Hangi erişim Site Recovery, VMware sunucuları için ihtiyaç duyuyor mu?
Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Yapılandırma sunucusunu ve diğer şirket içi Site Recovery bileşenlerini çalıştıran bir VMware VM ayarlayın. [Daha fazla bilgi](vmware-azure-deploy-configuration-server.md)
- Otomatik olarak VM için çoğaltma keşfedin. Otomatik bulma için bir hesap hazırlama hakkında bilgi edinin. [Daha fazla bilgi](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Hangi erişim Site Recovery, VMware Vm'lerinde gerekiyor mu?

- Bir VMware VM çoğaltmak için Site Recovery Mobility hizmeti yüklü ve çalışıyor olması gerekir. Aracı el ile dağıtmak veya bir sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery hizmeti göndererek yükleme yapmalısınız belirtin. Gönderim yüklemesi için Site Recovery hizmet bileşeni yüklemek için kullanabileceği bir hesap gerekir. [Daha fazla bilgi edinin](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). (Varsayılan olarak yapılandırma sunucusunda çalışan) işlem sunucusu, bu yükleme gerçekleştirir. [Daha fazla bilgi edinin](vmware-azure-install-mobility-service.md) Mobility hizmeti yüklemesi hakkında.
- Çoğaltma sırasında VM'ler gibi Site Recovery ile iletişim:
    - VM'ler, çoğaltma yönetimi için HTTPS 443 numaralı bağlantı noktasındaki yapılandırma sunucusuyla iletişim kurar.
    - Vm'leri çoğaltma verilerini HTTPS 9443 gelen bağlantı (değiştirilebilir) işlem sunucusu gönderin.
    - Çoklu VM tutarlılığını etkinleştirirseniz, VM'ler birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar.


### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verilerini Site Recovery hizmetine gönderilir?
Hayır, Site Recovery çoğaltılan verilere müdahale etmez ve sanal makinelerinizde çalışan ne hakkında herhangi bir bilgi yoktur. Çoğaltma verilerini, VMware hiper yöneticileri ve Azure depolama değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Recovery, ISO 27001: 2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ile FedRAMP JAB değerlendirmelerini sürecinde olduğundan.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Biz şirket içi meta verileri bir coğrafi bölge içinde tutabilir miyim?
Evet. Bir bölgede bir kasası oluşturduğunuzda, tüm meta verilerini o bölgenin coğrafi sınırları içinde kalır Site Recovery tarafından kullanılan emin olun.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, hem şifreleme-aktarım sırasında ve [azure'da şifreleme](https://docs.microsoft.com/azure/storage/storage-service-encryption) desteklenir.


## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma
### <a name="how-far-back-can-i-recover"></a>Ne kadar geri kurtarma gerçekleştirebilir miyim?
VMware-Azure arası için kullanabileceğiniz en eski kurtarma noktası değer 72 saattir.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Yük devretme sonrasında Azure Vm'lerini nasıl erişebilirim?
Yük devretme sonrasında Azure Vm'lerini güvenli bir Internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için etmenizi hazırlamanız gerekir. [Daha fazla bilgi](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Dayanıklı veriler üzerinde başarısız oldu?
Azure esneklik için tasarlanmıştır. Site kurtarma, ikincil bir Azure veri merkezi, Azure SLA'sı uygun olarak yük devretme için tasarlanmıştır. Yük devretme işlemi gerçekleştiğinde, kasanız için seçtiğiniz aynı coğrafi bölgede kasaları kalır ve biz meta verilerinizi emin olun.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
[Yük devretme](site-recovery-failover.md) otomatik değildir. Portaldaki tek tıklamayla yük devretme başlatın veya kullanabileceğiniz [ PowerShell](/powershell/module/azurerm.siterecovery) bir yük devretmeyi tetiklemek için.

### <a name="can-i-fail-back-to-a-different-location"></a>Ben, farklı bir konuma başarısız olabilir?
Evet, Azure'a yük devretmesi, ilkinin kullanılamıyorsa farklı bir konuma başarısız olabilir. [Daha fazla bilgi edinin](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Bir VPN veya ExpressRoute geri başarısız olmasına neden ihtiyacım var?

Azure'dan yeniden çalışma, verileri azure'dan şirket içi Makinenize geri kopyalanır ve özel erişim gereklidir. 



## <a name="automation-and-scripting"></a>Otomasyon ve betik oluşturma

### <a name="can-i-set-up-replication-with-scripting"></a>Komut dosyası ile çoğaltma ayarlayabilir miyim?
Evet. Rest API, PowerShell veya Azure SDK'sını kullanarak Site Recovery iş akışlarını otomatik hale getirebilirsiniz. [Daha fazla bilgi edinin](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Performans ve kapasite
### <a name="can-i-throttle-replication-bandwidth"></a>Çoğaltma bant genişliği kullanımını azaltmak?
Evet. [Daha fazla bilgi edinin](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Sonraki adımlar
* [Gözden geçirme](vmware-physical-azure-support-matrix.md) gereksinimlerini destekler.
* [Ayarlanan](vmware-azure-tutorial.md) Vmware'den Azure'a çoğaltma.
