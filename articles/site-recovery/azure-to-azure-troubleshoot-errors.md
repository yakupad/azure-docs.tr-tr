---
title: Azure Site Recovery Azure'dan Azure'a çoğaltma sorunlarını ve hataları için sorun giderme | Microsoft Docs
description: Olağanüstü durum kurtarma için Azure sanal makineleri çoğaltırken hatalarını ve sorunlarını giderme
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: sujayt
ms.openlocfilehash: a41cd658060ef92efb0fc21a98ca616276378c5e
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113863"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure'dan Azure'a VM çoğaltmayla sorunları giderme

Bu makalede Azure Site Recovery çoğaltma ve Azure sanal makineleri bir bölgesinden başka bir bölgeye kurtarma giren yaygın sorunların ve bunları nasıl giderebileceğinizden açıklar. Desteklenen yapılandırmalar hakkında daha fazla bilgi için bkz. [Azure Vm'lerini çoğaltma için destek matrisi](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure kaynak kotası sorunları (hata kodu 150097)
Azure Vm'lerinde olağanüstü durum kurtarma bölgeniz olarak kullanmayı planlıyorsanız hedef bölgede oluşturmak için aboneliğinizi yeniden etkinleştirilmesi gerekir. Ayrıca, aboneliğinizi yeterli kotası belirli boyut sanal makineler oluşturmak için etkinleştirilmiş olması gerekir. Varsayılan olarak, Site Recovery, hedef sanal makine için aynı boyutta VM kaynağı olarak seçer. Eşleşen boyutu kullanılabilir değilse, en yakın olası boyutu otomatik olarak seçilir. Kaynak VM yapılandırması destekleyen hiçbir eşleşen boyutu varsa, bu hata iletisi görüntülenir:

**Hata kodu** | **Olası nedenler** | **Öneri**
--- | --- | ---
150097<br></br>**İleti**: VmName sanal makinesi için çoğaltma etkinleştirilemedi. | -Abonelik Kimliğinizi hedef bölge konumda herhangi bir VM oluşturmak için etkinleştirilmemiş olabilir.</br></br>-Abonelik Kimliğinizi etkinleştirilmemiş veya hedef bölge konumunda belirli VM boyutları oluşturmak için yeterli kotası yok.</br></br>-Bir uygun bir hedef kaynak VM NIC sayısı (2) eşleşen bir VM boyutu için abonelik Kimliğini hedef bölge konumunda bulunan değil.| İlgili kişi [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) gereken hedef konum aboneliğinizde VM boyutları için VM oluşturmayı etkinleştirmek için. Etkinleştirildikten sonra başarısız olan işlemi yeniden deneyin.

### <a name="fix-the-problem"></a>Sorunu
Sizinle iletişim [Azure fatura desteğine](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) hedef konumda gerekli boyutlardaki Vm'leri oluşturmak aboneliğinizi etkinleştirmek için.

Hedef konumu bir kapasite kısıtlaması varsa, çoğaltmayı devre dışı bırakın ve gerekli boyutlardaki Vm'leri oluşturmak için yeterli kotası aboneliğinizin bulunduğu farklı bir konuma etkinleştirin.

## <a name="trusted-root-certificates-error-code-151066"></a>Güvenilen kök sertifika (hata kodu 151066)

Sanal makinede mevcut en yeni güvenilen kök sertifikalar mevcut değilse, "çoğaltmayı etkinleştir" işi başarısız olabilir. Sertifikaları olmadan, Site Recovery hizmeti çağrıları VM'den yetkilendirme ve kimlik doğrulaması başarısız. Başarısız "çoğaltmayı etkinleştir" Site kurtarma işi için hata iletisi görüntülenir:

**Hata kodu** | **Olası nedeni** | **Öneriler**
--- | --- | ---
151066<br></br>**İleti**: Site Recovery yapılandırması başarısız oldu. | İstenen makinede mevcut olmayan kimlik doğrulaması ve yetkilendirme için kök sertifikalarını kullanılan güvenilir. | -Windows işletim sistemi çalıştıran bir VM için güvenilen kök sertifikalar makinede mevcut olduğundan emin olun. Bilgi için [yapılandırma Güvenilen Kökleri ve izin verilmeyen sertifikaları](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Linux işletim sistemini çalıştıran bir VM için Linux işletim sistemi sürümü dağıtıcı tarafından yayımlanan güvenilen kök sertifikalar için yönergeleri izleyin.

### <a name="fix-the-problem"></a>Sorunu
**Windows**

Tüm güvenilen kök sertifikalar makinede mevcut olacak şekilde sanal makinede en son Windows güncelleştirmelerini yükleyin. Bağlantısı kesilmiş bir ortamda kullanıyorsanız, kuruluşunuz sertifikaları almak için standart Windows güncelleştirme işlemi izleyin. Gerekli sertifikaları sanal makinede mevcut değilse, Site Recovery hizmetine çağrı güvenlikle ilgili nedenlerle başarısız.

Tipik Windows güncelleştirme yönetimi veya sertifika güncelleştirme yönetimi işlemi sanal makinelere en son kök sertifikalar ve güncelleştirilmiş sertifika iptal listesini almak için kuruluşunuzdaki izleyin.

Sorunun giderilmiş olduğunu doğrulamak için vm'nizde bir tarayıcıdan login.microsoftonline.com için gidin.

**Linux**

Sanal makinede en son güvenilir kök sertifikaları ve en son sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri izleyin.

SuSE Linux sertifika listesini korumak için çözümlemeyin kullandığından, aşağıdaki adımları izleyin:

1.  Kök kullanıcı olarak oturum açın.

2.  Dizini değiştirmek için bu komutu çalıştırın.

      ``# cd /etc/ssl/certs``

3. Symantec kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4. Symantec kök CA sertifika bulunmazsa, dosyayı indirmek için aşağıdaki komutu çalıştırın. Hatalar için denetleyin ve ağ hataları için önerilen eylemi izleyin.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

5. Baltimore kök CA sertifikası mevcut olup olmadığını denetleyin.

      ``# ls Baltimore_CyberTrust_Root.pem``

6. Baltimore kök CA sertifika bulunmazsa sertifikayı indirin.  

    ``# wget http://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

7. DigiCert_Global_Root_CA cert var olup olmadığını denetleyin.

    ``# ls DigiCert_Global_Root_CA.pem``

8. DigiCert_Global_Root_CA bulunmazsa sertifikayı indirmek için aşağıdaki komutları çalıştırın.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

9. Rehash betiği, bir sertifikayı güncelleştirmek için yeni indirilen sertifika için konu karmaları çalıştırın.

    ``# c_rehash``

10. Çözümlemeyin sertifikalarını oluşturuldukça konu karıştırır, kontrol edin.

    - Komut

      ``# ls -l | grep Baltimore``

    - Çıktı

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Komut

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Çıktı

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Komut

      ``# ls -l | grep DigiCert_Global_Root``

    - Çıktı

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

11. Filename b204d74a.0 ile VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem dosyanın bir kopyasını oluşturma

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

12. Filename 653b494a.0 ile Baltimore_CyberTrust_Root.pem dosyanın bir kopyasını oluşturma

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Filename 3513523f.0 ile DigiCert_Global_Root_CA.pem dosyanın bir kopyasını oluşturma

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Dosyaları mevcut olup olmadığını denetleyin.  

    - Komut

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Çıktı

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Recovery hizmeti URL'lerine veya IP aralıkları (hata kodu 151037 veya 151072) için giden bağlantı

Site Recovery çoğaltması için iş, giden bağlantı için özel URL veya IP aralıkları VM'den gerekli. Sanal makinenize bir güvenlik duvarının arkasındaysa ya da giden bağlantıyı denetlemek için ağ güvenlik grubu (NSG) kuralları kullanıyorsa, aşağıdaki hata iletilerinden birini görebilirsiniz:

**Hata kodları** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151037<br></br>**İleti**: Azure sanal makinesi Site Recovery'ye kaydetmek için başarısız oldu. | -NSG giden erişimi denetlemek için kullanmakta olduğunuz VM ve gerekli IP aralıkları giden erişim için izin verilenler listesinde değil.</br></br>-Üçüncü taraf güvenlik duvarı araçları kullanıyorsanız ve gerekli IP aralıkları/URL'ler izin verilenler listesinde değil.</br>| -VM üzerinde giden ağ bağlantısını denetlemek için güvenlik duvarı proxy'si kullanıyorsanız, önkoşul URL'leri veya veri merkezi IP aralıkları'nın izin verilenler listesinde olduğundan emin olun. Bilgi için [güvenlik duvarı proxy Kılavuzu](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-VM üzerinde giden ağ bağlantısını denetlemek için NSG kuralları'nı kullanıyorsanız, önkoşul veri merkezi IP aralıklarının Güvenilenler listesinde olduğundan emin olun. Bilgi için [ağ güvenlik grubu Kılavuzu](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**İleti**: Site Recovery yapılandırması başarısız oldu. | Site Recovery Hizmeti uç noktalarına bağlantı kurulamıyor. | -VM üzerinde giden ağ bağlantısını denetlemek için güvenlik duvarı proxy'si kullanıyorsanız, önkoşul URL'leri veya veri merkezi IP aralıkları'nın izin verilenler listesinde olduğundan emin olun. Bilgi için [güvenlik duvarı proxy Kılavuzu](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-VM üzerinde giden ağ bağlantısını denetlemek için NSG kuralları'nı kullanıyorsanız, önkoşul veri merkezi IP aralıklarının Güvenilenler listesinde olduğundan emin olun. Bilgi için [ağ güvenlik grubu Kılavuzu](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Sorunu
Güvenilir listeye eklenecek [gerekli URL'leri](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) veya [gerekli IP aralıkları](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), adımları [ağ rehberi belgesi](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disk (hata kodu 150039) makinede bulunamadı

VM'ye yeni bir disk başlatılmalıdır.

**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
150039<br></br>**İleti**: Azure veri diski (DiskName) (DiskURI) mantıksal birim numarası (LUN) (LUNValue) ile aynı LUN değerine sahip VM içinden bildirilen ilgili disk için eşlenmiş. | -Yeni veri diski VM'ye bağlı, ancak başlatılmamış değildi.</br></br>-VM içindeki veri diski diski VM'ye bağlı LUN değerini doğru şekilde bildirmiyor.| Veri disklerini başlatılır ve ardından işlemi yeniden deneyin emin olun.</br></br>İçin Windows: [Ekeme ve başlatma yeni bir disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Linux: [Linux'ta yeni bir veri diski başlatın](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Sorunu
Veri disklerinin başlatıldığından ve sonra işlemi yeniden deneyin emin olun:

- İçin Windows: [Ekeme ve başlatma yeni bir disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Linux: [Linux'ta yeni bir veri diski ekleme](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Sorun devam ederse desteğe başvurun.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Azure VM için "çoğaltmayı etkinleştir" seçimi görülemiyor

 **1. neden: Kaynak grubu ve kaynak sanal makine farklı konumlarda** <br>
Azure Site Recovery şu anda kaynak bölge kaynak grubunu ve sanal makineler aynı konumda olması gerektiğini uygulanan. Böyle değilse, ardından, koruma süresi sırasında sanal makineyi bulamadı olmaz.

**Neden 2: Kaynak grubu, seçili abonelik parçası değil** <br>
Belirtilen abonelik bir parçası değilse, kaynak grubunu koruma süresi bulmak mümkün olmayabilir. Kaynak grubu kullanılıyor aboneliğe ait olduğundan emin olun.

 **3. neden: Eski yapılandırma** <br>
VM için çoğaltmayı etkinleştirmek istediğiniz görmüyorsanız, Azure sanal makinesinde eski bir Site Recovery yapılandırması nedeniyle kalmayabilir. Eski yapılandırmayı aşağıdaki durumlarda bir Azure sanal makinesinde kalmış olabilir:

- Site Recovery kullanarak Azure VM için çoğaltma etkin ve ardından Site Recovery kasası açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan silinir.
- Site Recovery kullanarak Azure VM için çoğaltma etkin ve sonra açıkça bir VM üzerinde çoğaltmayı devre dışı bırakmadan Site Recovery kasasını içeren kaynak grubu silindi.

### <a name="fix-the-problem"></a>Sorunu

Kullanabileceğiniz [kaldırmak eski ASR yapılandırma betiğini](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) ve Azure sanal makinesinde eski Site Recovery yapılandırmayı kaldırmak. Eski yapılandırmayı kaldırdıktan sonra VM görüyor olması gerekir.

## <a name="unable-to-select-virtual-machine-for-protection"></a>Sanal makine koruma için işaretleyin yapılamıyor 
 **1. neden: sanal makine başarısız veya yanıt vermeyen bir durumda yüklü bazı uzantısı vardır.** <br>
 Sanal makineler gidin > ayarı > Uzantılar ve başarısız durumda herhangi bir uzantısı var olup olmadığını denetleyin. Başarısız uzantının yüklemesini kaldırmak ve sanal makine korumayı yeniden deneyin.<br>
 **2. neden: [VM'in sağlama durumu geçerli değil](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>VM'in sağlama durumu geçerli değil (hata kodu 150019)

VM üzerinde çoğaltmayı etkinleştirmek için sağlama durumu olmalıdır **başarılı**. Aşağıdaki adımları izleyerek VM durumunu kontrol edebilirsiniz.

1.  Seçin **kaynak Gezgini** gelen **tüm hizmetleri** Azure portalında.
2.  Genişletin **abonelikleri** listelemek ve aboneliğinizi seçin.
3.  Genişletin **ResourceGroups** listeleyebilir ve sanal makinenin kaynak grubunu seçin.
4.  Genişletin **kaynakları** listesinde ve sanal makinenizi seçin
5.  Denetleme **provisioningState** örneği görünümünde, sağ taraftaki alan.

### <a name="fix-the-problem"></a>Sorunu

- Varsa **provisioningState** olduğu **başarısız**, sorun giderme ayrıntıları ile Destek ekibiyle iletişime geçin.
- Varsa **provisioningState** olduğu **güncelleştirme**, başka bir uzantı dağıtılabilecek. Bekleme başarısız Site kurtarma işlemini yeniden deneyin ve bunlar için VM üzerinde devam eden herhangi bir işlem olup olmadığını kontrol **çoğaltmayı etkinleştir** işi.



## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>COM +/ Birim Gölge Kopyası Hizmeti hatası (hata kodu 151025)
**Hata kodu** | **Olası nedenler** | **Öneriler**
--- | --- | ---
151025<br></br>**İleti**: Site recovery uzantısı yüklenemedi | -'COM + Sistem uygulaması' hizmeti devre dışı.</br></br>-'Birim gölge kopyası' hizmeti devre dışı bırakıldı.| 'COM + Sistem uygulaması' ve 'Birim gölge kopyası' hizmetlerini otomatik veya el ile başlatma moduna ayarlayın.

### <a name="fix-the-problem"></a>Sorunu

'Hizmetler' konsolunu açın ve 'COM + Sistem uygulaması' emin olun ve 'Birim gölge kopyası', 'Başlangıç türü' 'Devre dışı' olarak ayarlanmamış.
  ![COM hatası](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure sanal makinelerini çoğaltma](site-recovery-replicate-azure-to-azure.md)
