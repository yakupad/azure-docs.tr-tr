---
title: Azure güvenlik ve uyumluluk planı - UK NHS için veri analizi
description: Azure güvenlik ve uyumluluk planı - UK NHS için veri analizi
services: security
author: jomolesk
ms.assetid: 103dff31-e262-44a6-9b50-3b3467c0cb3a
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 8885eba0d69c869ad5d298094b835f0351d8d94d
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37343039"
---
# <a name="azure-security-and-compliance-blueprint-data-analytics-for-uk-nhs"></a>Azure güvenlik ve uyumluluk planı: UK NHS için veri analizi

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk planı, bir başvuru mimarisi ve toplama, depolama, analiz ve sağlık verilerinin alınması için uygun bir veri analizi çözümü için yönergeler sağlar. Bu çözüm, müşteriler uyumlu, sağlanan yönergeler ile yolları gösterir [bulut güvenlik iyi uygulama Kılavuzu](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) tarafından yayımlanan [NHS dijital](https://digital.nhs.uk/), Birleşik Krallık'ın (UK Bölgesinde) iş ortağı Departman, sağlık ve sosyal Bakım (DHSC). Bulut güvenliği iyi uygulama Kılavuzu 14 üzerinde alan [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) yayımlanan Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC tarafından).

Bu başvuru mimarisi, Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-ek yapılandırma olmadan bir üretim ortamında olduğundan. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, müşteriler, kendi analiz araçları oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi işletimsel veri güncelleştirmelerine işletimsel bir kullanıcı veya toplu veri içeri aktarmaları SQL/veri Yöneticisi tarafından aracılığıyla müşteriler giriş verileri nerede genel kullanım örneği açıklanmaktadır. Hem Azure SQL veritabanı'na veri almak için iş akışlarını Azure işlevleri birleştirmek ve tüm dış bağlantıları TLSv1.2 gerektirir. Azure işlevleri, müşterinin analiz gereksinimleri için benzersiz alma görevlerini işlemek için Azure Portalı aracılığıyla müşteri tarafından yapılandırılması gerekir.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Ancak bu çözüm, Azure Analysis Services verilerine hızla göz atın ve Müşteri verilerinin daha akıllı modelleme aracılığıyla daha hızlı sonuçlar teslim etmek için Azure SQL veritabanı ile birlikte içerir. Azure Analiz Hizmetleri, machine Learning veri kümeleri arasında yeni ilişkiler bularak sorgu hızı artırmak için hedeflenen bir biçimidir. Veriler çeşitli istatistik işlevleri aracılığıyla eğitim almış sonra en fazla 7 ek sorgu havuzları (müşteri sunucu dahil olmak üzere toplam 8) sorgu iş yükü dağıtabilir ve yanıt sürelerini kısaltmak için aynı tablosal modelleri ile eşitlenebilir.

Azure SQL veritabanları, Gelişmiş analiz ve raporlama için columnstore dizinleri ile yapılandırılabilir. Hem Azure Analiz Hizmetleri hem de Azure SQL veritabanları yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Verileri Azure SQL veritabanı'na yüklenen ve Azure Analysis Services tarafından geliştirilen sonra hem işletimsel kullanıcı hem de Power BI ile SQL/veri Yöneticisi tarafından Sindirilmiş. Power BI verilerini sezgisel görüntüler ve bilgi ilişkin daha kapsamlı içgörüler çizmek için birden fazla veri kümesi arasında birlikte çeker. Müşteriler, iş gereksinimlerini gerektirdiği gibi senaryoları çeşit işlemek için yapılandırabilir, yüksek düzeyde uyumluluk ve Azure SQL veritabanı ile kolay tümleştirme sağlar.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için seçtiğiniz bir müşterinin merkezinde verilerin üç kopyasını depolar. Veriler bir ikincil veri merkezine mil uzaklıkta, yüz çoğaltılmış ve müşterinin birincil veri merkezinde olumsuz bir olay kaybına oluşan önleme, veri merkezi içinde üç kopya olarak yeniden depolanan, coğrafi olarak yedekli depolama sağlar. veriler.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Rol tabanlı erişim denetimi erişimi denetlemek için kullanılan Azure Active Directory, Azure anahtar Kasası'nda kendi anahtarları gibi kaynakları dağıtıldı. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı, yaygın olarak güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış yerel makineden çalışan SQL Server Management Studio aracılığıyla yönetilir. **Yönetim ve veriler için bir VPN veya ExpressRoute bağlantısı yapılandırma Microsoft önerir başvuru mimarisi kaynak grubuna içe**.

![UK NHS başvurusu mimari şeması için Analytics](images/uknhs-analytics-architecture.png?raw=true "analizi için UK NHS başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Active Directory
- Azure Analysis Services
- Azure Otomasyonu
- Azure Veri Kataloğu
- Azure Disk Şifrelemesi
- Azure Event Grid
- Azure İşlevleri
- Azure Key Vault
- Azure İzleyici
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Sanal Ağ
    - (1) /16 ağ
    - (2) /24 ağlar
    - (2) ağ güvenlik grupları
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı kullanıcıların sağlayan bir sunucusuz işlem hizmetidir. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Analysis Services**: [Azure Analysis Service](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) kurumsal veri modelleme ve Azure veri platformu hizmetleriyle tümleştirme sağlar. Azure Analysis Services, bir tek veri modeline birden çok kaynaktan gelen verileri birleştirerek çok büyük miktarda veriyi gözatma yukarı hızlandırır.

### <a name="virtual-network"></a>Sanal ağ

10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Ağ güvenlik grupları, trafiğin bir alt ağ veya tek tek sanal makine düzeyinde güvenliğini sağlamak için kullanılabilir. Aşağıdaki ağ güvenlik grupları mevcut:

  - Active Directory için 1 ağ güvenlik grubu
  - iş yükü için 1 ağ güvenlik grubu

Sahip ağ güvenlik gruplarının her biri belirli bağlantı noktaları ve protokolleri çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar için her bir ağ güvenlik grubu etkinleştirilir:

- [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
- Log Analytics'e bağlı olduğu [ağ güvenlik grubu&#39;s tanılama günlükleri](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen ağ güvenlik grubu ile ilişkilidir.

### <a name="data-in-transit"></a>Aktarımdaki verileri

Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini NHS dijital tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:

- [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
- [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
- Azure SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. Veriler güvencesi yetkisiz erişim ayarlanmamış saydam veri şifrelemesi sağlar.
- [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
- [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
- [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, tanımlamak ve yetkisiz erişim veritabanı yok, verilere erişim azaltmak için yardımcı olur. Müşteriler, dinamik veri maskeleme, veritabanı şeması uyması için ayarları ayarlamak için sorumludur.

### <a name="identity-management"></a>Kimlik yönetimi

Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft&#39;s çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure Active Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere dizininde, oluşturulur.
- Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure Active Directory veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her sınırsız kullanıcı izin vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar. Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluş etkileyen olası güvenlik açıklarını algılar&#39;s kimlikleri, bir kuruluş için ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır&#39;s kimlikleri ve bunları gidermek için uygun eylemde için şüpheli olayları araştırır.

### <a name="security"></a>Güvenlik

**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin korumak ve böyle verilere Yardımı:

- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar türü, bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtarı değil.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Azure Güvenlik Merkezi**: ile [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,.

**Log Analytics**: Bu günlükler, birleştirilmiş [Log Analytics](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Log Analytics sorguları kullanmak için sağlayarak müşterilerin Log Analytics ile entegre olur.

Aşağıdaki Log Analytics [yönetim çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, runbook'ları, Azure SQL veritabanı'ndan günlüklerini toplama yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek müşterilerin çözüm sağlar.

**Azure İzleyici**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve denetim, uyarılar oluşturabilir ve bunların Azure'da API çağrıları izleme dahil olmak üzere, verileri arşivlemek kuruluşların etkinleştirerek eğilimleri belirlemenize yardımcı olur kaynaklar.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/uknhs-analytics-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![UK NHS tehdit modeli için Analytics](images/uknhs-analytics-threat-model.png?raw=true "UK NHS tehdit modeli için analiz")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı: UK NHS müşteri sorumluluk matris](https://aka.ms/uknhs-crm) UK NHS tarafından gerekli tüm güvenlik ilkelerinin listeler. Bu matris her ilkesi uygulaması, Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı: UK NHS veri analizi uygulaması matris](https://aka.ms/uknhs-analytics-cim) üzerinde hangi UK NHS gereksinimleri nasıl ayrıntılı açıklamaları da dahil olmak üzere veri analizi mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama, her kapsanan ilkesi gereksinimlerini karşılar.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu verileri bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılacak analytics mimarisi başvurusu. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantının Internet üzerinden yer alır ve müşterilere güvenli bir şekilde sağlar &quot;tünel&quot; müşteri arasında şifrelenmiş bir bağlantı içinde bilgilerine&#39;s ağınız ve Azure. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşteri doğrudan bir bağlantı olduğundan&#39;s telekomünikasyon sağlayıcısı, verileri Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Çıkartma-dönüştürme-yükleme işlemi

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) Azure SQL veritabanı'na veri yükleme ayıklamak için ayrı bir gereksinimi olmadan, dönüştürme, yükleyebilir ve aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, aynı zamanda SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, Azure Active Directory ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) var olan bir Azure Active Directory alt etki alanı bir Azure Active Directory ormanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
