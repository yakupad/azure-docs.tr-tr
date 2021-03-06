---
title: 'İş sürekliliği ve olağanüstü durum kurtarma (BCDR): eşleştirilmiş Azure bölgeleri | Microsoft Docs'
description: Uygulamaları sırasında veri merkezi arızalarına karşı dayanıklı olmasını sağlamak için Azure bölgesel eşleme hakkında bilgi edinin.
author: rayne-wiselman
ms.service: multiple
ms.topic: article
ms.date: 07/03/2018
ms.author: raynew
ms.openlocfilehash: 13a2b78b50b1b10975a90c1da38810f1a62a6bb5
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436918"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR): eşleştirilmiş Azure bölgeleri

## <a name="what-are-paired-regions"></a>Eşleştirilmiş bölgeleri nelerdir?

Azure dünyanın dört bir yanındaki birden çok coğrafi çalışmaktadır. Her Azure coğrafyası dünyanın en az bir Azure bölgesi içeren tanımlanmış bir alandır. Bir Azure bölgesine bir veya daha fazla veri içeren bir coğrafyadaki alanıdır.

Her Azure bölgesi aynı coğrafyadaki birlikte bölgesel çift yaparak başka bir bölgeyle eşleştirilir. Brezilya Güney, kendi Coğrafya dışında bir bölgeyle eşleştirilir istisnadır.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Şekil 1 – Azure bölgesel çiftler

| Coğrafi konum | Eşleştirilmiş bölgeler |  |
|:--- |:--- |:--- |
| Asya |Doğu Asya |Güneydoğu Asya |
| Avustralya |Avustralya Doğu |Avustralya Güneydoğu |
| Avustralya |Avustralya Orta |Avustralya Orta 2 |
| Brezilya |Brezilya Güney 2 |Orta Güney ABD |
| Kanada |Kanada Orta |Doğu Kanada |
| Çin |Çin Kuzey |Çin Doğu|
| Avrupa |Kuzey Avrupa |Batı Avrupa |
| Almanya |Almanya Orta |Almanya Kuzeydoğu |
| Hindistan |Orta Hindistan |Güney Hindistan |
| Hindistan |Batı Hindistan (1) |Güney Hindistan |
| Japonya |Japonya Doğu |Japonya Batı |
| Kore |Kore Orta |Kore Güney |
| Kuzey Amerika |Doğu ABD |Batı ABD |
| Kuzey Amerika |Doğu ABD 2 |Orta ABD |
| Kuzey Amerika |Orta Kuzey ABD |Orta Güney ABD |
| Kuzey Amerika |Batı ABD 2 |Batı Orta ABD 
| UK |Birleşik Krallık Batı |Birleşik Krallık Güney |
| ABD Savunma Bakanlığı |US DoD Doğu |US DoD Orta |
| ABD Devleti |ABD Devleti Arizona |ABD Devleti Texas |
| ABD Devleti |ABD Devleti Iowa (3) |ABD Devleti Virginia |
| ABD Devleti |ABD Devleti Virginia (4) |ABD Devleti Texas |

Tablo 1 - Azure bölgesel çiftler eşleme

- (1) Batı Hindistan farklı olduğundan, yalnızca bir yöndeki başka bir bölgeyle eşleştirilir. Güney Hindistan Batı Hindistan'ın ikincil bölgeye olduğu halde Orta Hindistan Güney Hindistan'ın ikincil bölgeye olduğu.
- (2) Güney Brezilya benzersiz olduğundan, kendi Coğrafya dışında bir bölgeyle eşleştirilir. Brezilya Güney'nın ikincil bölgeye Orta Güney ABD, ancak orta Güney ABD'ın ikincil bölgeye Brezilya Güney değil.
- (3) ABD Devleti Iowa'nın ikincil bölgeye ABD Devleti Virginia, ancak ikincil bölgeye ABD Devleti Virginia'nın ABD Devleti Iowa değil.
- (4) ABD Devleti Virginia'nın ikincil bölgeye, ABD Devleti Texas olmakla birlikte US Gov Teksas ikincil bölgeye ABD Devleti Virginia değil.


Azure'nın yalıtım ve kullanılabilirlik ilkelerinden yararlanmak için bölgesel çiftler arasında iş yüklerini çoğaltın öneririz. Örneğin, planlı Azure sistem güncelleştirmeleri sırayla dağıtılır (değil aynı zamanda) eşleştirilmiş bölgelerin arasında. Hatta ender olayda hatalı bir güncelleştirme, her iki bölgeleri aynı anda etkilenmez, anlamına gelir. Ayrıca, geniş kapsamlı bir kesinti olası durumunda her çiftte en az bir bölgenin kurtarılmasına öncelik verilir.

## <a name="an-example-of-paired-regions"></a>Eşleştirilmiş bölgeler örneği
Şekil 2'in altında olağanüstü durum kurtarma için bölgesel çift kullanan kuramsal bir uygulamanın gösterir. Yeşil sayıları (Azure işlem, depolama ve veritabanı) üç Azure Hizmetleri ve bölgeler arasında çoğaltmak için nasıl yapılandırılacağı bölgeler arası etkinliklerini vurgulayın. Eşleştirilmiş bölgeler arasında dağıtma benzersiz avantajları turuncu sayılarla vurgulanır.

![Eşleştirilmiş bölge avantajları genel bakış](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Şekil 2 – kuramsal Azure bölgesel çift

## <a name="cross-region-activities"></a>Bölgeler arası etkinlikleri
Şekil 2 ' başvurulan gibi.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure bilgi işlem (PaaS)** – kaynakları kullanılabilir başka bir bölgede bir olağanüstü durum sırasında önceden sağlamak için ek işlem kaynakları hazırlamanız gerekir. Daha fazla bilgi için [Azure dayanıklılık teknik Kılavuzu](resiliency/resiliency-technical-guidance.md).

![Depolama](./media/best-practices-availability-paired-regions/2Green.png) **Azure depolama** -coğrafi olarak yedekli depolama (GRS), Azure depolama hesabınız oluşturulduğunda varsayılan olarak yapılandırılır. GRS ile verileriniz otomatik olarak üç kez birincil bölge içinde ve üç kez eşleştirilmiş bölge içinde çoğaltılır. Daha fazla bilgi için [Azure depolama Yedekliliği seçenekleri](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL veritabanları** – ile Azure SQL standart coğrafi çoğaltma, zaman uyumsuz çoğaltma işlemlerinin eşleştirilmiş bir bölge için yapılandırabilirsiniz. Premium coğrafi çoğaltma, dünyanın herhangi bir bölgeye çoğaltma yapılandırabilirsiniz; Ancak, bu kaynakların çoğu olağanüstü durum kurtarma senaryoları için eşleştirilmiş bir bölge içinde dağıttığınız öneririz. Daha fazla bilgi için [coğrafi çoğaltma, Azure SQL veritabanı'nda](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -Resource Manager kendiliğinden bölgeler arasında Hizmet Yönetim bileşenleri mantıksal yalıtım sağlar. Başka bir deyişle, bir bölgede mantıksal hataları başka bir etkisi daha düşüktür.

## <a name="benefits-of-paired-regions"></a>Eşleştirilmiş bölgeler avantajları
Şekil 2 ' başvurulan gibi.  

![Yalıtım](./media/best-practices-availability-paired-regions/5Orange.png)
**fiziksel yalıtım** – bu tüm coğrafyalardaki pratik veya mümkün olmasa da mümkün olan Azure en az 300 mil ayırma bir bölge çiftindeki veri merkezleri arasında tercih ettiği zaman. Fiziksel veri merkezi ayrımı, doğal felaketler, toplumsal karmaşa, güç kesintileri veya her iki bölgeleri aynı anda etkileyen bir fiziksel ağ kesintisi olasılığını azaltır. Yalıtım coğrafyadaki (Coğrafya boyutu, güç/ağ altyapı kullanılabilirliğini, düzenlemelere, vb.) kısıtlamalara tabidir.  

![Çoğaltma](./media/best-practices-availability-paired-regions/6Orange.png)
**Platform tarafından sağlanan çoğaltma** -otomatik çoğaltma eşleştirilmiş bölgeye coğrafi olarak yedekli depolama alanı gibi bazı hizmetler sağlar.

![Kurtarma](./media/best-practices-availability-paired-regions/7Orange.png)
**bölge kurtarma sipariş** – olay geniş kapsamlı bir kesinti her çiftte bir bölgenin kurtarılmasına öncelik verilir. Eşleştirilmiş bölgeler arasında dağıtılan uygulamalar öncelikte kurtarılan bölgelerden birine sahip olacağı garanti edilir. Bir uygulama değil eşleştirilmiş bölgeler arasında dağıtılması durumunda kurtarma – seçilen bölgeleri kurtarılması için son iki olabilecek en kötü durumda gecikebilir.

![Güncelleştirmeleri](./media/best-practices-availability-paired-regions/8Orange.png)
**sıralı güncelleştirme** – planlı Azure sistem güncelleştirmeleri sağlık bölge çiftlerine sırayla (değil aynı zamanda) kapalı kalma süresi, hataları ve nadir bozuk mantıksal hataları etkisini en aza indirmek için güncelleştirin.

![Veri](./media/best-practices-availability-paired-regions/9Orange.png)
**veri yerleşikliği** – vergi ve yasa uygulama dairesi amacıyla veri yerleşikliğinin karşılanması için (hariç, Brezilya Güney) çiftiyle aynı coğrafyada bulunan bir bölgede bulunuyor.
