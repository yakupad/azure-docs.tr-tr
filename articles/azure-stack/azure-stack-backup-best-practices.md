---
title: Azure Stack için altyapı Backup hizmeti en iyi yöntemler | Microsoft Docs
description: Dağıtıp geri dönülemez bir hata varsa, veri kaybını önlemeye yardımcı olmak için veri merkezinizde Azure Stack yönettiğinizde, en iyi yöntem kümesi takip edebilirsiniz.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 221FDE40-3EF8-4F54-A075-0C4D66EECE1A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/20/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 4a0166856cf4d3f91c6cadf3e71f93000fbf158d
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242980"
---
# <a name="infrastructure-backup-service-best-practices"></a>Altyapı Backup hizmeti en iyi uygulamalar

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Dağıtıp bir arıza durumunda veri kaybını önlemeye yardımcı olmak için veri merkezinizde Azure Stack yönettiğinizde, en iyi uygulamaları takip edebilirsiniz.

En iyi uygulamalar, değişiklikler için işlem akışı yapıldığında yüklemenizi hala uyumlu olup olmadığını doğrulamak için düzenli aralıklarla gözden geçirmelidir. Bu en iyi uygulama çalışırken herhangi bir sorun karşılaşmamanız gerekir, Yardım için Microsoft Support başvurun.

## <a name="configuration-best-practices"></a>Yapılandırma en iyi uygulamalar

### <a name="deployment"></a>Dağıtım

Her Azure Stack Bulutu dağıtıldıktan sonra altyapı yedeklemeyi etkinleştirin. Tüm istemci/sunucu erişimi olan bir yedeklerden işleci yönetim API uç noktasına zamanlayabilirsiniz AzureStack araçlarını kullanma.

### <a name="networking"></a>Ağ

Yolu için Evrensel Adlandırma Kuralı (UNC) dizesi, bir tam etki alanı adı (FQDN) kullanmanız gerekir. IP adresi, ad çözümlemesi mümkün değilse mümkündür. Bir UNC dize paylaşılan dosyalarını veya cihazları gibi kaynakların konumunu belirtir.

### <a name="encryption"></a>Şifreleme

Şifreleme anahtarı, dış depolama birimine dışarı, yedekleme verilerini şifrelemek için kullanılır. Anahtar parçası olarak oluşturulan [PowerShell ile Azure Stack için yedeklemeyi etkinleştirme](azure-stack-backup-enable-backup-powershell.md).

Anahtarı güvenli bir konuma (örneğin, ortak Azure Key Vault gizli) depolanması gerekir. Azure Stack yeniden dağıtım sırasında bu anahtarı kullanılması gerekir. 

![Anahtarı güvenli bir konuma depolanır.](media\azure-stack-backup\azure-stack-backup-encryption2.png)

## <a name="operational-best-practices"></a>İşletimsel en iyi uygulamalar

### <a name="backups"></a>Yedeklemeler

 - Altyapı yedekleme denetleyicisi isteğe bağlı olarak tetiklenmesi gerekiyor. Günde en az iki kez yedekleme için önerilir.
 - Kapalı kalma süresi olmadan kullanıcı uygulamaları ve yönetim deneyimleri olduğundan sistem çalışırken yedekleme işlerini Yürüt. Yedekleme işlerinin makul yük altında bir çözüm için 20-40 dakika bekler.
 - Yönerge sağlanan OEM kullanarak, el ile yedekleme ağ anahtarları ve donanım yaşam döngüsü ana bilgisayar (HLH) burada altyapısını yedekleme denetleyicisi depoları denetim düzlemi yedekleme verilerini aynı yedekleme paylaşımında depolanması gerekir. Anahtar ve HLH yapılandırmaları bölge klasöre depolamayı düşünün. Azure Stack birden fazla aynı bölgede olması Ölçek birimine ait her bir yapılandırma için bir tanımlayıcı kullanmayı düşünün.

### <a name="folder-names"></a>Klasör adları

 - Altyapı MASBACKUP klasörü otomatik olarak oluşturur. Microsoft tarafından yönetilen bir paylaşım budur. MASBACKUP ile aynı düzeyde paylaşımları oluşturabilirsiniz. Klasörleri veya Azure Stack oluşturmaz MASBACKUP içinde depolama veri oluşturma önerilmez. 
 -  Kullanıcı FQDN ve bölge adı klasörde farklı bulut yedekleme verilerden ayırmak için. Azure Stack dağıtımı ve uç noktalarına tam etki alanı adı (FQDN) bölge parametresi ve dış etki alanı adı parametresi birleşimidir. Daha fazla bilgi için [Azure Stack'i veri merkezi tümleştirmesi - DNS](azure-stack-integrate-dns.md).

Örneğin, yedekleme fileserver01.contoso.com üzerinde barındırılan AzSBackups paylaşımıdır. Bu dosya paylaşımı başına dış etki alanı adı ve bölge adını kullanan bir alt klasör Azure Stack dağıtımı bir klasör olabilir. 

FQDN: contoso.com  
Bölge: nyc


    \\fileserver01.contoso.com\AzSBackups
    \\fileserver01.contoso.com\AzSBackups\contoso.com
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\MASBackup

Azure Stack yedekleme verilerini depoladığı MASBackup klasörü olur. Kendi verilerinizi depolamak için bu klasör kullanmamanız gerekir. OEM ya tüm yedekleme verilerini depolamak için bu klasör kullanmamalısınız. 

OEM'lerin bileşenlerinin bölge klasörü altındaki yedekleme verilerini depolamak için önerilir. Her ağ anahtarları, donanım yaşam döngüsü ana bilgisayar (HLH) ve benzeri kendi alt klasöründe depolanabilir. Örneğin:

    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\HLH
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Switches
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\DeploymentData
    \\fileserver01.contoso.com\AzSBackups\contoso.com\nyc\Registration

### <a name="monitoring"></a>İzleme

Aşağıdaki uyarıları, sistem tarafından desteklenir:

| Uyarı                                                   | Açıklama                                                                                     | Düzeltme                                                                                                                                |
|---------------------------------------------------------|-------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Dosya Paylaşımı kapasitesi dışında olduğu için yedekleme başarısız oldu. | Dosya paylaşımıdır kapasitesi azalıyor ve yedekleme denetleyicisi yedekleme dosyalarının konumuna dışarı aktaramazsınız. | Daha fazla depolama kapasitesi eklemenize ve yedekleme yeniden deneyin. Yer kazanmak için mevcut yedeklemeler (ilk başlanarak) silin.                    |
| Yedekleme bağlantısı sorunları nedeniyle başarısız oldu.             | Ağ paylaşımı dosya ve Azure Stack arasında sorun yaşıyor.                          | Ağ sorunu giderin ve yedekleme işlemini yeniden deneyin.                                                                                            |
| Yedekleme yolunda bir hata nedeniyle başarısız oldu                | Dosya Paylaşımı yolu çözümlenemiyor                                                          | Paylaşımı erişilebilir olduğundan emin olmak için farklı bir bilgisayara paylaşımından eşleyin. Artık geçerli değilse yolu güncelleştirmeniz gerekebilir.       |
| Kimlik doğrulama sorunu nedeniyle yedekleme başarısız oldu               | Kimlik bilgileri ile ilgili bir sorun veya kimlik doğrulaması etkileyen bir ağ sorunu olabilir.    | Paylaşımı erişilebilir olduğundan emin olmak için farklı bir bilgisayara paylaşımından eşleyin. Artık geçerli olmayan kimlik bilgilerini güncelleştirmeniz gerekebilir. |
| Genel bir hata nedeniyle yedekleme başarısız oldu                    | Başarısız istek aralıklı olarak ortaya çıkan bir sorun nedeniyle olabilir. Yedekleme yeniden deneyin.                    | Desteği arayın                                                                                                                               |

## <a name="next-steps"></a>Sonraki adımlar

 - Gözden geçirme için başvuru materyalleri [altyapı Backup hizmeti](azure-stack-backup-reference.md).  
 - Etkinleştirme [altyapısını yedekleme hizmeti](azure-stack-backup-enable-backup-console.md).