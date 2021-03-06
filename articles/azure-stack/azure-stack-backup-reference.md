---
title: Azure Stack altyapısını yedekleme hizmeti başvurusu | Microsoft Docs
description: Bu makale, Azure Stack altyapısını yedekleme hizmeti için başvuru bilgileri içerir.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/20/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 608f3043e0e4b851820274ca743cbc44d1c8c0f1
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242912"
---
# <a name="infrastructure-backup-service-reference"></a>Altyapı Backup hizmeti başvurusu

## <a name="azure-backup-infrastructure"></a>Azure Yedekleme Altyapısı

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack portal, Azure Resource Manager'ı oluşturan çok sayıda hizmetten oluşur ve altyapı yönetimi deneyimi. Azure Stack Gereci gibi yönetim deneyimini kullanıma sunulan çözüm işlecine karmaşıklığı azaltmak üzerinde odaklanır.

Altyapı yedeklemeyi yedekleme karmaşıklığı internalize için tasarlanmıştır ve altyapı hizmetleri için verilerin geri yüklenmesi, işleçler sağlayarak çözümü yönetme ve kullanıcılar için bir SLA'yı tutarak odaklanabilirsiniz.

Dış bir paylaşıma yedekleme verileri dışarı aktarma, yedeklemeler aynı sistemde depolanmasını önlemek için gereklidir. Yönetici, bir dış paylaşım gerektiren var olan şirket BC/DR ilkelerine bağlı olarak verilerin depolanacağı konumu belirlemek için esnekliği sunar. 

### <a name="infrastructure-backup-components"></a>Altyapı Backup bileşenleri

Yedekleme Altyapısı aşağıdaki bileşenleri içerir:

 - **Yedekleme Altyapısı denetleyici**  
 Yedekleme Altyapısı Denetleyici ile örneği ve her Azure Stack bulutunda yer alan.
 - **Yedekleme kaynak sağlayıcısı**  
 Yedekleme kaynak sağlayıcısı (Yedekleme RP) için Azure Stack altyapısının temel yedekleme işlevselliği kullanıma sunma kullanıcı arabirimi ve uygulama programı arabirimleri (API) s oluşur.

#### <a name="infrastructure-backup-controller"></a>Yedekleme Altyapısı denetleyici

Altyapı yedekleme hizmeti için bir Azure Stack bulut örneği bir Service Fabric denetleyicisidir. Yedekleme kaynakları bölgesel düzeyi ve yakalama bölgeye özgü verileri bir hizmet AD, CA, Azure Kaynak Yöneticisi'nden en oluşturulur CRP, SRP, NRP, anahtar kasası, RBAC. 

### <a name="backup-resource-provider"></a>Yedekleme kaynak sağlayıcısı

Yedekleme kaynak sağlayıcısı, Azure Stack portalında temel yapılandırmasını ve yedekleme kaynaklar listesi için kullanıcı arabirimi sunar. İşleci kullanıcı arabiriminin aşağıdaki işlemleri gerçekleştirebilirsiniz:

 - Dış depolama konumu, kimlik bilgilerini ve şifreleme anahtarı sağlayarak ilk kez yedeklemeyi etkinleştirme
 - Yedekleme ve durum kaynaklarının oluşturma'nın altında tamamlandı görünümü oluşturan
 - Burada yedekleme verilerini yedekleme denetleyicisi yerleştirir depolama konumu değiştirin
 - Yedekleme denetleyicisi dış depolama konumuna erişmek için kullandığı kimlik bilgilerini değiştirme
 - Yedekleme denetleyicisi yedeklemeleri şifrelemek için kullanılan şifreleme anahtarını değiştir 


## <a name="backup-controller-requirements"></a>Yedekleme denetleyicisi gereksinimleri

Bu bölümde altyapı yedeklemesine önemli gereksinimleri açıklanmaktadır. Azure Stack Örneğiniz için yedeklemeyi etkinleştirme ve ardından geri gerektiğinde dağıtım ve sonraki işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

Gereksinimleri şunları içerir:

  - **Yazılım gereksinimleri** – desteklenen depolama konumlarını ve boyutlandırma kılavuzluğu açıklar. 
  - **Ağ gereksinimleri** – farklı depolama konumları için ağ gereksinimlerini açıklar.  

### <a name="software-requirements"></a>Yazılım gereksinimleri

#### <a name="supported-storage-locations"></a>Desteklenen depolama konumları

| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilen ağ ortamında bir depolama cihazı üzerinde barındırılan SMB dosya paylaşımı | SMB, Azure Stack dağıtıldığı aynı veri merkezinde veya farklı bir veri merkezinde paylaşın. Azure Stack birden fazla aynı dosya paylaşımını kullanabilirsiniz. |
| Azure'da SMB dosya paylaşımı                                                          | Şu anda desteklenmiyor.                                                                                                                                 |
| Azure BLOB Depolama                                                            | Şu anda desteklenmiyor.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Desteklenen SMB sürümleri

| SMB | Sürüm |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Depolama konumu boyutlandırma 

Altyapı yedekleme denetleyicisi verilerini isteğe bağlı olarak yedekler. En son iki gün ve canlı en fazla yedi güne kadar yedek yedeklemek için önerilir. 

| Ortam ölçek | Yedekleme tahmini boyutu | Toplam gereken alan miktarı |
|-------------------|--------------------------|--------------------------------|
| 4-12 düğümleri        | 10 GB                     | 140 GB                          |

### <a name="network-requirements"></a>Ağ gereksinimleri
| Depolama konumu                                                                 | Ayrıntılar                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Güvenilen ağ ortamında bir depolama cihazı üzerinde barındırılan SMB dosya paylaşımı | Azure Stack örneği bağlantısıyla bir ortamda yer alıyorsa, 445 bağlantı noktası gereklidir. Altyapı yedekleme denetleyicisi bağlantı noktası 445 üzerinden SMB dosya sunucusu için bir bağlantı başlatır. |
| Dosya sunucusu FQDN'si kullanmak için ad CESARETLENDİRİCİ çözülebilir olması gerekir             |                                                                                                                                                                                         |

> [!Note]  
> Hiçbir gelen bağlantı noktalarının açılması gerekir.


## <a name="infrastructure-backup-limits"></a>Yedekleme Altyapısı sınırları

Planlama, dağıtma ve Microsoft Azure Stack örneklerinizin çalışmak gibi bu sınırları göz önünde bulundurun. Bu sınırlar aşağıdaki tabloda açıklanmaktadır.

### <a name="infrastructure-backup-limits"></a>Altyapı yedekleme sınırları
| Sınır tanımlayıcı                                                 | Sınır        | Yorumlar                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Yedekleme türü                                                      | Yalnızca tam    | Altyapı yedekleme denetleyicisi, yalnızca tam yedeklemeleri destekler. Artımlı yedeklemeler desteklenmez.                                          |
| Zamanlanmış yedeklemeler                                                | Yalnızca el ile  | Yedekleme denetleyicisi şu anda yalnızca isteğe bağlı yedeklemeleri destekler                                                                                 |
| En fazla eş zamanlı yedekleme işleri                                   | 1            | Yedekleme denetleyici örneği başına yalnızca bir etkin yedek iş desteklenir.                                                                  |
| Ağ anahtarı yapılandırmasının                                     | Kapsam içinde değil | Yönetici, OEM araçlar kullanarak ağ anahtarı yapılandırmasının yedeklemeniz gerekir. Azure Stack her OEM satıcısı tarafından sağlanan belgelere bakın. |
| Donanım yaşam döngüsü konak                                          | Kapsam içinde değil | Yönetici, donanım yaşam döngüsü OEM araçlarını kullanarak konak yedeklemeniz gerekir. Azure Stack her OEM satıcısı tarafından sağlanan belgelere bakın.      |
| En fazla sayıda dosya paylaşımı                                    | 1            | Yalnızca bir dosya paylaşımını yedekleme verilerini depolamak için kullanılabilir                                                                                        |
| Uygulama Hizmetleri, işlev, SQL, mysql kaynak sağlayıcı verilerini yedekleme | Kapsam içinde değil | Dağıtma ve yönetme değeri-Microsoft tarafından oluşturulan RPs eklemek için yayımlanan kılavuzuna bakın.                                                  |
| Yedekleme üçüncü taraf kaynak sağlayıcıları                              | Kapsam içinde değil | Dağıtma ve yönetme değeri-üçüncü taraf satıcıları ile oluşturulan RPs eklemek için yayımlanan kılavuzuna bakın.                                          |

## <a name="next-steps"></a>Sonraki adımlar

 - Altyapı Backup hizmeti hakkında daha fazla bilgi için bkz: [altyapı Backup hizmeti ile Azure Stack için yedekleme ve veri kurtarma](azure-stack-backup-infrastructure-backup.md).