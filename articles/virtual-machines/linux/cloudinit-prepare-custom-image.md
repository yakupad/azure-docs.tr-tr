---
title: Bulut init ile kullanmak için Azure VM görüntüsünü hazırlama | Microsoft Docs
description: Bulut init ile dağıtım için önceden var olan bir Azure VM görüntüsü hazırlama
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: ff5c76ca0a164d09e45488cb7abf7f2c2ee50a95
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064110"
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Bulut init ile kullanılmak üzere var olan bir Linux Azure VM görüntüsünü hazırlama
Bu makale mevcut bir Azure sanal makine alın ve yeniden dağıtılan ve bulut init kullanıma hazır olması için hazırlama gösterilmektedir. Elde edilen görüntü, yeni bir sanal makine veya sanal makine ölçek kümeleri biri ya da sonra daha fazla bulut-Init dağıtım sırasında özelleştirilebilecek - dağıtmak için kullanılabilir.  Kaynakları Azure tarafından sağlanan sonra bu bulut başlatma komut dosyaları ilk önyükleme çalıştırın. Bulut init yerel olarak Azure ve desteklenen Linux distro'lar işleyişi hakkında daha fazla bilgi için bkz: [bulut init genel bakış](using-cloud-init.md)

## <a name="prerequisites"></a>Önkoşullar
Bu belge, Linux işletim sisteminin desteklenen bir sürümünü çalıştıran bir çalışan Azure sanal makine zaten olduğunu varsayar. Makine, gereksinimlerinize uygun için zaten yapılandırılmış gerekli olan tüm modülleri yüklü, tüm gerekli güncelleştirmeler işlenen ve gereksinimlerinizi karşıladığından emin olmak için sınanmıştır. 

## <a name="preparing-rhel-74--centos-74"></a>RHEL 7.4 hazırlama / CentOS 7.4
Bulut init yükleyebilmek için aşağıdaki komutları çalıştırın ve Linux VM'de oturum için SSH gerekir.

```bash
sudo yum install -y cloud-init gdisk
sudo yum check-update cloud-init -y
sudo yum install cloud-init -y
```

Güncelleştirme `cloud_init_modules` bölümüne `/etc/cloud/cloud.cfg` aşağıdaki modüller eklemek için:
```bash
- disk_setup
- mounts
```

Hangi bir genel amaçlı bir örnek şudur `cloud_init_modules` bölüm gibi görünüyor.
```bash
cloud_init_modules:
 - migrator
 - bootcmd
 - write-files
 - growpart
 - resizefs
 - disk_setup
 - mounts
 - set_hostname
 - update_hostname
 - update_etc_hosts
 - rsyslog
 - users-groups
 - ssh
```
Bir dizi görevi kısa ömürlü diskleri işleme ve sağlama ile ilgili olarak güncelleştirilmesi gereken `/etc/waagent.conf`. Uygun ayarları güncelleştirmek için aşağıdaki komutları çalıştırın. 
```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
```
Yalnızca Azure, yeni bir dosya oluşturarak Azure Linux aracısı için bir veri kaynağı izin `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` aşağıdaki satırları içeren bir düzenleyiciyi kullanarak:

```bash
# This configuration file is provided by the WALinuxAgent package.
datasource_list: [ Azure ]
```

Bir bekleyen ana bilgisayar adı kayıt hatayı gidermek için bir yapılandırma ekleyin.
```bash
cat > /etc/cloud/hostnamectl-wrapper.sh <<\EOF
#!/bin/bash -e
if [[ -n $1 ]]; then
  hostnamectl set-hostname $1
else
  hostname
fi
EOF

chmod 0755 /etc/cloud/hostnamectl-wrapper.sh

cat > /etc/cloud/cloud.cfg.d/90-hostnamectl-workaround-azure.cfg <<EOF
# local fix to ensure hostname is registered
datasource:
  Azure:
    hostname_bounce:
      hostname_command: /etc/cloud/hostnamectl-wrapper.sh
EOF
```

Var olan Azure görüntünüzü yapılandırılmış takas dosyası varsa ve bulut init kullanarak yeni görüntüleri takas dosyası yapılandırmasını değiştirmek istediğiniz varolan takas dosyası kaldırmanız gerekir.

Red Hat görüntüleri - tabanlı için belge aşağıdaki Red Hat açıklayan içindeki yönergeleri izleyin nasıl [takas dosyası kaldırmak](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/swap-removing-file).

Etkin swapfile ile CentOS görüntüleri için devre dışı Takas dosyasını etkinleştirmek için aşağıdaki komutu çalıştırabilirsiniz:
```bash
sudo swapoff /mnt/resource/swapfile
```

Takas başvuru kaldırılır olun `/etc/fstab` -aşağıdaki çıkış gibi görünmelidir:
```text
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

Alanı kaydetmek ve takas dosyası kaldırmak için aşağıdaki komutu çalıştırabilirsiniz:
```bash
rm /mnt/resource/swapfile
```
## <a name="extra-step-for-cloud-init-prepared-image"></a>Bulut init hazırlıklı görüntüsü için ek adım
> [!NOTE]
> Görüntünüzü daha önce varsa bir **bulut init** hazırlıklı ve yapılandırılmış görüntüsü, yapmanız gereken aşağıdaki adımları.

Aşağıdaki üç komutu yalnızca yeni bir özel kaynak görüntüsü olmasını özelleştirme VM bulut başlatma tarafından daha önce sağlanan kullanılır.  Görüntünüzü Azure Linux Aracısı'nı kullanarak yapılandırdıysanız bu çalışması gerekmez.

```bash
sudo rm -rf /var/lib/cloud/instances/* 
sudo rm -rf /var/log/cloud-init*
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Linux aracısı sonlandırılıyor ayarı 
Tüm Azure platform görüntüleri Azure Linux aracısı tarafından bulut başlatma veya yapılandırıldıysa, ne olursa olsun yüklü olması.  Linux makineden kullanıcı sağlamayı tamamlamak için aşağıdaki komutu çalıştırın. 

```bash
sudo waagent -deprovision+user -force
```

Azure Linux Aracısı deprovision komutları hakkında daha fazla bilgi için bkz: [Azure Linux Aracısı](../extensions/agent-linux.md) daha fazla ayrıntı için.

SSH oturumu çıkın ve ardından, bash kabuğundan ayırması, generalize ve yeni bir Azure VM görüntüsü oluşturmak için aşağıdaki AzureCLI komutları çalıştırın.  Değiştir `myResourceGroup` ve `sourceVmName` , sourceVM yansıtma uygun bilgilerle.

```bash
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek bulut init örnekleri için aşağıdakilere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)
