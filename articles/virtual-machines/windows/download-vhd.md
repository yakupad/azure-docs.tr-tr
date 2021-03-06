---
title: VHD Windows Azure'dan karşıdan | Microsoft Docs
description: Windows Azure portalını kullanarak VHD indirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: f62c1b815180e39468a39b8bc2a220a6bfb9ea5a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726304"
---
# <a name="download-a-windows-vhd-from-azure"></a>VHD Windows Azure'dan indirin

Bu makalede, nasıl yükleneceği hakkında bilgi edineceksiniz bir [Windows sanal sabit disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure portalını kullanarak Azure dosyasından. 

## <a name="stop-the-vm"></a>VM’yi durdurma

Çalışan bir VM bağlıysa VHD Azure'dan indirilemiyor. Bir VHD yüklemek için VM durdurmanız gerekir. Bir VHD'yi kullanılabilir olarak kullanmak istiyorsanız bir [görüntü](tutorial-custom-images.md) diğer VM'ler ile yeni diskler oluşturmak için kullandığınız [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) dosyada bulunan işletim sistemini genelleştirir ve VM'yi durdurun. VHD'yi yeni bir örneğini bir var olan VM veya veri diski için disk olarak kullanmak için yalnızca durdurun ve VM ayırması gerekir.

VHD diğer sanal makineleri oluşturmak için bir resim olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2.  [VM'ye bağlanın](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
3.  VM, bir yönetici olarak komut istemi penceresi açın.
4.  Dizinine değiştirin *%windir%\system32\sysprep* ve sysprep.exe çalıştırın.
5.  Sistem Hazırlama Aracı iletişim kutusunda seçin **girin sistem Out-of-Box deneyimi (OOBE)**, emin olun **Generalize** seçilir.
6.  Kapatma seçenekleri belirleyin **kapatma**ve ardından **Tamam**. 

VHD'yi yeni bir örneğini bir var olan VM veya veri diski için disk olarak kullanmak için aşağıdaki adımları tamamlayın:

1.  Azure portalında Hub menüsünde **sanal makineleri**.
2.  VM listeden seçin.
3.  VM için dikey penceresinde **durdurmak**.

    ![VM'yi Durdur](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>SAS URL oluştur

VHD dosyasını indirmek için oluşturmak gereken bir [paylaşılan erişim imzası (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL. URL oluşturulduğunda, sona erme süresi URL atanır.

1.  VM için dikey pencerenin menüsünde **diskleri**.
2.  VM için işletim sistemi diski seçin ve ardından **verme**.
3.  Sona erme süresini URL'sini ayarlayın *36000*.
4.  Tıklatın **URL'yi oluşturmak**.

    ![URL'yi oluşturmak](./media/download-vhd/export-generate.png)

> [!NOTE]
> Bir Windows Server işletim sistemi için büyük VHD dosyasını karşıdan yüklemek için yeterli süre sağlamak için varsayılan süre artar. Bağlantınızı bağlı olarak yüklemek için birkaç saat sürebilir Windows Server işletim sistemini içeren bir VHD dosyasının bekleyebilirsiniz. Veri diski için bir VHD yüklüyorsanız, varsayılan saat yeterli olur. 
> 
> 

## <a name="download-vhd"></a>VHD indirin

1.  Oluşturulan URL altında VHD dosyasını yükle'yi tıklatın.

    ![VHD indirin](./media/download-vhd/export-download.png)

2.  ' İ tıklatmanız gerekir **kaydetmek** karşıdan yüklemeyi başlatmak için tarayıcıda. VHD dosyası için varsayılan ad *abcd*.

    ![Tarayıcıda Kaydet'e tıklayın.](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [VHD dosyasını Azure'a yüklemeniz](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
- [Bir depolama hesabı yönetilmeyen disklerden yönetilen diskleri oluşturma](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- [PowerShell ile Azure diskleri yönetme](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

