---
title: Yeniden başlatma veya yeniden boyutlandırma VM sorunları Azure'da | Microsoft Docs
description: Yeniden başlatmadan veya mevcut bir Windows sanal makine azure'da yeniden boyutlandırma Resource Manager dağıtım sorunlarını giderme
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 11/03/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 482a194535cba8715b70ecff4679691515b206a9
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
ms.locfileid: "27581405"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Yeniden başlatmadan veya varolan bir Windows VM azure'da yeniden boyutlandırma ile dağıtım sorunlarını giderme
Durdurulmuş bir Azure sanal makine (VM) Başlat veya mevcut bir Azure VM'yi yeniden boyutlandırmak çalıştığınızda karşılaştığınız ortak bir ayırma hatası hatadır. Küme veya bölgede kullanılabilir kaynak yok veya istenen VM boyutu destekleyemez olduğunda bu hata oluşur.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Toplama etkinliklerini günlüğe kaydeder
Sorun giderme başlatmak için sorunu ile ilişkili hata tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar süreci hakkında ayrıntılı bilgiler içerir:

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarınızı yönetmek için etkinlik günlüklerini görüntüle](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: durmuş bir VM'yi başlatma sırasında hata
Durmuş bir VM'yi Başlat ancak alma ayırma hatası deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmeti barındıran özgün kümesine denenmesi durdurulmuş VM başlatmak için bir istek aldı. Ancak, Küme isteği gerçekleştirmek kullanılabilir boş disk alanı yok.

### <a name="resolution"></a>Çözüm
* Kullanılabilirlik kümesindeki tüm sanal makineleri durdurmak ve ardından her VM yeniden başlatın.
  
  1. Tıklatın **kaynak grupları** > *kaynak grubunuz* > **kaynakları** > *kullanılabilirlik kümesi* > **sanal makineleri** > *sanal makineniz* > **durdurmak**.
  2. Tüm sanal makineleri durdurduktan sonra durdurulmuş VM'lerin her birinde seçin ve Başlat'ı tıklatın.
* Yeniden başlatma isteği daha sonra yeniden deneyin.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: mevcut bir VM'yi yeniden boyutlandırılırken hata
Mevcut bir VM'yi yeniden boyutlandırın, ancak bir ayırma hatası almak deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmeti barındıran özgün kümesine denenmesi VM yeniden boyutlandırmak için bir istek aldı. Ancak, küme istenen VM boyutu desteklemez.

### <a name="resolution"></a>Çözüm
* Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
* İstenen VM boyutu değiştirilemiyorsa:
  
  1. Kullanılabilirlik kümesindeki tüm sanal makineleri durdurun.
     
     * Tıklatın **kaynak grupları** > *kaynak grubunuz* > **kaynakları** > *kullanılabilirlik kümesi* > **sanal makineleri** > *sanal makineniz* > **durdurmak**.
  2. Tüm sanal makineleri durdurduktan sonra daha büyük bir boyutu için istenen VM'yi yeniden boyutlandırın.
  3. Yeniden boyutlandırılan VM seçin ve tıklatın **Başlat**, ve durdurulmuş VM'lerin her birinde başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni bir Windows VM oluşturduğunuzda sorunlarıyla karşılaşırsanız bkz [Azure'da yeni bir Windows sanal makine oluşturma ile dağıtım sorunlarını giderme](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

