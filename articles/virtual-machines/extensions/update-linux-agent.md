---
title: Github'dan Azure Linux Aracısı güncelleştirme | Microsoft Docs
description: Azure'da, Linux VM için Azure Linux Aracısı güncelleştirme öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: danis
ms.openlocfilehash: b5a482ef6d15f1c6b1942a6128a807d7c6189918
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942805"
---
# <a name="how-to-update-the-azure-linux-agent-on-a-vm"></a>Bir VM üzerinde Azure Linux Aracısı güncelleştirme

Güncelleştirmek için [Azure Linux Aracısı](https://github.com/Azure/WALinuxAgent) azure'da bir Linux VM üzerinde önceden olmanız gerekir:

- Azure'da çalışan bir Linux VM.
- SSH kullanarak, Linux VM bağlantı.

Her zaman Linux distro depodaki bir paket için ilk denetlemelisiniz. Kullanılabilir paketi otomatik güncelleştirme'yi etkinleştirme en son sürümü, ancak, Linux Aracısı'nı her zaman en son güncelleştirmeleri al sağlayacak olmayabilir mümkündür. Paket yöneticileri yüklerken sorunlarla olmalıdır, destek distro satıcıdan arama.

## <a name="minimum-virtual-machine-agent-support-in-azure"></a>Azure'da minimum sanal makine Aracısı desteği
Doğrulama [azure'da sanal makine aracılar için en düşük sürüm Destek](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) devam etmeden önce.

## <a name="updating-the-azure-linux-agent"></a>Azure Linux Aracısı'nı güncelleştirme

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Güncelleştirme paketi önbelleği

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

#### <a name="restart-agent-for-1404"></a>14.04 için aracıyı yeniden başlatın

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a>Aracı 16.04 için yeniden başlatma / 17.04

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a>Debian

### <a name="debian-7-wheezy"></a>Debian 7 "Wheezy"

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Güncelleştirme paketi önbelleği

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Aracı için otomatik güncelleştirmeyi etkinleştir
Debian bu sürümü bir sürümü mevcut değil > = 2.0.16, bu nedenle edinebilmeleri için kullanılamıyor. Yukarıdaki komut çıktısı paket güncel olup olmadığını gösterir.

### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 "Jessie" / "Debian 9 uzatma"

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
apt list --installed | grep waagent
```

#### <a name="update-package-cache"></a>Güncelleştirme paketi önbelleği

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun 

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a>RedHat / CentOS

### <a name="rhelcentos-6"></a>RHEL/CentOS 6

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetle

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun 

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/\# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a>RHEL/CentOS 7

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetle

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun 

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a>SUSE SLES

### <a name="suse-sles-11-sp4"></a>SUSE SLES 11 SP4

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetle

Yukarıdaki çıkış paketi güncel olup olmadığını gösterir.

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun 

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a>SUSE SLES 12 SP2

#### <a name="check-your-current-package-version"></a>Geçerli paket sürümü denetleyin

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Kullanılabilir güncelleştirmeleri denetle

Yukarıdaki çıktıda Bu, paket değerine kadar tarih olup olmadığını gösterir.

#### <a name="install-the-latest-package-version"></a>Son paket sürümü yükleyin

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun 

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>Waagent hizmetini yeniden başlatın

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a>Oracle 6 ve 7

Oracle Linux için olduğundan emin olun `Addons` depo etkindir. Dosyayı düzenlemek seçin `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) veya `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), satır değiştirip `enabled=0` için `enabled=1` altında **[ol6_addons]** veya **[ol7_addons]** bu dosyadaki.

Ardından, Azure Linux Aracısı'nın en son sürümünü yüklemek için şunu yazın:

```bash
sudo yum install WALinuxAgent
```

Eklenti deposu bulamazsanız, Oracle Linux sürüme göre .repo dosyanızı sonunda bu satırları yalnızca ekleyebilirsiniz:

Oracle Linux 6 sanal makineler için:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Oracle Linux 7 sanal makineler için:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```

Ardından yazın:

```bash
sudo yum update WALinuxAgent
```

Tüm yapmanız gereken, herhangi bir nedenden dolayı ondan yüklemeniz gerekir, ancak budur genellikle https://github.com doğrudan, aşağıdaki adımları kullanın.


## <a name="update-the-linux-agent-when-no-agent-package-exists-for-distribution"></a>Dağıtım için hiçbir aracı paketi mevcut olduğunda Linux aracısını güncelleştirme

(Varsayılan olarak, Redhat, CentOS ve Oracle Linux sürümleri 6.4 ve 6.5 gibi yüklemeyin bazı distro'lar vardır) wget yazarak yüklemek `sudo yum install wget` komut satırında.

### <a name="1-download-the-latest-version"></a>1. En son sürümü yükleyin
Açık [github'da Azure Linux Aracısı sürüm](https://github.com/Azure/WALinuxAgent/releases) bir web sayfası ve en son sürüm numarasını öğrenin. (Geçerli sürümünüzü yazarak bulabilirsiniz `waagent --version`.)

#### <a name="for-version-22x-or-later-type"></a>Sürüm için 2.2.x veya daha sonra yazın:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip
cd WALinuxAgent-2.2.x
```

Aşağıdaki satırı sürüm 2.2.0 örnek olarak kullanılmıştır:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-the-azure-linux-agent"></a>2. Azure Linux Aracısı'nı yükleme

#### <a name="for-version-22x-use"></a>Sürüm için 2.2.x, kullanın:
Paket yüklemeniz gerekebilir `setuptools` ilk--bkz [burada](https://pypi.python.org/pypi/setuptools). Ardından çalıştırın:

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Otomatik güncelleştirme etkinleştirildiğinden emin olun

İlk olarak, etkin olup olmadığını denetleyin:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' bulun. Bu çıktı görürseniz etkinleştirilir:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Çalışma etkinleştirmek için:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-the-waagent-service"></a>3. Waagent hizmetini yeniden başlatın
Linux distro'lar çoğu için:

```bash
sudo service waagent restart
```

Ubuntu için kullanın:

```bash
sudo service walinuxagent restart
```

CoreOS için kullanın:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-the-azure-linux-agent-version"></a>4. Azure Linux Aracısı sürüm onaylayın
    
```bash
waagent -version
```

CoreOS için yukarıdaki komut çalışmayabilir.

Azure Linux Aracısı sürüm yeni sürüme güncelleştirilip güncelleştirilmediğini görürsünüz.

Azure Linux Aracısı ile ilgili daha fazla bilgi için bkz: [Azure Linux Aracısı Benioku](https://github.com/Azure/WALinuxAgent).
