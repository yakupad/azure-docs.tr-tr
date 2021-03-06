---
title: Azure yığın Geliştirme Seti temel kavramları | Microsoft Docs
description: Azure yığın Geliştirme Seti (ASDK) için temel yönetim görevlerini gerçekleştirmek açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: fa2ee4df9a0a28c08237f30fcf6a4bac50c21100
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849517"
---
# <a name="asdk-administration-basics"></a>ASDK Yönetimi temelleri 
Azure yığın Geliştirme Seti (ASDK) yönetim için yeni bilmeniz gereken birkaç nokta vardır. Bu kılavuz rolünüze değerlendirme ortamı Azure yığın işlecinde olarak genel bir bakış sağlar ve test kullanıcılarınızın emin olmak nasıl hızla üretken olabilirsiniz.

İlk olarak, gözden geçirmeniz gereken [Azure yığın Geliştirme Seti nedir?](asdk-what-is.md) makale amacını ASDK ve kendi kısıtlamaları anladığınızdan emin olun. Burada, Azure geliştirmek ve uygulamalarınızı bir üretim dışı ortamda test etmek için yığın değerlendirebilmeniz bir "korumalı," Geliştirme Seti kullanmanız gerekir. 

Biz düzenli olarak ASDK, yeni derlemeler yayın böylece Azure gibi Azure yığın hızlı bir şekilde innovates. Ancak, Azure tümleşik yığını sistemleri dağıtımları gibi ASDK yükseltemezsiniz. En son sürüme taşımak istiyorsanız, bu nedenle, tamamen gerekir [ASDK dağıtmanız](asdk-redeploy.md). Güncelleştirme paketleri uygulanamıyor. Bu işlem zaman alır, ancak kullanılabilir durumda olduklarında hemen sonra en son özellikleri denemek avantajdır. 

## <a name="what-account-should-i-use"></a>Hangi hesabı kullanmalıyım?
Azure yığın yönetirken bilmeniz gereken birkaç hesabında dikkate alınacak noktalar vardır. Özellikle dağıtımlarda, kimlik sağlayıcısı Azure Active Directory (Azure AD) yerine Windows Server Active Directory Federasyon Hizmetleri (AD FS) kullanarak. Aşağıdaki hesabında dikkate alınacak noktalar hem Azure tümleşik yığını sistemleri hem de ASDK dağıtımları için geçerlidir:

|Hesap|Azure AD|AD FS|
|-----|-----|-----|
|Yerel yönetici (. \Administrator)|ASDK ana bilgisayar yöneticisi|ASDK ana bilgisayar yöneticisi|
|AzureStack\AzureStackAdmin|ASDK ana bilgisayar yöneticisi<br><br>Azure yığın Yönetim Portalı'nda oturum için kullanılabilir<br><br>Görüntülemek ve Service Fabric çalma yönetmek için erişim|ASDK ana bilgisayar yöneticisi<br><br>Azure yığın yönetim portalı için erişim yok<br><br>Görüntülemek ve Service Fabric çalma yönetmek için erişim<br><br>Artık sahibi varsayılan sağlayıcı abonelik (DPS)|
|AzureStack\CloudAdmin|Erişebilir ve ayrıcalıklı Endpoint içinde izin verilen komutlarını çalıştırın|Erişebilir ve ayrıcalıklı Endpoint içinde izin verilen komutlarını çalıştırın<br><br>ASDK ana bilgisayara oturum değil<br><br>Varsayılan sağlayıcı aboneliğin (DPS) sahibi|
|Azure AD genel Yöneticisi|Yükleme sırasında kullanılan<br><br>Varsayılan sağlayıcı aboneliğin (DPS) sahibi|Uygulanamaz|
|

## <a name="what-tools-do-i-use-to-manage"></a>Yönetmek için hangi Araçlar kullanıyor?
Kullanabileceğiniz [Azure yığın Yönetici portalı](https://adminportal.local.azurestack.external) veya Azure yığın yönetmek için PowerShell. Portalı aracılığıyla temel kavramları öğrenmeniz en kolay yoludur. PowerShell kullanmak istiyorsanız, yüklemenize gerek [Azure yığını için PowerShell](asdk-post-deploy.md#install-azure-stack-powershell) ve [Azure yığın araçları Github'dan indirdiğinizde](asdk-post-deploy.md#download-the-azure-stack-tools).

Azure yığın Azure Resource Manager, temel alınan dağıtım, yönetim ve kuruluş mekanizması olarak kullanır. Azure yığın yönetmek ve kullanıcıların desteklemeye yardımcı olmak için kullanacaksanız, Azure Resource Manager hakkında bilgi edinin. Okuyarak daha fazla bilgi edinebilirsiniz [Azure Resource Manager teknik ile çalışmaya başlama](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf).

## <a name="your-typical-responsibilities"></a>Tipik sizin Sorumluluklarınız
Kullanıcılarınızın hizmetleri kullanmak istiyorsunuz. Kendi açısından, ana hizmetlerin bunları kullanılabilir hale getirmek için rolüdür. ASDK kullanarak sunmak için hangi hizmetlerin öğrenin ve services tarafından kullanılabilir hale getirmeniz nasıl [planları, teklifleri ve kotalar oluşturma](asdk-offer-services.md). Sanal makine görüntüleri gibi Market öğelerini eklemek gerekir. En kolay yolu [Market öğesi karşıdan](asdk-marketplace-item.md) Azure yığınına azure'dan.

> [!NOTE]
> Test planları, teklifleri ve Hizmetleri istiyorsanız, kullanması gereken [kullanıcı portalı](https://portal.local.azurestack.external); değil [Yönetici portalı](https://adminportal.local.azurestack.external).

Hizmetleri sağlamaya ek olarak, bir Azure yığın işlecinin tüm normal görevleri ASDK hazır ve çalışır tutmak için gerçekleştirmeniz gerekir. Bu görevleri aşağıdaki gibi öğeleri içerir:
- Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) dağıtımları için kullanıcı hesaplarını ekleyin.
- Rol tabanlı erişim denetimi (RBAC) rolleri (Bu yalnızca yöneticiler tarafından sınırlı değil) atayın
- İzleyici altyapı durumu
- Ağ ve depolama kaynaklarını yönetme
- Başarısız Geliştirme Seti ana bilgisayar donanımı değiştirme 

## <a name="where-to-get-support"></a>Destek almak nereye
Geliştirme Seti için yalnızca destek içinde desteği ile ilgili sorular sormak için seçenektir [Microsoft Azure yığın Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Yönetici portalı'nı sağ üst köşesinde Yardım ve Destek (soru işareti) simgesine tıklayın ve ardından, **yeni destek isteği**, forumlar sitenin doğrudan açılır. Bu forumları düzenli olarak izlenir. 

> [!IMPORTANT]
> ASDK bir değerlendirme ortamı olduğundan, Microsoft Müşteri Destek Hizmetleri'ne (CSS) aracılığıyla sunulan resmi desteği yoktur.

## <a name="next-steps"></a>Sonraki adımlar
[ASDK dağıtma](asdk-install.md)

