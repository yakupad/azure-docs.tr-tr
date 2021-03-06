---
title: Visual Studio kullanarak bir Azure sanal makinesinde | Microsoft Docs
description: Visual Studio, Azure sanal makinesinde kullanma.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: sacalla
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 03/02/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: 2c8cd5cb837787db7bd6cb0d76bdf734b7d7bc0f
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285383"
---
# <a name="visual-studio-images-on-azure"></a>Azure'da Visual Studio görüntüleri
Önceden yapılandırılmış Azure sanal makineler'de (VM) Visual Studio kullanarak herhangi bir şey bir yukarı ve çalışan geliştirme ortamına gitmek için hızlı ve kolay bir yoludur. Sistem görüntüleri farklı Visual Studio yapılandırmalarıyla kullanılabilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?search=%22visual%20studio%202017%22&page=1).

Azure’da yeni misiniz? [Ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Hangi yapılandırmaları ve sürümlerinin kullanılabilir mi?
En son ana sürümleri, Visual Studio 2017 ve Visual Studio 2015 için görüntüleri Azure Market'te bulunabilir. Her bir ana sürüm için yayınlanmış (RTW) sürümünü ve en son güncelleştirilmiş sürümlerine bakın. Bu sürümlerin her biri, Visual Studio Enterprise ve Visual Studio Community sürümleri sunar. Bu görüntüler, en son Visual Studio ve Windows güncelleştirmelerini dahil etmek için en az her ay güncelleştirilir. Her görüntünün açıklaması, yüklü bir ürün sürümü ve görüntünün "Başlangıç" tarihi görüntüleri adları aynı kalsa da içerir.

| Yayın sürümü                                              | Sürümleri                     |     Ürün sürümü     |
|:------------------------------------------------------------:|:----------------------------:|:-----------------------:|
| Visual Studio 2017: En son (sürüm 15.7)                    |    Kurumsal ve topluluk     |      Sürüm 15.7.5     |
| Visual Studio 2017: En son Önizleme (sürüm 15,8, Preview 5) |    Kurumsal ve topluluk     |      Sürüm 15.8.5     |
|         Visual Studio 2017: RTW                              |    Kurumsal ve topluluk     |      Sürüm 15.0.15    |
|   Visual Studio 2015: En son (Aktualizace 3)                      |    Kurumsal ve topluluk     |  Sürüm 14.0.25431.01  |
|         Visual Studio 2015: RTW                              |             None             | (Bakım için süresi doldu) |

> [!NOTE]
> Hizmet İlkesi Microsoft uygun olarak hizmet vermek için Visual Studio 2015'in yayınlanmış (RTW) sürüm süresi doldu. Visual Studio 2015 güncelleştirme 3 için Visual Studio 2015 ürün sınıfıyla sunulan yalnızca kalan sürümüdür.

Daha fazla bilgi için [Visual Studio Hizmet İlkesi](https://www.visualstudio.com/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Hangi özellikleri yüklenir?
Bu Visual Studio sürümü için önerilen özelliklere her görüntü içerir. Genel olarak, yüklemeyi içerir:

* Önerilen tüm kullanılabilir iş yükleri, her iş yükünün dahil olmak üzere isteğe bağlı bileşenler
* .NET 4.6.2 ve .NET 4.7 geliştirici araçları SDK'ları ve hedefleme paketleri
* Visual F #
* Visual Studio için GitHub uzantısı
* LINQ to SQL araçları

Görüntüleri oluştururken Visual Studio'yu yüklemek için kullanılan komut satırı aşağıdaki gibidir:

```
    vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
       add Microsoft.Net.Component.4.7.SDK ^
       add Microsoft.Net.Component.4.7.TargetingPack ^ 
       add Microsoft.Net.Component.4.6.2.SDK ^
       add Microsoft.Net.Component.4.6.2.TargetingPack ^
       add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
       add Microsoft.VisualStudio.Component.FSharp ^
       add Component.GitHub.VisualStudio ^
       add Microsoft.VisualStudio.Component.LinqToSql
```

Görüntüleri gerektiren bir Visual Studio özelliği dahil etmezseniz, sayfanın sağ üst köşesindeki geri bildirim aracında aracılığıyla geri bildirim sağlayın.

## <a name="what-size-vm-should-i-choose"></a>VM boyutu seçmem gerekir?
Azure sanal makine boyutları sunar. Visual Studio, güçlü, çok iş parçacıklı bir uygulama olduğundan, 7 GB bellek ve en az iki işlemcileri içeren bir VM boyutu istersiniz. Visual Studio görüntüleri için aşağıdaki VM boyutları öneririz:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
En son makine boyutları hakkında daha fazla bilgi için bkz. [boyutları için Windows azure'da sanal makineler](/azure/virtual-machines/windows/sizes).

Azure ile ilk seçiminizi VM'yi yeniden boyutlandırmadan olarak yeniden dengeleyebilir. Daha uygun bir boyut ile yeni bir VM sağlama veya farklı bir temel alınan donanım için var olan sanal makinenizi yeniden boyutlandırma. Daha fazla bilgi için [Windows VM'yi yeniden boyutlandırma](/azure/virtual-machines/windows/resize-vm).

## <a name="after-the-vm-is-running-whats-next"></a>Sanal makine çalışmaya başladıktan sonra bu sırada ne var?
Visual Studio, azure'a "kendi lisansını getir" modelini kullanır. Bir yüklemede gibi özel bir donanım ile ilk adımlarından biri Visual Studio yüklemenizin lisanslama. Visual Studio ya da kilidini açmak için:
- Visual Studio aboneliğiyle ilişkili Microsoft hesabınızla oturum açın 
- Visual Studio ile ilk Alışverişinizi verilen ürün anahtarı kilidini aç

Daha fazla bilgi için [Visual Studio'da oturum](/visualstudio/ide/signing-in-to-visual-studio) ve [Visual Studio kilidini açma](/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Nasıl miyim gelecek için VM geliştirme kaydetme veya takım kullanıyor musunuz?

Geliştirme ortamlarını spektrumun büyük ve gerçek maliyet daha karmaşık ortamları oluşturmaya ile ilişkili. Ortamınızın yapılandırmasını bağımsız olarak kaydedebilir veya yakalama, bir "temel görüntü olarak" diğer ekip üyelerinin veya ileride kullanmak için yapılandırılan sanal makinenizin. Ardından, yeni bir sanal makine önyükleme yaparken, bunu Azure Market görüntüsü yerine temel görüntü sağlayın.

Kısa bir Özet: Sistem Hazırlama (Sysprep) aracını kullanın ve çalışan sanal makineyi ve ardından yakalamak *(Şekil 1)* Azure portalındaki kullanıcı Arabirimi aracılığıyla bir görüntü olarak VM. Azure kaydeder `.vhd` seçtiğiniz depolama hesabındaki görüntüsünü içeren dosya. Yeni görüntü sonra aboneliğinizin kaynak listesi bir görüntü kaynağı olarak görünür.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Şekil 1) Azure portalı kullanıcı Arabirimi üzerinden bir görüntüsünü yakalayın.*</center>

Daha fazla bilgi için [Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü oluşturma](/azure/virtual-machines/windows/capture-image-resource).

> [!IMPORTANT]
> VM'yi hazırlama için Sysprep kullanma unutmayın. Bu adımı atlarsanız Azure VM'den görüntü sağlayamazsınız.

> [!NOTE]
> Yine de görüntülerin depolama için bazı maliyetler doğurur, ancak bir gereksinim duyan her ekip üyesi için sıfırdan bir VM yeniden oluşturmak için yüksek maliyetleri karşılaştırıldığında maliyet artışı Önemsiz olabilir. Örneğin, oluşturmak ve takımınız tarafından yeniden kullanılabilir bir ay için 127 GB'lık görüntü depolamak için birkaç lira karşılığında maliyeti. Ancak, bu her çalışan oluşturmak ve bunların bireysel kullanıma yönelik bir düzgün bir şekilde yapılandırılmış geliştirme kutusunda doğrulamak için yatırım alışkanlıklarını saate kıyasla anlamsız ücretlerdir.

Ayrıca, geliştirme görevleri veya teknolojileri geliştirme yapılandırmaları ve birden çok makine yapılandırmaları çeşitleri gibi daha fazla ölçek gerekebilir. Azure DevTest Labs oluşturmak için kullanabileceğiniz _tarifler_ , "altın görüntü." yapımı otomatikleştirin DevTest Labs, ekibinizin çalışan VM'ler için ilkelerini yönetmek için de kullanabilirsiniz. [Geliştiriciler için Azure DevTest Labs'i kullanarak](/azure/devtest-lab/devtest-lab-developer-lab) DevTest Labs hakkında daha fazla bilgi için en iyi bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Önceden yapılandırılmış Visual Studio görüntüler hakkında artık bildiğinize göre sonraki adım yeni bir VM oluşturmaktır:

* [Azure portalından bir VM oluşturma](quick-create-portal.md)
* [Windows sanal makinelerine genel bakış](overview.md)
