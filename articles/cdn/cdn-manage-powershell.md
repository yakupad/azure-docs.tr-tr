---
title: PowerShell ile Azure CDN'yi yönetme | Microsoft Docs
description: Azure CDN'yi yönetmek için Azure PowerShell cmdlet'lerini kullanmayı öğrenin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: mazha
ms.openlocfilehash: 15feb7b1d2873bc3f088eaad78079df2e063d73b
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114081"
---
# <a name="manage-azure-cdn-with-powershell"></a>PowerShell ile Azure CDN'yi yönetme
PowerShell, Azure CDN profili ve uç noktaları yönetmek için en esnek yöntemi sağlar.  Yönetim görevlerini otomatik hale getirmek için etkileşimli olarak veya komut dosyası yazma PowerShell kullanabilirsiniz.  Bu öğretici, birkaç gösterir PowerShell ile Azure CDN profili ve uç noktaları yönetmek için en yaygın görevleri gerçekleştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Azure CDN profili ve uç noktaları yönetmek için PowerShell'i kullanma hakkında bilgi için Azure PowerShell Modülü yüklü olmalıdır.  Azure PowerShell'i yükleme ve Azure kullanarak bağlanma hakkında bilgi edinmek için `Connect-AzureRmAccount` cmdlet'ini bkz [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

> [!IMPORTANT]
> İle oturum açmanız gerekir `Connect-AzureRmAccount` önce Azure PowerShell cmdlet'lerini çalıştırabilirsiniz.
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a>Azure CDN cmdlet öğelerini listeleme
Azure CDN cmdlet'leri kullanarak tüm listeleyebilirsiniz `Get-Command` cmdlet'i.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Yardım alma
Kullanarak bu cmdlet'lerden herhangi birini ile Yardım alabilirsiniz `Get-Help` cmdlet'i.  `Get-Help` Kullanım ve söz dizimi sağlar ve isteğe bağlı olarak örnekleri gösterir.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Listenin mevcut Azure CDN profilleri
`Get-AzureRmCdnProfile` Cmdlet'i parametre olmadan, tüm mevcut CDN profili alır.

```powershell
Get-AzureRmCdnProfile
```

Bu çıkış, numaralandırma cmdlet'lerinin ayrıştırılabilir.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "Standard_Verizon" }
```

Ayrıca, profil adı ve kaynak grubu belirterek tek bir profil döndürebilir.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> Farklı kaynak gruplarında bulunan oldukları sürece aynı ada sahip birden çok CDN profili olması mümkündür.  Atlama `ResourceGroupName` parametresi eşleşen bir ada sahip tüm profilleri döndürür.
> 
> 

## <a name="listing-existing-cdn-endpoints"></a>Listenin mevcut CDN uç noktası
`Get-AzureRmCdnEndpoint` tek bir uç nokta veya bir profildeki tüm uç noktalar alabilirsiniz.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>CDN profili ve uç noktaları oluşturma
`New-AzureRmCdnProfile` ve `New-AzureRmCdnEndpoint` CDN profili ve uç noktaları oluşturmak için kullanılır. Aşağıdaki SKU'ları desteklenir:
- Standard_Verizon
- Premium_Verizon
- Custom_Verizon
- Standard_Akamai
- Standard_ChinaCdn

> [!NOTE]
> Önizleme aşamasında olduğu sürece Standard_Microsoft SKU desteklenmiyor.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku Standard_Akamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Uç nokta adın kullanılabilirliği denetleniyor
`Get-AzureRmCdnEndpointNameAvailability` bir uç nokta adı kullanılabilir olup olmadığını gösteren bir nesne döndürür.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Özel bir etki alanı ekleme
`New-AzureRmCdnCustomDomain` özel etki alanı için mevcut bir uç nokta ekler.

> [!IMPORTANT]
> CNAME DNS sağlayıcınızda açıklanan şekilde ayarlamanız gerekir [nasıl özel etki alanını Content Delivery Network (CDN) uç noktasına eşleme](cdn-map-content-to-custom-domain.md).  Uç noktayı kullanarak değiştirmeden önce eşleme sınayabilirsiniz `Test-AzureRmCdnCustomDomain`.
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Bir uç nokta değiştirme
`Set-AzureRmCdnEndpoint` Varolan bir uç noktada değiştirir.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Temizleme/öncesi-loading CDN varlıklar
`Unpublish-AzureRmCdnEndpointContent` temizler önbelleğe alınan varlıkları sırada `Publish-AzureRmCdnEndpointContent` desteklenen uç noktasında varlıkları önceden yükler.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Başlatma/durdurma CDN uç noktası
`Start-AzureRmCdnEndpoint` ve `Stop-AzureRmCdnEndpoint` başlatıp tekil uç noktalarını ya da uç noktalar grupları durdurmak için kullanılabilir.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN kaynakları silme
`Remove-AzureRmCdnProfile` ve `Remove-AzureRmCdnEndpoint` profillerini ve uç noktalarını kaldırmak için kullanılabilir.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Sonraki Adımlar
Azure CDN’yi [.NET](cdn-app-dev-net.md) veya [Node.js](cdn-app-dev-node.md) ile nasıl otomatik hale getireceğinizi öğrenin.

CDN özellikleri hakkında bilgi edinmek için [CDN'ye genel bakış](cdn-overview.md).

