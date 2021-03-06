---
title: Azure depolama kesintisi olması durumunda yapmanız gerekenler | Microsoft Docs
description: Azure depolama kesintisi olması durumunda yapmanız gerekenler
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/15/2018
ms.author: tamram
ms.openlocfilehash: 160fe756458e067125b9d696fd0cdb929774446e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072044"
---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Bir Azure depolama kesinti oluşursa yapmanız gerekenler
Microsoft'ta, sabit hizmetlerimizin her zaman kullanılabilir olduğundan emin olmak için çalışıyoruz. Bazen bizim denetim etkisi bize bir veya daha fazla bölgede plansız bir hizmet kesintisi neden yollarla zorlar. Bu ender örnekleri işlemek yardımcı olmak için Azure depolama hizmetleri için aşağıdaki ileri düzey rehberlik sağlarız.

## <a name="how-to-prepare"></a>Hazırlamayı öğrenin
Her müşterinin kendi olağanüstü durum kurtarma planını hazırlaması kritik öneme sahiptir. Bir depolama kesintisi genellikle kurtarmak için gereken çabayı uygulamalarınızı çalışır duruma yeniden etkinleştirmek için hem operasyon personeli hem de otomatik yordamları içerir. Lütfen kendi olağanüstü durum kurtarma planınızı oluşturmak için aşağıdaki Azure belgelerine bakın:

* [Kullanılabilirlik denetim listesi](https://docs.microsoft.com/azure/architecture/checklist/availability)
* [Azure için dayanıklı uygulamalar tasarlama](https://docs.microsoft.com/azure/architecture/resiliency/)
* [Azure Site Recovery hizmeti](https://azure.microsoft.com/services/site-recovery/)
* [Azure Depolama çoğaltması](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Azure Backup hizmeti](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Tespit etme
Abone olmak için Azure hizmet durumunu belirlemek için önerilen yöntem olduğu [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Bir depolama kesintisi oluşursa yapmanız gerekenler
Bir veya daha fazla depolama hizmetleri bir veya daha fazla bölge geçici olarak kullanılamıyorsa, dikkate almanız için iki seçenek vardır. Lütfen verilerinize hemen erişim isterse, seçenek 2 göz önünde bulundurun.

### <a name="option-1-wait-for-recovery"></a>1. seçenek: Kurtarma için bekleyin.
Bu durumda, sizin herhangi bir eylemi gerekli değildir. Yapıyorduk Azure hizmet kullanılabilirliğini geri yüklemeye çalışıyoruz. Hizmet durumunu izleyebilirsiniz [Azure hizmet durumu Panosu](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>2. seçenek: verileri ikincil bölgeden kopyalayın.
Seçerseniz, [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (depolama hesaplarınız için önerilir), okuma, verileri ikincil bölgeden erişebilirsiniz. Gibi araçları kullanın [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)ve [Azure veri taşıma Kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) başka bir depolama hesabında halinde ikincil bölgeden verileri kopyalamak için bir unimpacted bölge ve ardından noktası her ikisi için uygulamalarınızı bu depolama hesabı okuma ve yazma kullanılabilirliğini.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Bir depolama yük devretme oluşursa beklenmesi gerekenler
Seçerseniz, [coğrafi olarak yedekli depolama (GRS)](storage-redundancy-grs.md) veya [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (önerilen), Azure depolama, verilerinizi iki bölgeleri (birincil ve ikincil) dayanıklı tutar. Her iki bölgede de Azure depolama, sürekli verilerinizi birden çok kopyasını tutar.

Bölgesel bir olağanüstü durum, birincil bölge etkilediğinde, önce bu bölgede hizmetini geri yüklemek deneyeceğiz. Olağanüstü durum ve bazı nadir durumlarda, kendi etkileri yapısını bağımlı biz birincil bölgeye geri yüklemeniz mümkün olmayabilir. Bu noktada, biz bir coğrafi olarak yük devretme gerçekleştirir. Bölgeler arası veri çoğaltma olası ikincil bölgeye henüz çoğaltılmamış değişiklikler kaybolabilir, bu nedenle, bir gecikme ekleyebileceği bir zaman uyumsuz bir işlemdir. Sorgulayabilirsiniz ["Son eşitleme zamanı" depolama hesabınızın](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) çoğaltma durumu ayrıntıları alınamıyor.

Birkaç depolama coğrafi olarak yük devretme deneyimi ilgili noktaları:

* Depolama coğrafi olarak yük devretme yalnızca Azure depolama ekibi tarafından tetiklenir: Müşteri Eylem gerekmiyor.
* Var olan depolama hizmet uç noktalarınıza blobları, tablolar, kuyruklar ve dosyalar için aynı yük devretme sonrasında kalır; Microsoft tarafından sağlanan DNS girişini birincil bölgeden ikincil bölgeye geçiş yapmak için güncelleştirilmesi gerekir.  Microsoft, bu güncelleştirme coğrafi olarak yük devretme işleminin parçası olarak otomatik olarak gerçekleştirir.
* Önce ve coğrafi olarak yük devretme sırasında olağanüstü durum etkisi nedeniyle, depolama hesabına yazma erişimi olmaz ancak yine de, depolama hesabı RA-GRS yapılandırılmış olması halinde ikincil okuyabilirsiniz.
* Coğrafi olarak yük devretme tamamlandı ve DNS değişiklikleri yayılır, okuma ve yazma erişimi depolama hesabınıza devam edecek; başka bir işaret ne ikincil uç noktanız olması için kullanılır. 
* Yazma erişimi, GRS veya RA-GRS depolama hesabı için yapılandırılmış olacağını unutmayın. 
* Sorgulayabilirsiniz ["Son coğrafi yük devretme zamanı" depolama hesabınızın](https://msdn.microsoft.com/library/azure/ee460802.aspx) daha ayrıntılı bilgi edinmek için.
* Yük devretmeden sonra depolama hesabınızın tam olarak çalışmıyor olabilir, ancak "düşürülmüş" durumu olarak aslında hiçbir coğrafi çoğaltma olası bir tek başına bölgeyle barındırılır. Bu riski azaltmak için biz özgün birincil bölgeye geri yükleyecek ve ardından bir coğrafi-özgün durumunu geri yüklemek için yeniden çalışma yapın. Özgün birincil bölgeye kurtarılamaz ise, biz başka bir ikincil bölgeye ayırır.
  Azure depolama coğrafi çoğaltma'nın altyapısı hakkında daha fazla ayrıntı için lütfen depolama ekibi blogu makaleye hakkında bakın [Yedeklilik seçenekleri ve RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

## <a name="best-practices-for-protecting-your-data"></a>Verilerinizi korumak için en iyi uygulamalar
Düzenli olarak, depolama verileri yedeklemek için bazı önerilen yaklaşım vardır.

* VM diskleri – [Azure Backup hizmeti](https://azure.microsoft.com/services/backup/) , Azure sanal makineler tarafından kullanılan VM disklerini yedekleme için.
* Blok blobları – açma [geçici silme](../blobs/storage-blob-soft-delete.md) nesne düzeyinde silme karşı korumak ve üzerine yazar veya başka bir depolama hesabına başka bir bölge kullanarak blobları kopyalama [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), veya [Azure veri taşıma Kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Tabloları – kullanın [AzCopy](storage-use-azcopy.md) başka bir bölgede başka bir depolama hesabına tablo verilerini dışarı aktarmak için.
* Dosyalar – kullanın [AzCopy](storage-use-azcopy.md) veya [Azure PowerShell](storage-powershell-guide-full.md) dosyalarınızı başka bir bölgede başka bir depolama hesabına kopyalamak için.

RA-GRS özelliği tam olarak yararlanan uygulamalar oluşturma hakkında daha fazla bilgi için lütfen kullanıma [tasarlama yüksek oranda kullanılabilir RA-GRS depolama kullanan uygulamalar](../storage-designing-ha-apps-with-ragrs.md)
