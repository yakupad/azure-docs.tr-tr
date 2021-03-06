---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 336e6e163178cd6d244460dbf9bee2a5bc9d714e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935794"
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Azure Iaas VM diskleri ve yönetilen ve yönetilmeyen premium diskleri hakkında sık sorulan sorular

Bu makalede, Azure yönetilen diskler ve Azure Premium SSD diskleri hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="managed-disks"></a>Yönetilen Diskler

**Azure yönetilen diskler nedir?**

Yönetilen diskler, depolama hesabı yönetimini işleyerek Azure Iaas Vm'leri için disk yönetimini basitleştiren bir özelliğidir. Daha fazla bilgi için [yönetilen disklere genel bakış](../articles/virtual-machines/windows/managed-disks-overview.md).

**Standart yönetilen disk 80 GB olan mevcut bir VHD'den oluşturursanız, ne kadar bana maliyeti ne olacak?**

80 GB'lık bir VHD'den oluşturulan standart yönetilen disk S10 disk sonraki kullanılabilir standart disk boyutu kabul edilir. S10 disk fiyatlarına göre ücretlendirilirsiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**Standart yönetilen diskler için herhangi bir işlem ücreti var mıdır?**

Evet. Her işlem için ücret ödersiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**Standart yönetilen disk için diskin sağlanan kapasitesine veya diskteki veriler gerçek boyutu için ücretlendirilirim?**

Diskin sağlanan kapasitesine göre ücret ödersiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**Nasıl premium yönetilen diskler yönetilmeyen disklerden farklı fiyatlandırma?**

Premium yönetilen diskler fiyatlandırma, yönetilmeyen premium diskler ile aynı olur.

**Depolama hesabı türünü (standart veya Premium) yönetilen disklerim değiştirebilirim?**

Evet. Azure portalı, PowerShell veya Azure CLI kullanarak yönetilen diskleriniz depolama hesabı türünü değiştirebilirsiniz.

**Yönetilen disk ile farklı bir abonelik oluşturmak için bir VHD dosyasında Azure depolama hesabı kullanabilir miyim?**

Evet.

**Farklı bir bölgede bir yönetilen disk oluşturmak için bir VHD dosyasında Azure depolama hesabı kullanabilir miyim?**

Hayır.

**Yönetilen diskler kullanan müşteriler için herhangi bir ölçek sınırlaması var mı?**

Yönetilen diskler, depolama hesapları ile ilişkili sınırlar ortadan kaldırır. Bununla birlikte, üst sınırı 50.000 yönetilen diskler bölge ve abonelik için disk türü.

**Ben, artımlı bir yönetilen diskin anlık görüntüsünü yararlanabilir miyim?**

Hayır. Geçerli anlık görüntü özelliği, bir yönetilen diskin tam bir kopyasını oluşturur.

**Bir kullanılabilirlik kümesindeki VM'ler, yönetilen ve yönetilmeyen diskler bileşiminden oluşabilir?**

Hayır. Bir kullanılabilirlik kümesindeki VM'ler, tüm yönetilen diskler veya tüm yönetilmeyen diskler kullanmanız gerekir. Bir kullanılabilirlik kümesi oluşturduğunuzda, kullanmak istediğiniz disk türünü seçebilirsiniz.

**Yönetilen diskler, Azure portalında varsayılan seçenektir?**

Evet. 

**Boş bir yönetilen disk oluşturabilir miyim?**

Evet. Boş bir disk oluşturabilirsiniz. Yönetilen disk sanal Makineye eklemeden bağımsız olarak bir VM, örneğin, oluşturulabilir.

**Ne desteklenen hata etki alanı sayısı kullanılabilirlik için yönetilen diskler kullanan ayarlanır?**

Yönetilen diskler kullanan bir kullanılabilirlik kümesi bulunduğu bağlı olarak, desteklenen hata etki alanı sayısı 2 veya 3 bölgedir.

**Standart depolama hesabı tanılama ayarlamak için nasıl mi?**

Bir özel depolama hesabı için VM tanılamayı ayarlayın.

**Ne tür bir rol tabanlı erişim denetimi desteğini yönetilen diskler için kullanılabilir mi?**

Diskleri destekleyen üç anahtar varsayılan rol yönetilen:

* Sahibi: erişim dahil her şeyi yönetebilir
* Katkıda bulunan: erişim dışında her şeyi yönetebilir
* Okuyucu: her şeyi görüntüleyebilir ancak değişiklik yapamaz

**Ben kopyalayabilir veya yönetilen disk bir özel depolama hesabına aktarın, bir yolu var mı?**

Yönetilen diskin salt okunur bir paylaşılan erişim imzası (SAS) URI oluşturmak ve özel depolama hesabı veya şirket içi depolama içeriklerini kopyalamak için kullanın. Azure portalı, Azure PowerShell, Azure CLI kullanarak SAS URI'sini kullanabilirsiniz veya [AzCopy](../articles/storage/common/storage-use-azcopy.md)

**Yönetilen diskimi bir kopyasını oluşturabilir miyim?**

Müşteriler, yönetilen disk anlık görüntüsünü alın ve sonra başka bir yönetilen disk oluşturmak için anlık görüntüyü kullanın.

**Yönetilmeyen diskler hala destekleniyor mu?**

Evet, yönetilmeyen ve yönetilen diskleri desteklenir. Yeni iş yükleri için yönetilen diskleri kullanma ve geçerli iş yüklerinizi yönetilen disklere geçirme öneririz.


**128 GB disk oluşturun ve ardından 130 GB'a artırırsanız, sonraki disk boyutu (256 GB) ücretlendirilirim?**

Evet.

**Yerel olarak yedekli depolama, coğrafi olarak yedekli depolama, oluşturabiliyorum ve bölgesel olarak yedekli depolama yönetilen diskleri?**

Azure yönetilen diskler, şu anda yalnızca yerel olarak yedekli depolama yönetilen diskleri destekler.

**Küçültme veya miyim yönetilen disklerim downsize?**

Hayır. Bu özellik şu anda desteklenmiyor. 

**My disk üzerinde bir kira sonu?**

Hayır. Bir kira disk kullanıldığında yanlışlıkla silinmesini önlemek için mevcut olduğundan bu şu anda desteklenmiyor.

**Bilgisayar adı özelliği değiştirmek özelleştirilmiş (sistem hazırlığı aracını kullanarak oluşturduğunuz veya genelleştirilmiş) işletim sistemi diski bir sanal makine sağlamak için kullanılır?**

Hayır. Bilgisayar adı özelliği güncelleştirilemiyor. Yeni sanal makine işletim sistemi diski oluşturmak için kullanılan sanal makine üst öğeden devralır. 

**Vm'leri yönetilen disklerle oluşturmak için Azure Resource Manager şablonları kullanarak nerede bulabilirim?**
* [Yönetilen Diskler'i kullanarak şablonları listesi](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**Yönetilmeyen ve yönetilen diskleri aynı VM'de birlikte bulabilirim?**

Hayır.

## <a name="standard-ssd-disks-preview"></a>Standart SSD disk (Önizleme)

**Azure standart SSD disk nelerdir?**
Standart SSD disk olarak daha düşük IOPS düzeylerinde tutarlı bir performans gerektiren iş yükleri için düşük maliyetli depolama için iyileştirilmiş katı hal medya tarafından desteklenen standart disklerdir. Önizleme aşamasında oldukları bölgesiyle sınırlı yönetilebilirlik (Resource Manager şablonları kullanılabilir) sınırlı sayıda kullanılabilir.

<a id="standard-ssds-azure-regions"></a>**Standart SSD disk için (Önizleme) şu anda desteklenen bölgeler nelerdir?**
* Kuzey Avrupa
* Fransa Orta
* Doğu ABD 2
* Orta ABD
* Orta Kanada
* Doğu Asya
* Kore Güney
* Avustralya Doğu

**Standart SSD disk nasıl oluşturulur?**
Şu anda, Azure Resource Manager şablonlarını kullanarak standart SSD disk oluşturabilirsiniz. Standart SSD disk oluşturmak için Resource Manager şablonunda gereken parametreleri aşağıdadır:

* *apiVersion* Microsoft.Compute olarak ayarlanması için `2018-04-01` (veya üzeri)
* Belirtin *managedDisk.storageAccountType* olarak `StandardSSD_LRS`

Aşağıdaki örnekte gösterildiği *properties.storageProfile.osDisk* bölüm standart SSD diskleri kullanan bir VM için:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Standart SSD disk ile bir şablon oluşturma tam şablon örneği için bkz [standart SSD veri diskleri ile bir Windows görüntüsünden VM oluşturma](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/).

**Standart SSD için mevcut disklerim dönüştürebilir miyim?**
Evet, uygulayabilirsiniz. Başvurmak [dönüştürme Azure yönetilen diskler depolama standart, premium ve tersi](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/convert-disk-storage) yönetilen diskleri dönüştürme için genel yönergeler için. Ve için standart SSD disk türünü güncelleştirmek için aşağıdaki değeri kullanın.
-AccountType StandardSSD_LRS

**Standart SSD yönetilmeyen diskler kullanabilir miyim?**
Standart SSD disk Hayır, yalnızca yönetilen diskler olarak kullanılabilir.

**Standart SSD disk "Tek Örnekli sanal makine SLA" destekliyor musunuz?**
Hayır, standart SSD'ler Tek Örnekli sanal makine SLA'sı yoktur. Premium SSD diskleri tek örnek sanal makine SLA'sını kullanın.

## <a name="migrate-to-managed-disks"></a>Yönetilen Disklere geçme 

**Hangi değişiklikleri bir önceden var olan Azure Backup hizmeti yapılandırma önceki/sonraki yönetilen Diskler'e geçiş gerekli midir?**

Değişiklik, gerekmez. 

**Azure Backup hizmeti geçiş işleminden önce aracılığıyla oluşturulan VM yedeklemelerim çalışmaya devam eder mi?**

Evet, yedeklemeler sorunsuz çalışır.

**Hangi değişiklikleri bir önceden var olan Azure disk şifrelemesi yapılandırma önceki/sonraki yönetilen Diskler'e geçiş gerekli midir?**

Değişiklik, gerekmez. 

**Otomatik geçişi mevcut bir sanal makine ölçek kümeleri yönetilmeyen disklerden yönetilen disklere desteklenir mi?**

Hayır. Yeni bir ölçek kümesi yönetilen diskler ile yönetilmeyen diskler eski ölçek kümenizi görüntüyü kullanarak oluşturabilirsiniz. 

**Yönetilen disklere geçirmeden önce geçen sayfa blob anlık görüntüden yönetilen Disk oluşturabilir miyim?**

Hayır. Sayfa blob anlık görüntüsü bir sayfa blobu dışarı aktarma ve yönetilen bir Disk dışarı aktarılan sayfa blob'u oluşturun. 

**Yönetilen disklerle bir VM için Azure Site Recovery tarafından korunan şirket içi makinelerime üzerinden başarısız olabilir?**

Evet, yönetilen disklerle bir VM için yük devretme için seçebilirsiniz.

**Azure'dan Azure'a çoğaltma Azure Site Recovery tarafından korunan Azure vm'lerinde geçişin herhangi bir etkisi var mı?**

Evet. Yönetilen disklere sahip VM'ler için Azure Site Recovery Azure'a korumayı şu anda yalnızca bir genel Önizleme hizmeti olarak olarak kullanılabilir.

**Veya yönetilen diskleri daha önce şifrelenmiş depolama hesaplarında yer alan yönetilmeyen disklere sahip Vm'leri geçirebilirim?**

Evet

## <a name="managed-disks-and-storage-service-encryption"></a>Yönetilen diskler ve depolama hizmeti şifrelemesi 

**Yönetilen disk oluşturduğumda Azure depolama hizmeti şifrelemesi varsayılan olarak etkin mi?**

Evet.

**Şifreleme anahtarları yöneten?**

Microsoft, şifreleme anahtarları yönetir.

**Ben depolama hizmeti şifrelemesi için yönetilen disklerim devre dışı bırakabilirim?**

Hayır.

**Depolama hizmeti şifrelemesi, yalnızca belirli bölgelerde kullanılabilir?**

Hayır. Yönetilen diskler kullanılabilir olduğu tüm bölgelerde kullanılabilir. Yönetilen diskler, kullanılabilir tüm genel bölgelerde ve Almanya içinde. Yalnızca Microsoft anahtarları, yönetilmeyen için aynı zamanda Çin'de, ancak kullanılabilir müşteri tarafından yönetilen anahtarlar.

**Nasıl miyim yönetilen diskimi şifrelenmiş olup olmadığını öğrenebilirsiniz?**

Azure portalı, Azure CLI ve PowerShell yönetilen disk ne zaman oluşturulduğu zaman bulabilirsiniz. Ardından süresi 9 Haziran 2017'den sonra ise, disk şifrelenir.

**10 Haziran 2017'den önce oluşturulan mevcut disklerim nasıl şifreleyebilir mi?**

10 Haziran 2017'den itibaren mevcut yönetilen disklere yazılan yeni veriler otomatik olarak şifrelenir. Biz de mevcut verileri şifrelemek planlıyor ve şifreleme zaman uyumsuz olarak arka planda gerçekleşir. Artık mevcut verileri şifrelemeniz gerekir, diskinin bir kopyasını oluşturun. Yeni disk şifrelenir.

* [Azure CLI kullanarak yönetilen diskleri kopyalama](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [PowerShell kullanarak yönetilen diskleri kopyalama](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

**Yönetilen anlık görüntüler ve görüntüler şifrelenir?**

Evet. Tüm yönetilen anlık görüntüler ve resimler 9 Haziran 2017'den sonra oluşturulan otomatik olarak şifrelenir. 

**Vm'leri yönetilen disklere daha önce şifrelenmiş veya depolama hesaplarında bulunan yönetilmeyen diskler içeren dönüştürebilirsiniz?**

Evet

**Bir dışarı aktarılan VHD'den yönetilen disk veya anlık görüntüsünü de şifrelenir mi?**

Hayır. Ancak bir VHD için şifrelenmiş depolama hesabında bir şifrelenmiş dışarı, disk veya anlık görüntü yönetilen ardından şifrelenir. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premium diskler: yönetilen ve yönetilmeyen

**Bir VM boyutu serisi bir DSv2 gibi Premium SSD diskleri destekler kullanıyorsa miyim hem premium hem de standart veri diski ekleyebilir miyim?** 

Evet.

**D, Dv2, G veya F serisi gibi Premium SSD diskleri desteklemiyor boyutu serisi için hem premium hem de standart veri diskleri ekleyebilirsiniz miyim?**

Hayır. Premium SSD diskleri destekler boyutu serisi kullanmayan Vm'leri için yalnızca standart veri diskleri ekleyebilirsiniz.

**80 GB olan mevcut bir VHD'den premium veri diski oluşturursanız, ne kadar maliyeti ne olacak?**

80 GB'lık bir VHD'den oluşturulan premium veri diski bir P10 disk sonraki kullanılabilir premium disk boyutu kabul edilir. P10 disk fiyatlarına göre ücretlendirilirsiniz.

**Premium SSD diskleri kullanmak için işlem ücreti var mıdır?**

Her disk boyutu, IOPS ve aktarım hızı belirli sınırları ile sağlanan birlikte gelen sabit bir maliyeti yoktur. Diğer giden bant genişliğini ve anlık görüntü kapasitesi varsa ücretlerdir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**IOPS ve disk önbellekten alabilirim işleme sınırları nelerdir?**

Önbellek için birleşik sınırları ve DS serisi için yerel SSD 4000 IOPS çekirdek başına çekirdek başına saniyede 33 MB içindir. GS serisi, çekirdek başına 5.000 IOPS ve çekirdek başına saniyede 50 MB sunar.

**Yerel SSD için yönetilen diskler VM destekleniyor mu?**

Yerel SSD ile yönetilen diskler VM içerdiği geçici depolamadır. Herhangi bir ek bu geçici depolama maliyeti yoktur. Azure Blob depolamada kalıcı değil çünkü uygulama verilerinizi depolamak için bu yerel SSD kullanmamanızı öneririz.

**Premium disklerde TRIM kullanımı için herhangi bir varsa var mı?**

TRIM Azure disklerde, premium veya standart diskler kullanımı için herhangi bir dezavantajı vardır.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Yeni disk boyutları: yönetilen ve yönetilmeyen

**İşletim sistemi ve veri diskleri için desteklenen en büyük disk boyutu nedir?**

Azure destekleyen bir işletim sistemi diski için bölüm ana önyükleme kaydı (MBR) türüdür. Diskin MBR biçimini destekleyen en fazla 2 TB boyut. Azure destekleyen bir işletim sistemi diski için en büyük boyut 2 TB'dir. Azure veri diskleri için 4 TB'a kadar destekler. 

**Desteklenen en büyük sayfa blob boyutu nedir?**

Azure'un desteklediği en büyük sayfa blob boyutu 8 TB (8191 GB) ' dir. Bir VM'ye veri veya işletim sistemi diskleri olarak bağlanıldığında en fazla sayfa blog (4.095 GB) 4 TB boyutudur.

**Azure Araçları'nın yeni bir sürüm oluşturma, ekleme, yeniden boyutlandırma ve 1 TB'den büyük diskleri karşıya yükleme için kullanılacak gerekiyor mu?**

Oluşturma, ekleme veya 1 TB'den büyük diskleri yeniden boyutlandırmak için mevcut Azure araçlarınızı yükseltmeniz gerekmez. Bir sayfa blobu veya yönetilmeyen disk olarak doğrudan azure'a şirket içi, VHD dosyası yüklemek için en son araç kümeleri kullanmanız gerekir:

|Azure Araçları      | Desteklenen sürümler                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Sürüm numarası 4.1.0: Haziran 2017 sürümü veya üzeri|
|Azure CLI v1     | Sürüm numarası 0.10.13: Mayıs 2017 sürümü veya üzeri|
|AzCopy           | Sürüm numarası 6.1.0: Haziran 2017 sürümü veya üzeri|

Azure CLI v2 ve Azure Depolama Gezgini için destek yakında sunulacaktır. 

**P4 ve P6 disk boyutları, yönetilmeyen diskler veya sayfa blobları için destekleniyor mu?**

Hayır. P4 (32 GB) ve P6 (64 GB) disk boyutları, yalnızca yönetilen diskler için desteklenir. Yönetilmeyen diskler ve sayfa blobları için destek yakında sunulacaktır.

**Nasıl daha az 64 GB (Haziran 15 2017) küçük disk etkinleştirilmeden önce oluşturulan mevcut my premium yönetilen disk, faturalandırılır?**

Var olan küçük premium P10 fiyatlandırma katmanına göre faturalandırılmaya devam 64 GB değerinden diskler. 

**P4 veya P6 64 GB P10 ile değerinden daha küçük premium diskler, disk katmanı nasıl geçiş yapabilirim?**

Küçük disklerinizi anlık görüntüsünü alın ve ardından otomatik olarak fiyatlandırma katmanını P4 veya P6 sağlanan boyutuna göre geçiş yapmak için bir disk oluşturun. 


## <a name="what-if-my-question-isnt-answered-here"></a>Peki sorumun cevabı burada bulamadığınız?

Sorunuzu burada listelenmiyorsa, bize bildirin ve yanıt bulmanıza yardımcı olacağız. Açıklamalar bu makalenin sonunda bir soru gönderebilir. Azure depolama ekibi ve diğer topluluk üyelerinin bu makale hakkında ile etkileşim kurmak amacıyla MSDN kullanın [Azure depolama Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).

İstek özellikleri için fikir ve istek göndermek için [Azure Depolama'ya geri bildirim Forumu](https://feedback.azure.com/forums/217298-storage).
