---
title: Azure'dan Azure'a çoğaltma için azure Site Recovery destek matrisi | Microsoft Docs
description: Desteklenen işletim sistemleri ve yapılandırmalar Azure sanal makineleri (VM'ler), Azure Site Recovery çoğaltması için bir bölgeden diğerine için olağanüstü durum kurtarma (DR) gereksinimlerini özetler.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.author: sujayt
ms.openlocfilehash: c2892d51c6eb5e71c0b1af400b78e993742fede0
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173059"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Bir Azure bölgesinden diğerine çoğaltma için destek matrisi



Bu makalede, desteklenen yapılandırmalar ve çoğalttığınızda bileşenleri ve Azure sanal makineleri bir bölgesinden başka bir bölgeye kurtarma özetler kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmeti.

## <a name="user-interface-options"></a>Kullanıcı arabirimi seçenekleri

**Kullanıcı arabirimi** |  **Desteklenen / desteklenmeyen**
--- | ---
**Azure portal** | Desteklenen
**PowerShell** | [PowerShell ile azure'dan Azure'a çoğaltma](azure-to-azure-powershell.md)
**REST API** | Şu anda desteklenmiyor
**CLI** | Şu anda desteklenmiyor


## <a name="resource-support"></a>Kaynak desteği

**Kaynak taşıma türü** | **Ayrıntılar** 
--- | --- | ---
**Kasa kaynak grupları arasında taşıma** | Desteklenmiyor<br/><br/> Kurtarma Hizmetleri kasası kaynak grupları arasında taşıyamazsınız.
**İşlem/depolama/ağ kaynakları kaynak grupları arasında taşıma** | Desteklenmiyor.<br/><br/> Bir VM veya depolama/ağ gibi ilişkili bileşenleri taşırsanız, bu çoğaltma sonra çoğaltmayı devre dışı bırakın ve sanal makine için çoğaltmayı etkinleştirmeniz gerekir.
**İçin olağanüstü durum kurtarma için başka bir aboneliği Azure Vm'lerini çoğaltma** | Desteklenmiyor.
**Abonelikler arasında sanal makineleri geçirme** | Desteklenmiyor.
**Aynı bölge içinde sanal makineleri geçirme** | Desteklenmiyor.


## <a name="support-for-replicated-machine-os-versions"></a>Çoğaltılan makinelerin işletim sistemi sürümleri için destek

Destek sözü edilen işletim sistemi çalıştıran tüm iş yükleri için geçerlidir.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Sunucu Çekirdeği, masaüstü deneyimi ile sunucu) *
- Windows Server 2012 R2
- Windows Server 2012
- Itanium tabanlı sistemler için Windows Server 2008 R2 ile en az SP1

>[!NOTE]
>
> \* Windows Server 2016 Nano Server desteklenmiyor.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6,8, 6.9, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5   
- CentOS 6.5, 6.6, 6.7, 6,8, 6.9, 7.0, 7.1, 7.2, 7.3,7.4, 7.5
- Ubuntu 14.04 LTS Server [ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (çekirdek sürümleri desteklenir)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7 [ (çekirdek sürümleri desteklenir)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- Debian 8 [ (çekirdek sürümleri desteklenir)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 12 SP1, SP2 SP3 [ (çekirdek sürümleri desteklenir)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4
- Oracle Enterprise Linux 6.4, 6.5 Red Hat uyumlu çekirdek veya kesilemeyen Enterprise çekirdeği sürüm 3 (UEK3) yüklü

(Makineleri için SLES 11 SP4 SLES 11 SP3 çoğaltma, yükseltme desteklenmez. Çoğaltılmış bir makineden SLES 11 SP4 ' SLES 11SP3 yükseltildiyse, çoğaltmayı devre dışı bırakıp makine yükseltme sonrası yeniden korumak için ihtiyacınız.)

>[!NOTE]
>
> Parola tabanlı kimlik doğrulaması ve oturum açma ve bulut sanal makineleri yapılandırmak için cloud-init'i paket kullanarak Ubuntu sunucuları parola tabanlı oturum açma (yapılandırmasına bağlı olarak cloudinit.) yük devretme sonrasında devre dışı olabilir. Parola tabanlı oturum açma olabilir sanal makinede yeniden etkin Ayarlar menüsünden parolayı sıfırlayarak (Destek + sorun giderme altında bölümünde) başarısız, Azure portalında sanal makine üzerinde.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen bir Ubuntu çekirdek sürümleri

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.18 | 3.13.0-24-Generic 3.13.0-151-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-128-generic için |
14.04 LTS | 9.17 | 3.13.0-24-Generic 3.13.0-147-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-124-generic için |
14.04 LTS | 9.16 | 3.13.0-24-Generic 3.13.0-144-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-119-generic için |
14.04 LTS | 9.15 | 3.13.0-24-Generic 3.13.0-143-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-116-generic için |
|||
16.04 LTS | 9.18 | 4.4.0-21-Generic 4.4.0-128-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için |
16.04 LTS | 9.17 | 4.4.0-21-Generic 4.4.0-124-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-41-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1016-azure için |
16.04 LTS | 9.16 | 4.4.0-21-Generic 4.4.0-119-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-38-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1012-azure için |
16.04 LTS | 9.15 | 4.4.0-21-Generic 4.4.0-116-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-37-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1012-azure için |


### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen Debian çekirdek sürümleri

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | 9.17,9.18 | 3.2.0-4-AMD64 3.2.0-6-amd64 için 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.15, 9.16 | 3.2.0-4-AMD64 3.2.0-5-amd64 için 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.17, 9.18 | 3.16.0-4-AMD64 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.6-amd64 için için |
Debian 8 | 9.15, 9.16 | 3.16.0-4-AMD64 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.5-amd64 için için |

### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen bir SUSE Linux Enterprise Server 12 çekirdek sürümleri

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.18 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.93-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.80-default için</br></br>SP3 4.4.73-5-default 4.4.138-94.39-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.17 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.88-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default</br></br>SP3 4.4.73-5-default 4.4.126-94.22-default için |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Desteklenen dosya sistemlerinin hem de Linux işletim sistemini çalıştıran Azure sanal makinelerinde Konuk depolama yapılandırmaları

* Dosya sistemleri: ext3, ext4, ReiserFS (Suse Linux Enterprise Server yalnızca), XFS
* Birim Yöneticisi: LVM2
* Çok yollu yazılım: cihaz Eşleyici

## <a name="region-support"></a>Bölge desteği

Çoğaltma ve aynı coğrafi kümedeki herhangi iki bölgeleri arasında Vm'lere'ı kurtarın.

**Coğrafi küme** | **Azure bölgeleri**
-- | --
Amerika | Kanada Doğu, Kanada Orta, Güney Orta ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2, Orta ABD, Kuzey Orta ABD
Avrupa | UK Batı, UK Güney, Kuzey Avrupa, Batı Avrupa, Fransa Orta, Fransa Güney
Asya | Güney Hindistan, Orta Hindistan, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Kore Orta, Kore Güney Batı, Japonya
Avustralya   | Avustralya Doğu, Avustralya Güneydoğu
Azure Kamu    | ABD Devleti Virginia, ABD Devleti Iowa, ABD Devleti Arizona, ABD Devleti Texas, US DOD Doğu, ABD DOD Orta
Almanya | Almanya Orta, Almanya Kuzeydoğu
Çin | Çin Doğu, Çin Kuzey

>[!NOTE]
>
> Güney Brezilya bölgesinde, yalnızca çoğaltma ve birini Orta Güney ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2 ve orta Kuzey ABD bölgeleri için yük devretme ve ilk duruma döndürme.

## <a name="support-for-vmdisk-management"></a>VM/disk yönetimi için destek

**Eylem** | **Ayrıntılar**
-- | ---
Çoğaltılmış sanal diski yeniden boyutlandırma | Desteklenen
Çoğaltılmış VM'ye disk ekleme | Desteklenmiyor. Sanal makine için çoğaltmayı devre dışı bırakmak için disk ekleyin ve ardından çoğaltmayı yeniden etkinleştirin.


## <a name="support-for-compute-configuration"></a>İşlem yapılandırması için destek

**Yapılandırma** | **Desteklenen/desteklenmeyen** | **Açıklamalar**
--- | --- | ---
Boyut | Herhangi bir Azure VM boyutu en az 2 CPU Çekirdeği ve 1 GB RAM | Başvurmak [Azure sanal makinesi boyutları](../virtual-machines/windows/sizes.md)
Kullanılabilirlik kümeleri | Desteklenen | Portalı'nda varsayılan seçenek 'çoğaltmayı Etkinleştir' adımı sırasında kullanıyorsanız, kullanılabilirlik kümesi kaynak bölge yapılandırmasını alarak oluşturduğunuz otomatik olarak. Hedef kullanılabilirlik kümesi değiştirebilirsiniz ' çoğaltılan öğe > Ayarlar > işlem ve ağ > kullanılabilirlik kümesi ' her zaman.
Karma kullanım Avantajı (HUB) VM'ler | Desteklenen | Kaynak VM, etkin hub'ı lisansı varsa, yük devretme testi veya yük devretme VM ayrıca HUB lisansı kullanır.
Sanal makine ölçek kümeleri | Desteklenmiyor |
Microsoft Azure galeri görüntüleri - yayımlandı | Desteklenen | VM'nin Site Recovery tarafından desteklenen bir işletim sisteminde çalıştırdığı sürece desteklenir
Azure Galerisi'ndeki görüntüleri - üçüncü taraf yayımlandı | Desteklenen | VM'nin Site Recovery tarafından desteklenen bir işletim sisteminde çalıştırdığı sürece desteklenmiyor.
Özel görüntüleri - üçüncü taraf yayımlandı | Desteklenen | VM'nin Site Recovery tarafından desteklenen bir işletim sisteminde çalıştırdığı sürece desteklenmiyor.
Site RECOVERY'yi kullanarak VM'lerin geçişi | Desteklenen | Site Recovery kullanılarak Azure'da VMware/fiziksel makine geçişi ise, mobilite hizmetinin eski sürümü kaldırın ve makineyi başka bir Azure bölgesine çoğaltmak önce yeniden gerekir.

## <a name="support-for-storage-configuration"></a>Depolama yapılandırması desteği

**Yapılandırma** | **Desteklenen/desteklenmeyen** | **Açıklamalar**
--- | --- | ---
En yüksek işletim sistemi disk boyutu | 2048 GB | Başvurmak [VM'ler tarafından kullanılan diskler.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Maksimum veri diski boyutu | 4095 GB | Başvurmak [VM'ler tarafından kullanılan diskler.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Veri diski sayısı | Belirli bir Azure VM boyutu tarafından desteklenen 64 olarak en fazla | Başvurmak [Azure sanal makinesi boyutları](../virtual-machines/windows/sizes.md)
Geçici disk | Her zaman çoğaltmanın dışında tutulan | Geçici disk her zaman çoğaltmadan dışlandı. Herhangi bir kalıcı veri Azure Kılavuzu göre geçici diskteki koymalısınız değil. Başvurmak [Azure Vm'leri üzerinde geçici disk](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) daha fazla ayrıntı için.
Disk üzerindeki veri değişiklik oranı | Premium depolama için disk başına 10 MB/sn ile standart depolama için disk başına 2 MB/sn sayısı | Disk ortalama veri değişikliği hızınıza 10 MB/sn (Premium için) 2 MB/sn (standart için) sürekli olarak ise, çoğaltma yakalamaz. Ancak, bir arada sırada veri veri bloğu ise ve veri değişim hızı (Premium için) 10 MB/sn ile 2 MB/sn (standart için) bir süre değerinden büyükse ve gelir, çoğaltma Kaçırdığınız. Bu durumda, biraz Gecikmeli kurtarma noktalarını görebilirsiniz.
Standart depolama hesaplarında diskler | Desteklenen |
Disklerde, premium depolama hesapları | Desteklenen | Bir VM, premium ve standart depolama hesapları arasında yayılabilir disk varsa, aynı depolama yapılandırması hedef bölgede olduğundan emin olmak her disk için farklı bir hedef depolama hesabı seçebilirsiniz.
Standart yönetilen diskler | Azure Site Recovery, desteklenen Azure bölgelerinde desteklenir. |  
Premium yönetilen diskler | Azure Site Recovery, desteklenen Azure bölgelerinde desteklenir. |
Depolama alanları | Desteklenen |         
(SSE) bekleyen şifreleme | Desteklenen | SSE, depolama hesapları varsayılan ayardır.   
Azure Disk şifrelemesi (ADE) | Desteklenmiyor |
Sık erişimli Ekle/Kaldır disk | Desteklenmiyor | VM veri diski ekleyip, çoğaltmayı devre dışı bırakın ve yeniden sanal Makineye yönelik çoğaltmayı etkinleştirmek gerekir.
Diski hariç tutma | Desteklenmiyor|   Geçici disk, varsayılan olarak çıkarılır.
Depolama alanları doğrudan  | Desteklenmiyor|
Genişleme dosya sunucusu  | Desteklenmiyor|
LRS | Desteklenen |
GRS | Desteklenen |
RA-GRS | Desteklenen |
ZRS | Desteklenmiyor |  
Seyrek erişimli ve sık erişimli depolama | Desteklenmiyor | Seyrek erişimli ve sık erişimli depolama alanı sanal makine diskleri desteklenmez
Sanal ağlar için Azure depolama güvenlik duvarları  | Hayır | Önbellek depolama hesaplarına çoğaltılan verileri depolamak için kullanılan belirli bir Azure sanal ağları erişimine desteklenmiyor.
Genel amaçlı V2 depolama hesaplarının (hem sık erişimli ve seyrek erişimli Katmanlar) | Hayır | İşlem maliyetleri artırmak için genel amaçlı V1 depolama hesaplarında önemli ölçüde karşılaştırılır.

>[!IMPORTANT]
> VM disk ölçeklenebilirlik ve performans hedefleri için dikkate aldığınızdan emin olun [Linux](../virtual-machines/linux/disk-scalability-targets.md) veya [Windows](../virtual-machines/windows/disk-scalability-targets.md) herhangi bir performans sorunlarından kaçınmak için sanal makineler. Varsayılan ayarları izlerseniz, Site Recovery gerekli diskler ve depolama hesapları kaynak yapılandırmaya göre oluşturun. Özelleştirme ve kendi ayarlarınızı seçin, kaynak için VM'lerin disk ölçeklenebilirlik ve performans hedefleri izleyin emin olun.

## <a name="support-for-network-configuration"></a>Ağ yapılandırması için destek
**Yapılandırma** | **Desteklenen/desteklenmeyen** | **Açıklamalar**
--- | --- | ---
Ağ arabirimi (NIC) | Belirli bir Azure VM boyutu tarafından desteklenen NIC sayısı üst sınırı kadar | Yük devretme testi veya yük devretme işleminin bir parçası olarak oluşturulan VM NIC oluşturulur. VM yük devretmesi NIC sayısına VM çoğaltmayı etkinleştirme sırasında sahip kaynak NIC sayısına bağlıdır. Ekleme/NIC çoğaltmayı etkinleştirdikten sonra kaldırma, yük devretme VM NIC sayısı etkilemez.
İnternet Yük Dengeleyici | Desteklenen | Bir kurtarma planında bir azure Otomasyonu betik kullanarak önceden yapılandırılmış bir yük dengeleyici ilişkilendirmeniz gerekir.
İç Load balancer | Desteklenen | Bir kurtarma planında bir azure Otomasyonu betik kullanarak önceden yapılandırılmış bir yük dengeleyici ilişkilendirmeniz gerekir.
Genel IP| Desteklenen | NIC için zaten var olan bir genel IP ilişkilendirmek veya oluşturun ve bir azure Otomasyonu komut dosyası kullanarak bir kurtarma planında NIC ilişkilendirmeniz gerekir.
NSG, NIC'ye (Resource Manager) üzerinde| Desteklenen | NSG bir kurtarma planında bir azure Otomasyonu betik kullanarak NIC ile ilişkilendirme gerekir.  
NSG alt (Resource Manager ve klasik)| Desteklenen | NSG alt ağa bir azure Otomasyonu komut dosyası kullanarak bir kurtarma planında ilişkilendirme gerekir.
NSG, VM (Klasik)| Desteklenen | NSG bir kurtarma planında bir azure Otomasyonu betik kullanarak NIC ile ilişkilendirme gerekir.
Ayrılmış IP (statik IP) / kaynak IP koru | Desteklenen | Statik IP yapılandırması kaynak VM üzerindeki NIC varsa ve aynı IP hedef alt ağa sahip VM yük devretmesi için atanmış. Hedef alt ağ, aynı IP yoksa, bir alt ağdaki kullanılabilir IP'lerin bu VM için ayrılmıştır. Ettiğiniz sabit bir IP belirtebilirsiniz. ' çoğaltılan öğe > Ayarlar > işlem ve ağ > ağ arabirimleri. NIC'yi seçin ve tercih ettiğiniz IP ve alt ağ belirtin.
Dinamik IP| Desteklenen | Kaynak VM NIC'i dinamik IP yapılandırması varsa, yük devretme NIC VM varsayılan olarak dinamiktir. Ettiğiniz sabit bir IP belirtebilirsiniz. ' çoğaltılan öğe > Ayarlar > işlem ve ağ > ağ arabirimleri. NIC'yi seçin ve tercih ettiğiniz IP ve alt ağ belirtin.
Traffic Manager tümleştirmesi | Desteklenen | Traffic manager uç noktasına düzenli olarak kaynak bölgede ve yük devretme durumunda hedef bölgede uç noktaya trafik yönlendirilir şekilde önceden yapılandırabilirsiniz.
Azure DNS yönetilen | Desteklenen |
Özel DNS  | Desteklenen |    
Kimliği doğrulanmamış Proxy | Desteklenen | Başvurmak [ağ rehberi belgesi.](site-recovery-azure-to-azure-networking-guidance.md)    
Kimliği doğrulanmış Proxy | Desteklenmiyor | VM için giden bağlantı kimliği doğrulanmış bir ara sunucu kullanıyorsa, Azure Site Recovery kullanarak yinelenemez.    
Siteden siteye VPN ile şirket içi (ile veya olmadan ExpressRoute)| Desteklenen | Udr ve Nsg'ler Site kurtarma trafiği şirket içi yönlendirilmemesidir şekilde yapılandırıldığından emin olun. Başvurmak [ağ rehberi belgesi.](site-recovery-azure-to-azure-networking-guidance.md)  
VNET'ten VNET'e bağlantı | Desteklenen | Başvurmak [ağ rehberi belgesi.](site-recovery-azure-to-azure-networking-guidance.md)  
Sanal Ağ Hizmeti Uç Noktaları | Desteklenen | Azure depolama güvenlik duvarlarını sanal ağlar için desteklenmez. Önbellek depolama hesaplarına çoğaltılan verileri depolamak için kullanılan belirli bir Azure sanal ağları erişimine desteklenmiyor.
Hızlandırılmış Ağ | Desteklenmiyor | Hızlandırılmış ağ etkin olan bir VM çoğaltılabilir ancak yük devretme VM hızlandırılmış ağ etkin olmayacaktır. Hızlandırılmış ağ Ayrıca kaynak VM üzerinde yeniden çalışma için devre dışı bırakılır.


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure Vm'lerini çoğaltma Kılavuzu ağı](site-recovery-azure-to-azure-networking-guidance.md)
- Yüklerinizi korumaya başlayın [Azure Vm'lerini çoğaltma](site-recovery-azure-to-azure.md)
