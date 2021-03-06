---
title: Azure SQL veritabanı denetimi ile çalışmaya başlama | Microsoft Docs
description: Azure SQL veritabanı denetimi veritabanı olaylarını bir denetim günlüğüne izlemek için kullanın.
services: sql-database
author: giladmit
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 06/24/2018
ms.author: giladm
ms.openlocfilehash: f187a5fe1541f5508e55443abe80fc295ee63c87
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081464"
---
# <a name="get-started-with-sql-database-auditing"></a>SQL veritabanı denetimini kullanmaya başlayın
Azure SQL veritabanı denetimi veritabanı olaylarını ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar izler. Ayrıca denetleme:

* Yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve iş endişeleri veya güvenlik ihlalleri hakkında daha fazla bilgi kavramanıza yardımcı olur.

* Etkinleştirir ve uyumluluk garanti etmez ancak Uyumluluk standartlara uyulması kolaylaştırır. Bu destek standartları uyumluluğu Azure hakkında daha fazla bilgi programlar için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Genel Bakış denetim azure SQL veritabanı
SQL veritabanı için denetimi kullanabilirsiniz:


* **Korumak** seçili olayların bir denetim izi. Denetlenecek veritabanı eylemleri kategorilerini tanımlayabilirsiniz.
* **Rapor** veritabanı etkinlik. Etkinlik ve olay Raporlama ile hızlı bir şekilde başlamak için önceden yapılandırılmış raporları ve panoyu kullanabilirsiniz.
* **Analiz** raporlar. Şüpheli olayları, olağan dışı etkinliği ve eğilimleri bulabilirsiniz.

Farklı türlerde olay kategorisi için Denetim açıklandığı şekilde yapılandırabilirsiniz [veritabanınız için denetimi ayarlamanız](#subheading-2) bölümü.

> [!IMPORTANT]
> Denetim günlükleri için yazılır **ek Bloblarını** , Azure aboneliğinizin üzerinde bir Azure Blob storage'da.
>
> * **Premium depolama** şu anda **desteklenmiyor** ek Bloblarını tarafından.
> * **VNet içinde depolama** şu anda **desteklenmiyor**.

## <a id="subheading-8"></a>Sunucu düzeyinde ve veritabanı düzeyi denetim ilkesi tanımlayın

Bir denetim ilkesi, belirli bir veritabanı veya varsayılan sunucu ilkesi olarak tanımlanabilir:

* Sunucudaki tüm mevcut ve yeni oluşturulan veritabanları için bir sunucu ilkesi uygulanır.

* Varsa *sunucu blob denetimi etkin*, onu *veritabanı için her zaman geçerli*. Denetim ayarları veritabanı bağımsız olarak veritabanı denetlenecektir.

* Veritabanı üzerinde BLOB denetiminin etkinleştirilmesi, sunucu üzerinde etkinleştirmenin yanı sıra mu *değil* geçersiz kılabilir veya sunucu blob denetimi ayarlarından herhangi birini değiştirin. Her iki denetimleri yan yana yer alır. Diğer bir deyişle, veritabanı iki kez paralel olarak denetlenir; Sunucu İlkesi ve bir kez veritabanı İlkesi tarafından bir kez.

   > [!NOTE]
   > Sunucu blob denetlenmesi ve veritabanı blob birlikte denetimi sürece etkinleştirme kaçınmanız gerekir:
    > * Farklı bir kullanmak istediğiniz *depolama hesabı* veya *saklama dönemi* belirli bir veritabanı için.
    > * Olay türlerini veya belirli bir veritabanı için veritabanlarını sunucuda geri kalanından farklı kategorileri denetlemek istediğiniz. Örneğin, yalnızca belirli bir veritabanı için denetlenmesi gereken tablo ekler olabilir.
   >
   > Aksi takdirde, yalnızca sunucu düzeyinde blob denetlemeyi etkinleştirme ve devre dışı tüm veritabanları için veritabanı düzeyinde denetimi bırakın önerilir.


## <a id="subheading-2"></a>Veritabanınız için denetimi ayarlamanız
Aşağıdaki bölümde denetim Azure Portalı'nı kullanarak yapılandırmayı açıklar.

1. [Azure Portal](https://portal.azure.com) gidin.
2. Gidin **denetim** SQL veritabanı/sunucu bölmesinde güvenlik başlığı altında.

    <a id="auditing-screenshot"></a>![Gezinti Bölmesi][1]
3. Bir sunucu denetim ilkesini ayarlamak tercih ederseniz, seçebileceğiniz **sunucu ayarlarını görüntüleyin** veritabanı denetim dikey penceresinde bağlantı. Daha sonra görüntülemek veya sunucunun denetim ayarları değiştirin. Sunucu denetim ilkeleri, bu sunucudaki tüm mevcut ve yeni oluşturulan veritabanları için geçerlidir.

    ![Gezinti bölmesi][2]
4. Veritabanı düzeyinde denetimini etkinleştirmek tercih ederseniz, geçiş **denetim** için **ON**.

    Sunucu denetimi etkinse, veritabanı yapılandırılmış denetim yana birimi sunucu denetim ile mevcut.

    ![Gezinti bölmesi][3]
5. Açmak için **denetim günlüklerini depolama** dikey penceresinde, select **depolama ayrıntıları**. Burada günlükleri kaydedilecek ve Bekletme dönemi seçin Azure depolama hesabı seçin. Eski günlükleri silinir. Daha sonra, **Tamam**'a tıklayın.

    <a id="storage-screenshot"></a>![Gezinti Bölmesi][4]
6. Denetlenen olayları özelleştirmek istiyorsanız, bunu aracılığıyla yapabilirsiniz [PowerShell cmdlet'leri](#subheading-7) veya [REST API](#subheading-9).
7. Denetim ayarlarını yapılandırdıktan sonra yeni tehdit algılama özelliğini açmak ve güvenlik uyarıları almak için e-postaları yapılandırın. Tehdit algılama kullandığınızda, olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini öngörülü uyarılar alırsınız. Daha fazla bilgi için bkz: [tehdit algılama ile çalışmaya başlama](sql-database-threat-detection-get-started.md).
8. **Kaydet**’e tıklayın.





## <a id="subheading-3"></a>Denetim günlüklerini ve raporları analiz eder.
Denetim günlükleri, Kurulum sırasında seçtiğiniz Azure depolama hesabında toplanır. Gibi bir araç kullanarak denetim günlüklerini keşfedebilirsiniz [Azure Storage Gezgini](http://storageexplorer.com/).

BLOB denetim günlüklerini adlı bir kapsayıcı içindeki blob dosya koleksiyonu olarak kaydedilir **sqldbauditlogs**.

Adlandırma kuralları ve günlük biçimi, depolama klasör hiyerarşisini hakkında daha fazla ayrıntı için bkz: [Blob denetim günlük biçimi başvurusu](https://go.microsoft.com/fwlink/?linkid=829599).

Blob denetim günlüklerini görüntülemek için kullanabileceğiniz birkaç yöntem vardır:

* Kullanım [Azure portal](https://portal.azure.com).  İlgili veritabanını açın. Veritabanının üstündeki **denetim ve tehdit algılama** dikey penceresinde tıklatın **denetim günlüklerini görüntüle**.

    ![Gezinti bölmesi][7]

    Bir **denetim kayıtları** dikey penceresi açılır ve kendisinden, günlükleri görüntüleyebilirsiniz.

    - Belirli tarihleri tıklatarak görüntüleyebileceğiniz **filtre** en üstündeki **denetim kayıtları** dikey.
    - Tarafından oluşturulan denetim kayıtları arasında geçiş yapabilirsiniz *sunucu denetim ilkesi* ve *veritabanı Denetim İlkesi* geçiş tarafından **kaynak denetim**.
    - SQL ekleme Denetim kayıtlarını denetleyerek ilgili yalnızca görüntüleyebileceğiniz **Göster yalnızca SQL eklemelerini kayıtlarını denetim** onay kutusu.

       ![Gezinti bölmesi][8]

* Bir sistem işlevi kullanın **sys.fn_get_audit_file** (Denetim günlüğü verilerini tablo biçiminde döndürmek için T-SQL). Bu işlev kullanma hakkında daha fazla bilgi için bkz: [sys.fn_get_audit_file belgelerine](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


* Kullanım **denetim dosyaları birleştirme** SQL Server Management Studio'da (SSMS 17 ile başlayarak):
    1. SSMS menüsünden seçin **dosya** > **açık** > **denetim dosyaları birleştirme**.

        ![Gezinti bölmesi][9]
    2. **Denetim dosyaları Ekle** iletişim kutusu açılır. Aşağıdakilerden birini seçin **Ekle** Seçenekleri denetim dosyalarını yerel bir sürücüden birleştirmeniz veya Azure depolama biriminden almak isteyip istemediğinizi seçin. Azure Storage ayrıntıları ve hesap anahtarı sağlamak için gereklidir.

    3. Birleştirme için tüm dosyaları ekledikten sonra tıklatın **Tamam** birleştirme işlemini tamamlamak için.

    4. Burada, görüntülemek ve analiz edin, yanı sıra bir XEL'e ya da CSV dosyası veya tablo dışa SSMS birleştirilmiş dosya açılır.

* Kullanım [eşitleme uygulama](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) , oluşturduk. Azure üzerinde çalışır ve SQL denetim günlüklerini günlük analizi göndermek için günlük analizi genel API'leri kullanır. Eşitleme uygulama günlük analizi Pano tüketimi için günlük analizi SQL denetim günlüklerini iter.

* Power BI kullanın. Görüntülemek ve denetim günlüğü verilerini Power bı'da analiz edin. Daha fazla bilgi edinmek [Power BI ve erişim indirilebilir bir şablon](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Günlük dosyaları gibi bir araç kullanarak veya Portalı aracılığıyla Azure depolama blob kapsayıcısını indirin [Azure Storage Gezgini](http://storageexplorer.com/).
    * Bir günlük dosyası yerel olarak indirdikten sonra açın, görüntülemek ve SSMS'ndeki günlükleri çözümlemek için dosyayı çift tıklayabilirsiniz.
    * Ayrıca, Azure Storage Gezgini ile aynı anda birden çok dosya indirebilirsiniz. Belirli bir alt klasörü sağ tıklatın ve seçin **Farklı Kaydet** içinde yerel bir klasöre kaydeder.

* Ek yöntemleri:
   * Çeşitli dosyalar veya günlük dosyalarını içeren bir alt indirdikten sonra bunları yerel olarak daha önce açıklanan SSMS birleştirme Denetim dosyalarını yönergelerde açıklandığı gibi birleştirebilirsiniz.

   * Görünüm blob denetimi programlı olarak günlüğe kaydeder:

     * Kullanım [genişletilmiş olaylar okuyucu](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# Kitaplığı.
     * [Sorgu genişletilmiş olaylar dosyaları](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) PowerShell kullanarak.




## <a id="subheading-5"></a>Üretim uygulamaları
<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Coğrafi olarak çoğaltılmış veritabanları denetleme</a>
Birincil veritabanında denetim etkinleştirdiğinizde, coğrafi olarak çoğaltılmış veritabanları ile özdeş bir denetim ilkesi ikincil veritabanı gerekir. Denetimi etkinleştirme ikincil veritabanı üzerinde denetimi ayarlamanız mümkündür **ikincil sunucu**, birincil veritabanından bağımsız olarak.

* Sunucu düzeyinde (**önerilen**): hem denetim açıldıktan **birincil sunucu** yanı sıra **ikincil sunucu** -her birincil ve ikincil veritabanları denetlenmez bağımsız olarak kendi ilgili sunucu düzeyi ilkesini temel alarak.

* Veritabanı düzeyi: Veritabanı düzeyi ikincil veritabanları için denetimi yalnızca denetim ayarları birincil veritabanından yapılandırılabilir.
   * Üzerinde denetim etkinleştirilmelidir *birincil veritabanının kendisi*, sunucu değil.
   * Denetim birincil veritabanında etkinleştirildikten sonra aynı zamanda ikincil veritabanı etkin hale.

    >[!IMPORTANT]
    >Veritabanı düzeyi denetimi ile ikincil veritabanı için depolama ayarlarını çapraz bölge trafiği neden bu birincil veritabanının aynı olacaktır. Yalnızca sunucu düzeyinde denetlemeyi etkinleştirme ve devre dışı tüm veritabanları için veritabanı düzeyinde denetimi bırakın öneririz.
<br>

### <a id="subheading-6">Depolama anahtarını yeniden üretme</a>
Üretimde büyük bir olasılıkla depolama anahtarlarınızı düzenli aralıklarla yenileyin. Anahtarlarınızı yenilerken denetim ilkesi yeniden kaydetmeniz gerekir. İşlem aşağıdaki gibidir:

1. Açık **depolama ayrıntıları** dikey. İçinde **depolama erişim tuşu** kutusunda **ikincil**, tıklatıp **Tamam**. Ardından **kaydetmek** denetim yapılandırma dikey pencerenin üstündeki.

    ![Gezinti bölmesi][5]
2. Depolama yapılandırma dikey penceresine gidin ve birincil erişim anahtarını yeniden oluşturma.

    ![Gezinti bölmesi][6]
3. Birincil ikincil depolama erişim anahtarı yeniden denetim yapılandırma dikey penceresine, anahtar ve ardından Git **Tamam**. Ardından **kaydetmek** denetim yapılandırma dikey pencerenin üstündeki.
4. Depolama yapılandırma dikey penceresine geri dönün ve (için Hazırlanmakta sonraki anahtarın yenileme döngüsü) ikincil erişim anahtarını yeniden oluşturma.

## <a name="additional-information"></a>Ek Bilgiler

* Günlük hakkındaki ayrıntılar için biçimi, depolama klasör hiyerarşisini ve adlandırma kuralları, bkz: [Blob denetim günlük biçimi başvurusu](https://go.microsoft.com/fwlink/?linkid=829599).

    > [!IMPORTANT]
    > Azure SQL veritabanı ve denetim 4000 karakter veri karakter alanlar için bir denetim kaydı depolar. Zaman **deyimi** veya **data_sensitivity_information** denetlenebilir bir eylemin getirdiği değerler içeren en çok 4000 karakter, ilk 4000 karakterden ötesinde herhangi bir veriyi olacaktır  **kesilmiş ve değil denetlenen**.

* Denetim günlükleri için yazılır **ek Bloblarını** , Azure aboneliğinizin üzerinde bir Azure Blob storage'da:
    * **Premium depolama** şu anda **desteklenmiyor** ek Bloblarını tarafından.
    * **VNet içinde depolama** şu anda **desteklenmiyor**.

* Varsayılan Denetim İlkesi, tüm eylemleri ve tüm sorguları ve saklı yordamlar veritabanı yanı sıra karşı başarılı ve başarısız oturum açmalar yürütülen denetleyeceksiniz Eylem grupları aşağıdaki kümesini içerir:

    BATCH_COMPLETED_GROUP<br>
    SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP<br>
    FAILED_DATABASE_AUTHENTICATION_GROUP

    Bölümünde açıklandığı gibi eylemleri ve PowerShell kullanarak eylem grupları farklı türleri için denetimi yapılandırmak [Azure PowerShell kullanarak yönetme SQL veritabanı denetimi](#subheading-7) bölümü.

## <a id="subheading-7"></a>Azure PowerShell kullanarak SQL veritabanı denetimi yönetme

**PowerShell cmdlet'leri**:

* [Denetim İlkesi (Set-AzureRMSqlDatabaseAuditing) veritabanı Blob güncelle][105]
* [Denetim İlkesi (Set-AzureRMSqlServerAuditing) sunucu Blob güncelle][106]
* [Denetim İlkesi veritabanı (Get-AzureRMSqlDatabaseAuditing) Al][101]
* [Denetim İlkesi Sunucu Blob alın (Get-AzureRMSqlServerAuditing)][102]

Bir komut dosyası örneği için bkz: [denetim ve tehdit algılama PowerShell kullanarak yapılandırmanız](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a id="subheading-9"></a>REST API kullanarak SQL veritabanı denetimi yönetme

**REST API - Blob denetimi**:

* [Denetim İlkesi veritabanı Blob güncelle](https://docs.microsoft.com/en-us/rest/api/sql/database%20auditing%20settings/createorupdate)
* [Oluşturma veya güncelleştirme sunucusu Blob Denetim İlkesi](https://docs.microsoft.com/en-us/rest/api/sql/server%20auditing%20settings/createorupdate)
* [Denetim İlkesi veritabanı Blob alma](https://docs.microsoft.com/en-us/rest/api/sql/database%20auditing%20settings/get)
* [Denetim İlkesi Sunucu Blob alma](https://docs.microsoft.com/en-us/rest/api/sql/server%20auditing%20settings/get)

Burada yan tümcesi destekleyen ek filtreleme için ile Genişletilmiş İlke:
* [Oluşturma veya güncelleştirme veritabanı *Genişletilmiş* Denetim İlkesi Blob](https://docs.microsoft.com/en-us/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
* [Oluşturma veya güncelleştirme sunucusu *Genişletilmiş* Denetim İlkesi Blob](https://docs.microsoft.com/en-us/rest/api/sql/server%20extended%20auditing%20settings/createorupdate)
* [Veritabanını almak *Genişletilmiş* Denetim İlkesi Blob](https://docs.microsoft.com/en-us/rest/api/sql/database%20extended%20auditing%20settings/get)
* [Sunucusu *Genişletilmiş* Denetim İlkesi Blob](https://docs.microsoft.com/en-us/rest/api/sql/server%20extended%20auditing%20settings/get)

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Manage SQL database auditing using Azure PowerShell]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)
[Manage SQL database auditing using REST API]: #subheading-9

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditing
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditing
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditing
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditing
