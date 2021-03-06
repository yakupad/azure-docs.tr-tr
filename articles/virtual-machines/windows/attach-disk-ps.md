---
title: PowerShell kullanarak azure'da bir Windows VM'ye veri diski ekleme | Microsoft Docs
description: Windows PowerShell ile Resource Manager dağıtım modelini kullanarak bir VM için yeni veya var olan veri diski ekleme.
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
ms.date: 10/11/2017
ms.author: cynthn
ms.openlocfilehash: 384203134d1588053f91b66d32e9b0bf1ec69306
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38680924"
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>Windows PowerShell kullanarak bir VM'ye veri diski ekleme

Bu makalede hem yeni hem de mevcut diski, PowerShell kullanarak bir Windows sanal makine gösterilmektedir. 

Bunu yapmadan önce bu ipuçlarını gözden geçirin:
* Sanal makinenin boyutunu, iliştirebilirsiniz kaç veri diskinin denetler. Ayrıntılar için bkz [sanal makine boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Premium depolama kullanmak için bir Premium depolama gerekir DS serisi veya GS serisi bir sanal makine gibi VM boyutu etkin. Ayrıntılar için bkz [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 6.0.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Boş veri diski bir sanal makineye ekleyin.

Bu örnek, varolan bir sanal makineye boş veri diski ekleme gösterir.

### <a name="using-managed-disks"></a>Yönetilen diskleri kullanma

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Bir kullanılabilirlik alanında yönetilen diskleri kullanma
Bir kullanılabilirlik alanında bir disk oluşturmak için kullanın [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) ile `-Zone` parametresi. Aşağıdaki örnek, bölgesinde bir disk oluşturur *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Diski başlatın

Boş disk ekledikten sonra bunu başlatmak gerekir. Diski başlatmak için bir VM için oturum açın ve disk Yönetimi'ni kullanın. WinRM ve VM üzerindeki bir sertifika etkinleştirilirse, bunları oluşturduğunda diski başlatmak için Uzak PowerShell kullanın. Özel betik uzantısı da kullanabilirsiniz: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Betik dosyasının aşağıdaki gibi diskleri başlatmak için bu kodu içerebilir:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Bir VM'ye olan bir veri diski ekleme

Bir VM'ye veri diski olarak mevcut bir yönetilen disk ekleyebilirsiniz. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma bir [anlık görüntü](snapshot-copy-managed-disk.md).
