---
title: Azure PowerShell komut dosyası örneği - uygulamaların yüksek kullanılabilirlik için trafiği yönlendirme | Microsoft Docs
description: Azure PowerShell komut dosyası örneği - uygulamaların yüksek kullanılabilirlik için trafiği yönlendirme
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: kumud
ms.openlocfilehash: 13c24c31606d99f27ed2607fec71b381160624dd
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
ms.locfileid: "32313058"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-powershell"></a>Azure PowerShell kullanılarak uygulamaların yüksek kullanılabilirlik için trafiği yönlendirme

Bu komut dosyası, bir kaynak grubu, iki uygulama hizmeti planları, iki web uygulamaları, trafik Yöneticisi profili ve iki trafik Yöneticisi uç noktaları oluşturur. Birincil bölge uygulamada kullanılamadığında trafik Yöneticisi bir bölgede birincil bölge olarak uygulama ve ikincil bölge trafiğini yönlendirir. Komut yürütülmeden önce Azure arasında benzersiz değerler mywebapp şeklindedir, MyWebAppL1 ve MyWebAppL2 değerleri değiştirmelisiniz. Betiği çalıştırdıktan sonra uygulama URL'si mywebapp.trafficmanager.net ile birincil bölgede erişebilir.

Gerekirse, [Azure PowerShell kılavuzunda](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup1
Remove-AzureRmResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, web uygulaması, traffic manager profili ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu grubu gibidir. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Uygulama hizmeti planı içinde Azure web uygulaması oluşturur. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Uygulama hizmeti planı içinde Azure web uygulaması oluşturur. |
| [AzureRmTrafficManagerProfile yeni](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile) | Bir Azure Traffic Manager profili oluşturur. |
| [AzureRmTrafficManagerEndpoint yeni](/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerendpoint) | Bir uç noktası için bir Azure Traffic Manager profili ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell komut dosyası örnekleri bulunabilir [Azure ağ genel görünümü belgelerine](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
