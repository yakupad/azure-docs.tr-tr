---
title: include dosyası
description: include dosyası
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 669966ce21c5c6c2d0653eb51c81fe78aa0b3a12
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39057319"
---
**Yapılandırma/işlem sunucusu gereksinimleri**

**Bileşen** | **Gereksinim** 
--- | ---
**DONANIM AYARLARI** | 
CPU çekirdekleri | 8 
RAM | 16 GB
Disk sayısı | işletim sistemi diski, işlem sunucusu önbellek diski ve yeniden çalışma için bekletme sürücüsü dahil olmak üzere, 3 
Boş disk alanı (işlem sunucusu önbelleği) | 600 GB
Boş disk alanı (bekletme diski) | 600 GB
 | 
**YAZILIM AYARLARI** | 
İşletim sistemi | Windows Server 2012 R2 <br> Windows Server 2016
İşletim sistemi yerel ayarı | İngilizce (en-us)
Windows Server rolleri | Bu rolleri etkinleştirmeyin: <br> - Active Directory Domain Services <br>- İnternet Bilgi Hizmetleri <br> - Hyper-V 
Grup İlkeleri | Bu grup ilkeleri etkinleştirme: <br> -Komut istemine erişimi engelleyin. <br> -Kayıt defteri düzenleme araçlarına erişimi engelleyin. <br> -Mantıksal dosya ekleri için güven. <br> -Betik yürütmeyi açma. <br> [Daha fazla bilgi](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Önceden var olan varsayılan Web sitesi <br> -Önceden var olan Web sitesi/443 numaralı bağlantı noktasını dinlemeye uygulama <br>-Etkinleştir [anonim kimlik doğrulaması](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Etkinleştir [Fastcgı](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) ayarı 
| 
**AĞ AYARLARI** | 
IP adresi türü | Statik 
İnternet erişimi | Sunucusunun şu URL'lere erişimi olmalıdır (doğrudan veya proxy aracılığıyla) <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com  <br> - https://management.azure.com <br> -*. Services.VisualStudio.com adresine <br> - time.nist.gov <br> - time.windows.com <br> OVF ayrıca aşağıdaki URL'lere erişim gerekir <br> - https://login.microsoftonline.com <br> - https://secure.aadcdn.microsoftonline-p.com <br> - https://login.live.com  <br> - https://auth.gfx.ms <br> - https://graph.windows.net <br> - https://login.windows.net <br> - https://www.live.com <br> - https://www.microsoft.com <br> - https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi 
Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br>9443 (Veri aktarımı) 
NIC türü | VMXNET3 (yapılandırma sunucusu VMware VM ise)
 | 
**YÜKLENECEK YAZILIM** | 
VMware vSphere Powerclı | [Powerclı 6.0 sürümünün](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) yapılandırma sunucusunu bir VMware sanal makine üzerinde çalışıyorsa yüklü olması gerekir.
MYSQL | MySQL yüklü olması gerekir. El ile yükleyebilirsiniz veya Site Recovery yükleyebilirsiniz.

**Yapılandırma/işlem sunucusu boyutlandırma gereksinimleri**

**CPU** | **Bellek** | **Önbellek diski** | **Veri değişiklik oranı** | **Çoğaltılan makineler**
--- | --- | --- | --- | ---
8 Vcpu<br/><br/> Yuva 2 * 4 çekirdek \@ 2.5 GHz | 16GB | 300 GB | 500 GB veya daha az | < 100 makineler
12 Vcpu<br/><br/> 2 socks * 6 çekirdek \@ 2.5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | 100-150 makineler
16 Vcpu<br/><br/> 2 socks * 8 çekirdek \@ 2.5 GHz | 32 GB | 1 TB | 1-2 TB | 150 -200 makineler

