---
title: (Sayfa blobları) yönetilmeyen ve yönetilen diskler depolaması için Microsoft Azure Linux Vm'leri hakkında | Microsoft Docs
description: İle ilgili temel bilgileri öğrenin (sayfa blobları) yönetilmeyen ve yönetilen diskler depolaması için azure'da Linux sanal makineleri.
services: virtual-machines
author: roygara
manager: twooley
ms.service: virtual-machines
ms.workload: storage
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 11/15/2017
ms.author: rogarana
ms.openlocfilehash: a1c867e97879b1830a8596683b8314fd6535aa7c
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036008"
---
# <a name="about-disks-storage-for-azure-linux-vms"></a>Azure Linux Vm'leri için diskleri depolama hakkında
Yalnızca diğer bilgisayarlar gibi azure'da sanal makineler bir işletim sistemini, uygulamalarını ve verilerini depolamak için bir farkı şudur diskleri kullanın. Tüm Azure sanal makineler, en az iki diskin – Linux işletim sistemi diski ve geçici bir diskle sahiptir. İşletim sistemi diski bir görüntüden oluşturulur ve hem işletim sistemi diski ile görüntü sanal sabit bir Azure depolama hesabında depolanan diskleri (VHD). Sanal makineler, VHD'ler olarak da depolanan bir veya daha fazla veri diski olarak da olabilir.

Bu makalede, biz diskleri farklı kullanımları hakkında konuşacak ve ardından farklı tür diskler oluşturma ve kullanma tartışın. Bu makale için de kullanılabilir olan [Windows sanal makineleri](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Sanal makineleri tarafından kullanılan diskler

Diskler, VM'ler tarafından nasıl kullanıldığı bir bakalım.

## <a name="operating-system-disk"></a>İşletim sistemi diski

Her bir sanal makinede bir ekli işletim sistemi diski var. Bir SATA sürücüsü olarak kaydedilir ve/dev/sda varsayılan olarak etiketlenir. Bu disk 2048 gigabayt (GB) maksimum kapasiteye sahiptir.

## <a name="temporary-disk"></a>Geçici disk

Her sanal makine geçici bir diskle içerir. Geçici disk, uygulamalar ve işlemler için kısa vadeli depolama sağlar ve yalnızca sayfa veya takas dosyaları gibi verileri depolamak için tasarlanmıştır. Geçici diskteki veriler kaybolabilir sırasında bir [bakım olayı](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) veya ne zaman, [bir VM'yi yeniden dağıtma](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sırasında standart yeniden başlatma VM'nin geçici sürücüdeki verilerin kalıcı olması. Bununla birlikte, burada verileri, yeni bir ana bilgisayara taşımayı gibi korunmayabilir durumlar vardır. Buna geçici sürücüdeki tüm verileri sisteme önemli olan verilerin olmamalıdır.

Linux sanal makinelerinde genellikle disktir **/dev/sdb** ve biçimlendirilmiş ve takılı **/mnt** Azure Linux aracısı tarafından. Geçici diskin boyutunu, sanal makine boyutuna göre değişir. Daha fazla bilgi için [Linux sanal makine boyutları](../windows/sizes.md).

Azure geçici disk nasıl kullandığı hakkında daha fazla bilgi için bkz. [Microsoft Azure sanal makineler üzerinde geçici sürücüyü anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Veri diski

Veri diski uygulama verileri veya tutmak için ihtiyacınız olan diğer verileri depolamak için bir sanal makineye bağlı bir vhd'dir. Veri diskleri SCSI sürücüsü olarak kaydedilir ve seçtiğiniz bir harf ile etiketlenir. Her veri diski 4095 GB'lık maksimum kapasiteye sahiptir. Bunu ve depolama türünü ekleyebilirsiniz kaç veri diskinin diskleri barındırmak için kullanabileceğiniz sanal makinenin boyutunu belirler.

> [!NOTE]
> Sanal makineler kapasiteler hakkında daha fazla bilgi için bkz. [Linux sanal makine boyutları](./sizes.md).

Bir görüntüden sanal makine oluşturduğunuzda azure, bir işletim sistemi diski oluşturur. Veri diskleri içeren bir görüntü kullanırsanız, sanal makine oluşturduğunda, Azure veri diskleri de oluşturur. Aksi takdirde, sanal makineyi oluşturduktan sonra veri diski ekleyin.

Veri diskleri için sanal makine herhangi bir zamanda göre ekleyebileceğiniz **ekleme** sanal makineye disk. Karşıya yüklenen veya depolama hesabınız ya da Azure sizin için oluşturduğu bir kopyalanan VHD kullanabilirsiniz. Bir veri diski eklemeyi hala bağlıyken depolamadan silinemez şekilde VHD'de 'kira' yerleştirerek VHD dosyasını VM ile ilişkilendirir.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Bir diski](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) VM'niz için ek depolama alanı eklemek için.
* [Anlık görüntü oluşturma](snapshot-copy-managed-disk.md).
* [Yönetilen disklere dönüştürme](convert-unmanaged-to-managed-disks.md).

