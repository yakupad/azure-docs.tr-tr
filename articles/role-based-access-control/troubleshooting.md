---
title: Azure RBAC sorunlarını giderme | Microsoft Docs
description: Azure rol tabanlı erişim denetimi (RBAC) ile ilgili sorunları giderin.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: d1a0e46fe348bbc60a4d02a4727a9bb27cb26742
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223305"
---
# <a name="troubleshoot-rbac-in-azure"></a>Azure RBAC sorunlarını giderme

Bu makalede, Azure portalı ve rolleri erişimi sorunlarını giderme kullanırken beklenmesi gerekenler öğrenmek için rol tabanlı erişim denetimi (RBAC) hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="web-app-features-that-require-write-access"></a>Yazma erişimi gerektiren bir web uygulaması özellikleri

Tek bir web uygulaması için kullanıcı salt okunur erişim sağlıyorsa, beklediğiniz değil, bazı özellikler devre dışı bırakılır. Aşağıdaki yönetim özelliklerini gerektiren **yazma** (Katkıda bulunan veya sahip) bir web uygulamasına erişim ve salt okunur senaryoda kullanılamaz.

* Komutlar (örneğin, başlatma, durdurma, vs.)
* Genel yapılandırma, Ölçek ayarları, Yedekleme ayarlarını ve izleme ayarları gibi ayarlarını değiştirme
* Yayımlama kimlik bilgileri ve diğer gizli dizileri uygulama ayarlarının ve bağlantı dizeleri gibi erişme
* Akış günlükleri
* Tanılama günlüklerini yapılandırma
* Konsolu (komut istemi)
* Etkin ve son dağıtımları (için yerel git sürekli dağıtım)
* Tahmini harcama
* Web testleri
* Sanal ağ (yalnızca, bir sanal ağda yazma erişimine sahip bir kullanıcı tarafından önceden yapılandırılmış bir okuyucu için görünür).

Bu kutucuk erişemiyorsanız, web uygulamasına katkıda bulunan erişimi için yöneticinizden gerekir.

## <a name="web-app-resources-that-require-write-access"></a>Yazma erişimi gerektiren bir web uygulama kaynakları

Web apps, Etkileşim özelliği birkaç farklı kaynak varlığı karmaşık. Tipik bir kaynak grubu birkaç Web siteleri şu şekildedir:

![Web uygulaması kaynak grubu](./media/troubleshooting/website-resource-model.png)

Yalnızca web uygulaması, Azure portalında Web sitesi dikey penceresinde işlevselliğinin erişimi devre dışı vermeniz bir sonucu.

Bu öğeleri gerektiren **yazma** erişim **App Service planı** sitenize karşılık gelir:  

* Web uygulamasını görüntüleme (ücretsiz veya standart) katmanı fiyatlandırma  
* Ölçek yapılandırma (örnekleri, sanal makine boyutu, otomatik ölçeklendirme ayarları sayısı)  
* Kotalar (depolama, bant genişliği, CPU)  

Bu öğeleri gerektiren **yazma** tam erişim **kaynak grubu** sitenizi içeren:  

* SSL sertifikaları ve bağlamaları (SSL sertifikaları, coğrafi konum ve aynı kaynak grubundaki siteler arasında paylaşılabilir)  
* Uyarı kuralları  
* Otomatik ölçeklendirme ayarları  
* Application Insights bileşenleri  
* Web testleri  

## <a name="virtual-machine-features-that-require-write-access"></a>Yazma erişimi gerektiren sanal makine özellikleri

Benzer şekilde web uygulamaları, sanal makine dikey penceresinde bazı özellikler sanal makineye veya kaynak grubundaki diğer kaynaklara yazma erişimi gerektirir.

Sanal makineler, etki alanı adları, sanal ağlar, depolama hesapları ve uyarı kuralları ilgilidir.

Bu öğeleri gerektiren **yazma** erişim **sanal makine**:

* Uç Noktalar  
* IP adresleri  
* Diskler  
* Uzantılar  

Bunlar gerektirir **yazma** hem erişim **sanal makine**ve **kaynak grubu** (etki alanı adı ile birlikte) BT'nin zamanı:  

* Kullanılabilirlik kümesi  
* Yük dengeli küme  
* Uyarı kuralları  

Bu kutucuk erişemiyorsanız, kaynak grubuna katkıda bulunan erişimi için yöneticinizden isteyin.

## <a name="azure-functions-and-write-access"></a>Azure işlevleri ve yazma erişimi

Bazı özellikleri [Azure işlevleri](../azure-functions/functions-overview.md) yazma erişimi gerektirir. Okuyucu rolü atanmış bir kullanıcı, örneğin, bunlar bir işlev uygulaması işlevlerinde görüntülemek mümkün olmayacaktır. Portal görüntüler **(erişim yok)**.

![Uygulama erişimi işlevi](./media/troubleshooting/functionapps-noaccess.png)

Bağlanabilmesi **Platform özellikleri** sekmesine ve ardından **tüm ayarlar** bazı ayarları görüntülemek için ilgili bir işlev uygulaması (bir web app ile benzer), ancak bu ayarlardan herhangi birini değiştiremezler.

## <a name="rbac-changes-are-not-being-detected"></a>RBAC değişikliklerin algılanmaz

Azure Resource Manager bazen yapılandırmaları ve performansı artırmak için verileri önbelleğe alır. Rol atamaları silmesini veya yaratmasını, değişikliklerin etkili olması için 30 dakikaya kadar sürebilir. Azure portalı, Azure PowerShell veya Azure CLI kullanıyorsanız kapatıp açtıktan rol ataması değişikliklerinizi yenilemeye zorlayabilirsiniz. REST API çağrıları ile rol atamasını değişiklikler yapıyorsanız, erişim belirtecinizin yenileyerek yenilemeye zorlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [RBAC ve Azure portalı kullanarak erişimi yönetme](role-assignments-portal.md)
* [RBAC değişiklikler için etkinlik günlüklerini görüntüleme](change-history-report.md)

