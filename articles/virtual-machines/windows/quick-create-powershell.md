---
title: Hızlı Başlangıç - Azure PowerShell ile Windows VM Oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure PowerShell’i kullanarak Windows sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b199d031d4aaef4c1a707199f5692c86543e6360
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38767213"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-azure-with-powershell"></a>Hızlı Başlangıç: PowerShell ile Azure'da Windows sanal makinesi oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülü kullanarak Azure’da Windows Server 2016 çalıştıran bir sanal makinenin (VM) nasıl dağıtılacağı gösterilir. VM’ye RDP oluşturup IIS web sunucusunu yükleyerek VM’nizin çalıştığını görebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile bir sanal makine oluşturun. Kaynakların her biri için ad sağladığınızda, [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet’i henüz mevcut değilse bunları oluşturur.

İstendiğinde, VM için oturum açma bilgileri olarak kullanılacak bir kullanıcı adı ve parola sağlayın:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389
```

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Dağıtım tamamlandıktan sonra RDP VM'ye bağlanır. VM'nizin çalışmasını görmek için, IIS web sunucusu yüklenir.

VM'nin genel IP adresini görmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) cmdlet'ini kullanın:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

Yerel bilgisayarınızdan bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. IP adresini, sanal makinenizin genel IP adresi ile değiştirin. İstendiğinde, VM’yi oluştururken kullandığınız kimlik bilgilerini girin:

```powershell
mstsc /v:publicIpAddress
```

## <a name="install-web-server"></a>Web sunucusunu yükleyin

Sanal makinenizin çalıştığını görmek için IIS web sunucusunu yükleyin. VM’de bir PowerShell istemi açın ve şu komutu çalıştırın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

İşiniz bittiğinde, RDP’nin sanal makine bağlantısını kapatın.

## <a name="view-the-web-server-in-action"></a>Web sunucusunu iş başında görün

Sanal makinenizde İnternet’ten IIS yüklenmiş ve 80 numaralı bağlantı noktası açık olduğunda, varsayılan IIS karşılama sayfasını görüntülemek için tercih ettiğiniz bir web tarayıcısını kullanın. VM’nizin önceki bir adımda edinilen genel IP adresini kullanın. Aşağıdaki örnekte varsayılan IIS web sitesi gösterilir:

![Varsayılan IIS sitesi](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, basit bir sanal makine dağıttınız, web trafiği için bir ağ bağlantı noktası açtınız ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
