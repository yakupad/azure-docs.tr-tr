---
title: Azure SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanındaki elastik havuzlar için bazı ortak sanal çekirdek tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 5503ffaf8a429221a0a0730fc999cb7a90f43785
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092129"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Azure SQL veritabanı sanal çekirdek tabanlı model sınırları elastik havuzlar için satın alma

Bu makalede, Azure SQL veritabanı elastik havuzları ve sanal çekirdek tabanlı satın alma modeli kullanarak havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli limitleri için bkz. [SQL veritabanı DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).


## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Elastik havuz: depolama alanı boyutları ve performans düzeyleri

SQL veritabanı elastik havuzları için aşağıdaki tablolarda her hizmet katmanı ve performans düzeyinde kaynaklar gösterilmektedir. Bir hizmet katmanını, performans düzeyi ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), veya [RESTAPI](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Kaynak sınırları elastik havuzlardaki veritabanlarını tek tek genellikle tek veritabanları aynı performans düzeyine sahip havuzları dışında aynıdır. Örneğin, maks. eş zamanlı çalışan GP_Gen4_1 veritabanı için 200 çalışanları olur. Bu nedenle, GP_Gen4_1 havuzdaki bir veritabanı için en fazla eş zamanlı çalışan var. Ayrıca 200 çalışanları 210 olduğuna dikkat edin. eş zamanlı çalışan GP_Gen4_1 havuzundaki toplam sayısı.

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

#### <a name="generation-4-compute-platform"></a>4. nesil işlem platformu
|Performans düzeyi|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|En yüksek veri boyutu (GB)|512|756|1536|2048|3584|4096|
|Maksimum günlük boyutu|154|227|461|614|1075|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Maks. eş zamanlı çalışan (istek)|210|420|840|1680|3360|5040|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|100|200|500|500|500|500|
|Min/Maks elastik havuz tıklayın-durdurur|0, 0.25, 0,5, 1|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>5. nesil işlem platformu
|Performans düzeyi|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|En yüksek veri boyutu (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Maksimum günlük boyutu|154|227|461|614|922|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Maks. eş zamanlı çalışan (istek)|210|420|840|1680|2520|3360|4200|8400
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|100|200|500|500|500|500|500|500|
|Min/Maks elastik havuz tıklayın-durdurur|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Çoğaltma sayısı|1|1|1|1|1|1|1|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı

#### <a name="generation-4-compute-platform"></a>4. nesil işlem platformu
|Performans düzeyi|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|1|2|4|8|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|Maksimum günlük boyutu|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Maks. eş zamanlı çalışan (istek)|210|420|840|1680|3360|5040|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|Yok|50|100|100|100|100|
|Min/Maks elastik havuz tıklayın-durdurur|Yok|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|
|Çoğaltma sayısı|3|3|3|3|3|3|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>5. nesil işlem platformu
|Performans düzeyi|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|En yüksek veri boyutu (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maksimum günlük boyutu|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Maks. eş zamanlı çalışan (istek)|210|420|840|1680|2520|3360|5040|8400|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|Yok|50|100|100|100|100|100|100|
|Min/Maks elastik havuz tıklayın-durdurur|Yok|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Çoğaltma sayısı|3|3|3|3|3|3|3|3|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

Tüm sanal çekirdek, bir elastik havuzun meşgul ise, havuzdaki her bir veritabanı eşit miktarda sorguları işlemek için işlem kaynaklarını alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, herhangi bir kaynak yoksa veritabanı başına en düşük vCore değeri, sıfır olmayan bir değere ayarlandığında her veritabanı için garanti edilen miktarda'ek niteliğindedir.

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri tanımlar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına en yüksek sanal çekirdekler |Havuzdaki diğer veritabanlarının tarafından kullanılabilir kullanım oranlarına dayalı, herhangi bir veritabanı havuzu kullanabiliriz sanal çekirdek sayısı. Veritabanı başına en yüksek Vcore bir veritabanı için kaynak garantisi değil. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımı yükündeki yüksek veritabanı başına en yüksek Vcore ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir.|
| Veritabanı başına en düşük Vcore |Herhangi bir havuzda veritabanı sanal çekirdek sayısı alt sınırını garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına minimum sanal çekirdekler 0 olarak ayarlayın ve aynı zamanda varsayılan değerdir. Bu özellik 0 ile veritabanı başına ortalama çekirdek kullanımı arasında herhangi bir yere ayarlanır. Havuz başına sanal çekirdekler ürün veritabanları havuz ve veritabanı başına en az çekirdek sayısını aşamaz.|
| Veritabanı başına maks. depolama |Bir havuzda bir veritabanı için kullanıcı tarafından ayarlanmış maksimum veritabanı boyutu. Havuza alınmış veritabanları, bir veritabanı ulaşabilirsiniz boyutu sınırlıdır bu nedenle ayrılmış havuz depolama alanını paylaşır küçük kalan havuz depolama ve veritabanı boyutu. Maks. veritabanı boyutu, veri dosyaları için en büyük boyutunu ifade eder ve günlük dosyalarının kullandığı alanı içermez. |
|||
 
## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
