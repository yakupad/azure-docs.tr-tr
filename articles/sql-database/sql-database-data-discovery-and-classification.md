---
title: Azure SQL veritabanı veri bulma & sınıflandırma | Microsoft Docs
description: Azure SQL veritabanı veri bulma & sınıflandırma
services: sql-database
author: giladmit
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: giladm
ms.openlocfilehash: cc093bebb4b3c39140d6fa5370a78d59168990fa
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950825"
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL veritabanı veri bulma ve sınıflandırma
Veri bulma & sınıflandırma (şu anda önizlemede), Azure SQL veritabanı için gelişmiş özellikler sunar **keşfetme**, **sınıflandırma**, **etiketleme**  &  **koruma** veritabanlarınızı hassas verileri.
Bulma ve sınıflandırma en hassas verileriniz (iş, Finans, sağlık, PII, vb.), kuruluş bilgilerini koruma stature içinde rol yürütebilirsiniz. Altyapı olarak hizmet eder:
* Veri gizliliği standartlarını ve yasal uyumluluk gereksinimlerini karşılamak yardımcı olur.
* (Denetim) izleme gibi çeşitli güvenlik senaryoları ve anormal hassas verilere erişimi üzerinde uyarı.
* Erişimi denetleme ve son derece hassas veri içeren veritabanlarını güvenliğini artırma.

Veri bulma & sınıflandırma parçası olan [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md) Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir (ATP) teklifi. Veri bulma & sınıflandırma erişilen ve merkezi SQL ATP portalı üzerinden yönetilebilir.

> [!NOTE]
> Bu belge, Azure SQL veritabanı'na yalnızca ilişkilendirir. SQL Server (şirket içi) için bkz: [SQL veri bulma ve sınıflandırma](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Veri bulma ve sınıflandırma nedir?
Veri bulma & sınıflandırma yalnızca veritabanı verileri korumaya yönelik yeni bir SQL bilgi koruması paradigma oluşturan bir dizi Gelişmiş Hizmetleri ve yeni SQL işlevleri sunar:
* **Bulma ve öneriler** – sınıflandırma altyapısının veritabanınızı tarar ve potansiyel olarak hassas veriler içeren sütunlarını tanımlar. Bunun ardından, gözden geçirin ve uygun sınıflandırma öneriler Azure portal aracılığıyla uygulamak için kolay bir yol sağlar.
* **Etiketleme** – duyarlılık sınıflandırma etiketleri kalıcı olarak etiketlenmiş SQL motoruna sunulan yeni sınıflandırma meta veri öznitelikleri kullanarak sütunlarda. Bu meta veriler, ardından Gelişmiş duyarlılık tabanlı denetim ve koruma senaryoları için kullanılabilir.
* **Sorgu sonuç kümesi duyarlılık** – denetim amacıyla gerçek zamanlı sorgu sonuç kümesinde duyarlılığına hesaplanır.
* **Görünürlük** -veritabanı sınıflandırma durumu ayrıntılı bir panoya portalında görüntülenebilir. Ayrıca, uyumluluk ve denetim amacıyla, yanı sıra diğer gereksinimlerini kullanılacak bir raporu (Excel biçiminde) indirebilirsiniz.

## <a id="subheading-2"></a>Bulma, Sınıflandırma ve etiket hassas sütunları
Aşağıdaki bölümde, bulma, Sınıflandırma ve etiketleme veritabanı hem de veritabanı geçerli sınıflandırma durumunu görüntüleme ve raporları dışarı aktarma hassas veriler içeren sütunların adımlarını açıklar.

Sınıflandırma iki meta veri öznitelikleri içerir:
* Sütunda depolanan verilerin duyarlılık düzeyi tanımlamak için kullanılan etiketler – ana sınıflandırma öznitelikleri.  
* Bilgi türlerini sütunda depolanan verilerin türünü içine ek ayrıntı düzeyi sağlar.

## <a name="classify-your-sql-database"></a>SQL veritabanınız sınıflandırma

1. [Azure Portal](https://portal.azure.com) gidin.

2. Gidin **Gelişmiş tehdit koruması** güvenlik başlığı, Azure SQL veritabanı bölmesinde. Gelişmiş tehdit Koruması'nı etkinleştirmek için tıklayın ve ardından **veri bulma & sınıflandırma (Önizleme)** kart.

   ![Bir veritabanı tarama](./media/sql-data-discovery-and-classification/data_classification.png)

3. **Genel bakış** sekmesi yalnızca belirli bir şema bölümleri, bilgi türleri ve etiketleri görüntülemek için filtreleyebilirsiniz sınıflandırılmış sütunlar ayrıntılı bir listesi dahil olmak üzere, veritabanının geçerli sınıflandırma durumu özetini içerir. Hiçbir sütun sınıflandırılmış henüz yapmadıysanız, [5. adıma](#step-5).

   ![Geçerli sınıflandırma durumu özeti](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Excel biçimindeki bir rapor indirmek için tıklayın **dışarı** penceresinin üst menü seçeneği.

   ![Excel'e Aktar](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5.  <a id="step-5"></a>Verilerinizi sınıflandırmak başlatmak için tıklayın **sınıflandırma sekmesini** pencerenin üst kısmındaki.

    ![Veri sınıflandırma](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. Sınıflandırma altyapısının olası hassas verileri içeren sütunlar için veritabanınızı tarar ve bir listesini sağlar **sütun sınıflandırmaları önerilen**. Sınıflandırma önerileri uygulayın ve görüntülemek için:

    * Pencerenin alt kısmındaki önerileri panelde önerilen sütun sınıflandırmaları listesini görüntülemek için tıklayın:

      ![Veri sınıflandırma](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

    * Belirli bir sütun için bir öneri kabul edin, ilgili satır sol sütunda onay için öneriler – listesini gözden geçirin. Ayrıca işaretleyebilirsiniz *tüm önerileri* önerileri tablo üstbilgisinde onay kutusunu işaretleyerek kabul edildi olarak.

       ![Gözden geçirme öneri listesi](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

    * Seçili önerileri uygulamak için üzerinde mavi tıklayın **seçili önerileri kabul** düğmesi.

      ![Önerileri uygulama](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Ayrıca **el ile sınıflandırma** sütunları alternatif olarak veya ek olarak, öneri tabanlı sınıflandırma için:

    * Tıklayarak **sınıflandırma Ekle** penceresinin üst menüdeki.

      ![El ile sınıflandırma Ekle](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

    * Açılan bağlam penceresinde şemasını seçin > Tablo > sınıflandırmak istediğiniz sütunu ve bilgi türü ve duyarlılık etiketi. Mavi üzerinde ardından **sınıflandırma Ekle** bağlam pencerenin alt kısmındaki düğmesi.

      ![Sınıflandırmak için sütun seçin](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. Yeni sınıflandırma meta verilerle (etiketi) veritabanı sütunları kalıcı olarak etiket sınıflandırmanızı tamamlamak için tıklayın **Kaydet** penceresinin üst menüdeki.

   ![Kaydet](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Hassas verilere erişimi denetleme

Bilgi koruma paradigma önemli bir yönüdür hassas verilere erişimi izlemek için yeteneğidir. [Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) Denetim günlüğüne adlı yeni bir alan eklemek için Gelişmiş *data_sensitivity_information*, günlükleri tarafından döndürülen gerçek verilerin duyarlılık sınıflandırmaları (etiketler) Sorgu.

![Denetleme günlüğü](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Otomatik programlama sınıflandırma

Sütun sınıflandırmaları Ekle/Kaldır ek olarak, veritabanının tamamı için tüm sınıflandırmaları almak için T-SQL kullanabilirsiniz.

> [!NOTE]
> Etiketleri yönetmek için T-SQL kullanarak, eklenen bir sütuna kuruluş bilgilerini koruma İlkesi (portal önerileri görünen etiket kümesi) mevcut olan doğrulama yoktur. Therefor bunu doğrulamak için size bağlıdır.

* Bir veya daha fazla sütun sınıflandırmasını ekleme/güncelleştirme: [DUYARLIK SINIFLANDIRMASI ekleyin](https://docs.microsoft.com/en-us/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
* Bir veya daha fazla sütunlarından Sınıflandırmayı kaldırmak: [bırak DUYARLIK SINIFLANDIRMASI](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
* Tüm sınıflandırmalar veritabanında görüntüleyin: [sys.sensitivity_classifications](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

## <a id="subheading-5"></a>Sonraki adımlar

- Daha fazla bilgi edinin [SQL Gelişmiş tehdit koruması](sql-advanced-threat-protection.md).
- Yapılandırmayı göz önünde bulundurun [Azure SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) izleme ve, sınıflandırılmış hassas verilere erişimi denetleme.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
