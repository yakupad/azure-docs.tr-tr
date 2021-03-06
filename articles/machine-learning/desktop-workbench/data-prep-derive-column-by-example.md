---
title: Azure Machine Learning Workbench'i kullanarak örnek dönüştürme tarafından sütunu türetin
description: Başvuru belgesini 'Sütunu örneğe göre türet' dönüştürme için
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: 026ffed925606e2fdf31461035c9a0d73ad609e9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060575"
---
# <a name="derive-column-by-example-transformation"></a>Örnek dönüştürme tarafından sütunu türetin

**Sütunu örneğe göre türet** dönüştürme kullanıcıların bir kullanıcı tarafından sağlanan türetilen sonucu örneklerini kullanarak bir veya daha fazla mevcut sütunların türevi oluşturmasını sağlar. Türev desteklenen dizesi, tarih ve sayı dönüştürmeleri herhangi bir birleşimi olabilir. 

Dize, tarih ve sayı dönüştürmeleri desteklenir:

**Dize dönüştürme seçenekleri:** 

Akıllı ayıklama numarası ve tarihler, birleştirme, durum işleme, eşleme sabit değerler dahil olmak üzere bir alt dize.

**Tarih dönüştürme seçenekleri:** 

Tarih kısımlarını ayıklanması, eşleme zaman zaman depo tarih biçimini değiştirin.

Tarih dönüştürmeleri oldukça ile bazı önemli sınırlamalar genel şunlardır:
* Saat dilimleri desteklenmez.
* Desteklenmeyen bazı ortak biçimleri:
    * ISO 8601 yıl biçimini (örneğin "2009-W53-7") haftası 
    * UNIX dönem zamanı.
* Tüm biçimler büyük/küçük harfe duyarlıdır ("4 am" özellikle tanınmıyor olarak bir saat olsa da "04: 00").

**Sayı dönüştürmeleri:** 

Round, Floor, Ceiling, gruplama, sıfır veya boşluk, bölme veya 1000 gücünü çarpım doldurma.

**Bileşik dönüşümler:** 

Herhangi bir birleşimini dize, sayı veya tarih dönüştürmeleri.

## <a name="how-to-use-this-transformation"></a>Bu dönüşüm kullanma

Bu dönüştürme işlemini gerçekleştirmek için şu adımları izleyin:
1. Değeri türetmek için istediğiniz bir veya daha fazla sütun seçin. 
2. Seçin **sütunu örneğe göre türet** gelen **dönüştüren** menüsü. Ya da seçin ve seçili sütunların hiçbirinin başlığına sağ tıklayın **sütunu örneğe göre türet** bağlam menüsünden. Dönüştürme Düzenleyicisi açılır ve sağ en seçili olan sütunda yanındaki yeni bir sütun eklenir. Seçili sütunlar, sütunun üstbilgisinde onay kutularını tarafından tanımlanabilir. Sütun başlıklarını onay kutularını kullanarak eklenmesini ve kaldırılmasını sütun seçimi yapılabilir.
3. Örneği yazın *çıkış* bir satır ve ENTER tuşuna karşı girin. Bu noktada, Workbench, çıkış belirtilen girişleri dönüştüren bir program sentezlemek için sağlanan çıkış yanı sıra giriş sütunundaki analiz eder. Sentezlenen programı uygun bulduğu veri kılavuzunda tüm satırlara göre yürütülür. Belirsiz ve karmaşık durumlarda, birden çok örnek gerekebilir. Temel modda veya Gelişmiş modda olmanıza bağlı olarak, farklı şekilde birden fazla örnek sağlanabilir.
4. ' A tıklayın ve çıkış gözden **Tamam** dönüştürmeyi kabul etmek için.

### <a name="transform-editor-basic-mode"></a>Düzenleyici dönüştürme: Temel mod

Satır içi düzenleme deneyimi veri kılavuzunda temel modu sağlar. İlgilenilen hücresi için gezinme ve bir değer yazarak, çıkış örnekleri sağlayabilirsiniz. 

Workbench, verileri analiz eder ve kullanıcı tarafından incelenmelidir istisnai durumlara tanımlamayı dener. Veriler analiz ederken **veriler analiz ediliyor** dönüştürme Düzenleyicisi üst bilgisinde gösterilir. Bir analiz tamamlandığında ya da **Hayır önerileri** veya **gözden geçirme önerilen sonraki satırı** başlığı görüntülenir. İstisnai durumlara tıklayarak gidebilirsiniz **gözden geçirme önerilen sonraki satırı**. Değer için bir satır yanlış olması durumunda, ek örnek olarak doğru değeri anahtar. 

### <a name="transform-editor-advanced-mode"></a>Düzenleyici dönüştürme: Gelişmiş mod

Gelişmiş mod sütunları örneğe göre türetme için daha zengin bir deneyim sağlar. Tek bir yerde tüm örnekler gösterilmektedir. Tek bir yerde tüm istisnai durumlara tıklayarak da gözden geçirebilirsiniz **önerilen örnekler**. 

Gelişmiş modda kılavuzda bir satıra çift tıklayarak herhangi bir satır bir örnek satır ekleyebilirsiniz. Bir satır bir örnek satır kopyalanır, yapay bir örneğin çalışması için kaynak sütunlardaki verileri de düzenleyebilirsiniz. Bunu yaptığınızda, örnek verileri şu anda mevcut olmayan çalışmalarını ekleyebilirsiniz.

Kullanıcı arasında geçiş yapmak **temel mod** ve **Gelişmiş mod** dönüştürme Düzenleyicisi bağlantıları tıklayarak.

### <a name="transform-editor-send-feedback"></a>Düzenleyici dönüştürme: geri bildirim gönder

Tıklayarak **geri bildirim gönder** bağlantı açar **geri bildirim** örnekler kullanıcıyla önceden doldurulmuş açıklamaları kutusu iletişim sağlanan. Kullanıcı yorumları kutunun içeriğini gözden geçirmeniz ve sorunu anlamanıza yardımcı olmak için daha fazla ayrıntı sağlanır. Kullanıcı verilerini Microsoft ile paylaşmak istemediği, kullanıcı önceden doldurulmuş bir örnek veri tıklamadan önce silmelisiniz **geri bildirim gönder** düğmesi. 

### <a name="editing-existing-transformation"></a>Var olan dönüştürme düzenleme

Varolan bir kullanıcı düzenleyebilir **örnekle sütunu türetin** Dönüştür seçip **Düzenle** dönüştürme adımı seçeneği. Tıklayarak **Düzenle** dönüştürme Düzenleyicisi'nde açılır **Gelişmiş mod**, ve dönüştürme, oluşturma sırasında sağlanan tüm örnekler gösterilmektedir.

## <a name="examples-of-string-transformations-by-example"></a>Örneğe göre dize dönüşümleri örnekleri


>[!NOTE] 
>Değerler **kalın** gösterilen veri dönüşümü tamamlamak için sağlanan örnekleri temsil eder.


### <a name="s1-extracting-file-names-from-file-paths"></a>S1. Dosya yolları dosyası adları çıkartılırken

Bu çalışması için gerekli örnek sayısı: 2

|Girdi|Çıktı|
|:-----|:-----|
|C:\Python35\Tools\pynche\TypeinViewer.PY|**TypeinViewer.py**|
|C:\Python35\Tools\pynche\webcolors.txt|webcolors.txt|
|C:\Python35\Tools\pynche\websafe.txt|websafe.txt|
|C:\Python35\Tools\pynche\X\rgb.txt|RGB.txt|
|C:\Python35\Tools\pynche\X\xlicense.txt|xlicense.txt|
|C:\Python35\Tools\Scripts\2to3.PY|2to3.PY|
|C:\Python35\Tools\Scripts\analyze_dxp.PY|**analyze_dxp.PY**|
|C:\Python35\Tools\Scripts\byext.PY|byext.PY|
|C:\Python35\Tools\Scripts\byteyears.PY|byteyears.PY|
|C:\Python35\Tools\Scripts\checkappend.PY|checkappend.PY|

### <a name="s2-case-manipulation-during-string-extraction"></a>S2. Dize ayıklama sırasında büyük/küçük harf işleme

Bu çalışması için gerekli örnek sayısı: 3

|Girdi|Çıktı|
|:-----|:-----|
|U & ÇIKMAZ ren GEYİKLERİ;  YENİ HANOVER; İstasyon 332; 2015-12-10 \@ 17:10:52;|**Yeni Hanover**|
|BRIAR yolu & WHITEMARSH LN;  HATFIELD TOWNSHIP; İstasyon 345; 2015-12-10 \@ 17:29:21;|Hatfield Township|
|HAWS AVE; NORRISTOWN; 2015-12-10 \@ 14:39:21-istasyon: STA27;|**Norristown**|
|AIRY ST & SWEDE ST;  NORRISTOWN; İstasyon 308A; 2015-12-10 \@ 16:47:36;|**Norristown**|
|Kiraz AĞACI CT & ÇIKMAZ;  DAHA DÜŞÜK POTTSGROVE; İstasyon 329; 2015-12-10 \@ 16:56:52;|Daha düşük Pottsgrove|
|FIRLATICIYI AVE & W 9 ST;  LANSDALE; İstasyon 345; 2015-12-10 \@ 15:39:04;|Lansdale|
|DEFNE AVE & OAKDALE AVE;  HORSHAM; İstasyon 352; 2015-12-10 \@ 16:46:48;|Horsham|
|COLLEGEVILLE RD & LYWISKI RD;  SKIPPACK; İstasyon 336; 2015-12-10 \@ 16:17:05;|Skippack|
|MAIN ST & eski SUMNEYTOWN PİKES;  DAHA DÜŞÜK SALFORD; İstasyon 344; 2015-12-10 \@ 16:51:42;|Daha düşük Salford|
|BLUEROUTE &AMP; RAMP I476 NB KİMYASAL RD İÇİN; PLYMOUTH; 2015-12-10 \@ 17:35:41;|Plymouth|
|RT202 PKWY &AMP; KNAPP RD; MONTGOMERY; 2015-12-10 \@ 17:33:50;|Montgomery|
|TATLI RD &AMP; COLWELL LN; PLYMOUTH; 2015-12-10 \@ 16:32:10;|Plymouth|

### <a name="s3-date-format-manipulation-during-string-extraction"></a>S3. Tarih biçimi düzenleme dize ayıklama sırasında

Bu çalışması için gerekli örnek sayısı: 1

|Girdi|Çıktı|
|:-----|:-----|
|MONTGOMERY AVE & WOODSIDE RD;  DAHA DÜŞÜK MERION; İstasyon 313; 2015-12-11 \@ 04:11:35;|**12 Kasım 2015 04: 00**|
|DREYCOTT LN & W LANCASTER AVE;  DAHA DÜŞÜK MERION; İstasyon 313; 2015-12-11 \@ 01:29:52;|12 Kasım 2015'te 1|
|&AMP; E LEVERING DEĞİRMEN RD CONSHOHOCKEN DURUMU RD; DAHA DÜŞÜK MERION; 2015-12-11 \@ 07:29:58;|12 Kasım 2015'te 7|
|& Da VADİSİ RD MANOR RD;  DAHA DÜŞÜK MERION; İstasyon 313; 2015-12-10 \@ 20:53:30;|12 Ekim 2015'te 8|
|BELMONT AVE &AMP; OVERHILL RD; DAHA DÜŞÜK MERION; 2015-12-10 \@ 23:02:27;|12 Ekim 2015 23: 00|
|W MONTGOMERY AVE &AMP; PENNSWOOD RD; DAHA DÜŞÜK MERION; 2015-12-10 \@ 19:25:22;|12 Ekim 2015 19: 00|
|ROSEMONT AVE & ÇIKMAZ;  DAHA DÜŞÜK MERION; İstasyon 313; 2015-12-10 \@ 18:43:07;|12 Ekim 2015 18: 00|
|AVIGNON Sü & ÇIKMAZ; DAHA DÜŞÜK MERION; 2015-12-10 \@ 20:01:29-istasyon: STA24;|12 Ekim 2015'te 8|

### <a name="s4-concatenating-strings"></a>S4. Dizeleri birleştirme

Bu çalışması için gerekli örnek sayısı: 1

>[!NOTE] 
>Bu örnekte, özel karakter · Çıkış sütundaki alanları temsil eder.

|Ad|İkinci Ad|Soyadı|Çıktı|
|:-----|:-----|:-----|:-----|
|Laquanda||Lohmann|Laquanda·· Lohmann|
|Claudio|A|Chew|**Claudio· A· Chew**|
|Sarah Jane|S|Smith|Sarah Jane· S· Smith|
|Brandi||Blumenthal|Brandi·· Blumenthal|
|Jesusita|R|Yolculuğu|Jesusita· R· Yolculuğu|
|Hermina||Hults|Hermina·· Hults|
|Anne Başak|W|Jones|Anne Marie· W· Jones|
|Riko||Ropp|Rico·· Ropp|
|Lauren Mayıs||Fullmer|Lauren May·· Fullmer|
|Marc|T|Maine|Marc· T· Maine|
|Angie||Adelman|Angie·· Adelman|
|John Paul||Smith|John Paul·· Smith|
|Şarkı|W|Staller|Song· W· Staller|
|Jill||Jefferies|Jill·· Jefferies|
|Ruby-kullanım|M|Nasıl yapılacağını|Ruby Grace· M· Nasıl yapılacağını|

### <a name="s5-generating-initials"></a>S5. Baş harflerini oluşturuluyor

Bu çalışması için gerekli örnek sayısı: 2

|Tam Ad|Çıktı|
|:-----|:-----|
|Laquanda Lohmann|**L.L.**|
|Claudio Chew|C.C.|
|Sarah Jane Smith|S.S.|
|Brandi Blumenthal|B.B.|
|Jesusita yolculuğu|J.J.|
|Hermina Hults|H.H.|
|Anne Marie Jones|A.J.|
|Riko Ropp|R.R.|
|Lauren Mayıs Fullmer|L.F.|
|Marc Maine|M.M.|
|Angie Adelman|A.A.|
|John Paul Smith|**J.S.**|
|Şarkı Staller|S.S.|
|Jill Jefferies|J.J.|
|Ruby yetkisiz nasıl yapılacağını|R.S.|


### <a name="s6-mapping-constant-values"></a>S6. Eşleme sabit değerler

Bu çalışması için gerekli örnek sayısı: 3

|Yönetim cinsiyet|Çıktı|
|:-----|:-----:|
|Erkek|**0**|
|Kadın|**1**|
|Bilinmeyen|**2**|
|Kadın|1|
|Kadın|1|
|Erkek|0|
|Bilinmeyen|2|
|Erkek|0|
|Kadın|1|

## <a name="examples-of-number-transformations-by-example"></a>Örneğe göre sayı dönüştürmeleri örnekleri

>[!NOTE] 
>Değerler **kalın** gösterilen veri dönüşümü tamamlamak için sağlanan örnekleri temsil eder.


### <a name="n1-rounding-to-nearest-10"></a>N1. En yakın 10 yuvarlama

Bu çalışması için gerekli örnek sayısı: 1

|Girdi|Çıktı|
|-----:|-----:|
|112|**110**|
|117|120|
|11112|11110|
|11119|11120|
|548|550|

### <a name="n2-rounding-down-to-nearest-10"></a>N2. En yakın 10 aşağı yuvarlama

Bu çalışması için gerekli örnek sayısı: 2

|Girdi|Çıktı|
|-----:|-----:|
|112|**110**|
|117|**110**|
|11112|11110|
|11119|11110|
|548|540|

### <a name="n3-rounding-to-nearest-005"></a>N3. En yakın 0.05 için yuvarlama

Bu çalışması için gerekli örnek sayısı: 2

|Girdi|Çıktı|
|-----:|-----:|
|-75.5812935|**-75.60**|
|-75.2646799|-75.25|
|-75.3519752|-75.35|
|-75.343513|**-75.35**|
|-75.6033497|-75.60|
|-75.283245|-75.30|

### <a name="n4-binning"></a>N4. Gruplama

Bu çalışması için gerekli örnek sayısı: 1

|Girdi|Çıktı|
|-----:|:-----:|
|20.16|**20-25**|
|14.32|10-15|
|5.44|5-10|
|3.84|0-5|
|3.73|0-5|
|7.36|5-10|

### <a name="n5-scaling-by-1000"></a>N5. 1000 ile ölçeklendirme

Bu çalışması için gerekli örnek sayısı: 1

|Girdi|Çıktı|
|-----:|-----:|
|-243|**-243000**|
|-12.5|-12500|
|-2345.23292|-2345232.92|
|-1202.3433|-1202343.3|
|1202.3433|1202343.3|

### <a name="n6-padding"></a>N6. Doldurma

Bu çalışması için gerekli örnek sayısı: 1

|Kod|Çıktı|
|-----:|-----:|
|5828|**05828**|
|44130|44130|
|49007|49007|
|29682|29682|
|4759|04759|
|10029|10029|
|7204|07204|

## <a name="examples-of-date-transformations-by-example"></a>Örneğe göre tarih dönüştürmeleri örnekleri

>[!NOTE] 
>Değerler **kalın** gösterilen veri dönüşümü tamamlamak için sağlanan örnekleri temsil eder.


### <a name="d1-extracting-date-parts"></a>D1. Ayıklanan tarih kısımlarını

Bu tarih kısımlarını farklı olarak örnek dönüşümlere kullanarak aynı veri kümesini temel ayıklanan. Kalın dizeleri, ilgili dönüşümü verilen örnekler temsil eder.

|DateTime|Haftanın günü|Tarih|Ay|Yıl|Saat|Dakika|Saniye|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|31 Ocak 2031 05:54:18|**Cuma**|**31**|**Jan**|**2031**|**5**|**54**|**18**|
|17 Oca 1990 13:32:01|Çar|17|Oca|1990|13|32|01|
|14 Şubat 2034 05:36:07|Sal|14|Şub|2034|5|36|07|
|14 Mart'ta 2002 13:16:16|Per|14|Mar|2002|13|16|16|
|21 Oca 1985 05:44:43|Pzt|21|Oca|1985|5|44|**43**|
|16 Ağu 1985 01:11:56|Cum|16|Ağu|1985|1|11|56|
|20-Ara 12.00-2033 yılına 18:36:29|Sal|20|Ara|2033|18|36|29|
|16 Tem 1984 10:21:59|Pzt|16|Tem|1984|10|21|59|
|13 Ocak 2038 10:59:36|Çar|13|Oca|2038|10|59|36|
|14 Ağu 1982 15:13:54|Cmt|14|Ağu|1982|15|13|54|
|22 Kasım 2030 08:18:08|Cum|22|Kas|2030|8|18|08|
|21 Ekim 1997 08:42:58|Sal|21|Eki|1997|8|42|58|
|28 Kasım 2006 14:19:15|Sal|28|Kas|2006|14|19|15|
|29 Apr 2031 04:59:45|Sal|29|Nis|2031|4|59|45|
|29 Ocak 2032 02:38:36|Per|29|Oca|2032|2|38|36|
|11 Mayıs 2028 15:31:52|Per|11|Mayıs|2028|15|31|52|
|15 Temmuz 1977 12:45:39|Cum|15|Tem|1977|12|45|39|
|27 Ocak 2029 05:55:41|Cmt|27|Oca|2029|5|55|41|
|03 Mar 2024 10:17:49|Paz|3|Mar|2024|10|17|49|
|14 Apr 2010 00:23:13|Çar|14|Nis|2010|0|23|13|

### <a name="d2-formatting-dates"></a>D2. Tarihleri biçimlendirme

Bu tarih formattings yapıldığını aynı veri kümesinde farklı örnek tarafından dönüştürmeleri kullanma. Kalın dizeleri, ilgili dönüşümü verilen örnekler temsil eder.

|DateTime|Format1|Format2|Format3|Format4|Format5|
|-----:|-----:|-----:|-----:|-----:|-----:|
|31 Ocak 2031 05:54:18|**1/31/2031**|**31 Ocak 2031, Cuma**|**01312031 5:54**|**1/31/2031 5:54 AM**|**S1 2031**|
|17 Oca 1990 13:32:01|1/17/1990|17 Ocak 1990, Çarşamba|01171990 13:32|17/1/1990 1:32 PM|S1 1990|
|14 Şubat 2034 05:36:07|2/14/2034|14 Şubat 2034 Salı|02142034 5:36|14/2/2034 5: 36'DA|S1 2034
|14 Mart'ta 2002 13:16:16|3/14/2002|14 Mart 2002, Perşembe|03142002 13:16|14/3/2002 13: 16'TE|S1 2002
|21 Oca 1985 05:44:43|1/21/1985|21 Ocak 1985, Pazartesi|01211985 5:44|21/1/1985 5:44 AM|S1 1985
|16 Ağu 1985 01:11:56|8/16/1985|16 Ağustos 1985, Cuma|08161985 1:11|8/16/1985 1:11:00|Q3 1985
|20-Ara 12.00-2033 yılına 18:36:29|12/20/2033|Salı, 20 Aralık, 2033 yılına|12202033 18:36|20/12/2033 YILINA 6:36 PM|S4 2033 YILINA
|16 Tem 1984 10:21:59|7/16/1984|16 Temmuz 1984, Pazartesi|07161984 10:21|7/16/1984 10:21:00|Q3 1984
|13 Ocak 2038 10:59:36|1/13/2038|13 Ocak 2038, Çarşamba|01132038 10:59|1/13/2038 10:59:00|S1 2038
|14 Ağu 1982 15:13:54|8/14/1982|14 Ağustos 1982 Cumartesi|08141982 15:13|14/8/1982 3: 13'TE|Q3 1982
|22 Kasım 2030 08:18:08|11/22/2030|22 Kasım 2030, Cuma|11222030 8:18|22/11/2030 8:18:00|S4 2030
|21 Ekim 1997 08:42:58|10/21/1997|21 Ekim 1997 Salı|10211997 8:42|21/10/1997'DEN 8:42:00|S4 1997'DEN
|28 Kasım 2006 14:19:15|11/28/2006|28 Kasım 2006, Salı|11282006 14:19|28/11/2006 2:19 PM|S4 2006
|29 Apr 2031 04:59:45|4/29/2031|29 Nisan 2031 Salı|04292031 4:59|29/4/2031 4: 59'DA|S2 2031
|29 Ocak 2032 02:38:36|1/29/2032|29 Ocak 2032, Perşembe|01292032 2:38|29/1/2032 2:38:00|S1 2032
|11 Mayıs 2028 15:31:52|5/11/2028|11 Mayıs 2028, Perşembe|05112028 15:31|5/11/2028 3:31 PM|S2 2028
|15 Temmuz 1977 12:45:39|7/15/1977|15 Temmuz 1977, Cuma|07151977 12:45|7/15/1977 12:45 PM|Q3 1977
|27 Ocak 2029 05:55:41|1/27/2029|27 Ocak 2029 Cumartesi|01272029 5:55|27/1/2029 5:55 AM|S1 2029
|03 Mar 2024 10:17:49|3/3/2024|3 Mart 2024, Pazar|03032024 10:17|3/3/2024 10:17:00|S1 2024
|14 Apr 2010 00:23:13|4/14/2010|14 Nisan 2010, Çarşamba|04142010 0:23|4/14/2010 12:23:00|S2 2010


### <a name="d3-mapping-time-to-time-periods"></a>D3. Eşleme zaman dönemleri

Bu tarih/saat dönem eşlemeleri yapıldığını aynı veri kümesinde farklı örnek tarafından dönüştürmeleri kullanma. Kalın dizeleri, ilgili dönüşümü verilen örnekler temsil eder.

|DateTime|Period(seconds)|Period(minutes)|Süre (iki saat)|Süre (30 dakika)|
|-----:|-----:|-----:|-----:|-----:|
|31 Ocak 2031 05:54:18|**0-20**|**45-60**|**5AM-7AM**|**5:30-6:00**|
|17 Oca 1990 13:32:01|**0-20**|30-45|1PM-3PM|13:30-14:00|
|14 Şubat 2034 05:36:07|0-20|30-45|5AM-7AM|5:30-6:00|
|14 Mart'ta 2002 13:16:16|0-20|15-30|1PM-3PM|13:00-13:30|
|21 Oca 1985 05:44:43|40-60|30-45|5AM-7AM|5:30-6:00|
|16 Ağu 1985 01:11:56|40-60|0-15|1AM-3AM|1:00-1:30|
|20-Ara 12.00-2033 yılına 18:36:29|20-40|30-45|5PM-7PM|18:30-19:00|
|16 Tem 1984 10:21:59|40-60|15-30|09: 00 - 11: 00|10:00-10:30|
|13 Ocak 2038 10:59:36|20-40|45-60|09: 00 - 11: 00|10:30-11:00|
|14 Ağu 1982 15:13:54|40-60|0-15|3PM-5PM|15:00-15:30|
|22 Kasım 2030 08:18:08|0-20|15-30|7AM-9AM|8:00-8:30|
|21 Ekim 1997 08:42:58|40-60|30-45|7AM-9AM|8:30-9:00|
|28 Kasım 2006 14:19:15|0-20|15-30|1PM-3PM|14:00-14:30|
|29 Apr 2031 04:59:45|40-60|45-60|3'TE - 5'TE|4:30-5:00|
|29 Ocak 2032 02:38:36|20-40|30-45|1AM-3AM|2:30-3:00|
|11 Mayıs 2028 15:31:52|40-60|30-45|3PM-5PM|15:30-16:00|
|15 Temmuz 1977 12:45:39|20-40|45-60|11AM-1PM|12:30-13:00|
|27 Ocak 2029 05:55:41|40-60|45-60|5AM-7AM|5:30-6:00|
|03 Mar 2024 10:17:49|40-60|15-30|09: 00 - 11: 00|10:00-10:30|
|14 Apr 2010 00:23:13|0-20|15-30|23: 00 - 01'DA|0:00-0:30|

## <a name="examples-of-composite-transformations-by-example"></a>Bileşik dönüşümler örnekleri örneğe göre

|tripduration|StartTime|start Station id|Station latitude Başlat|İstasyon boylam Başlat|UserType|Sütun|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|61|2016-01-08 16:09:32|107|42.3625|-71.08822|Abone|**Abone biriminden 107, lat/uzun (42.363,-71.088) 08 Ocak 2016 yaklaşık 4 PM, bisiklet Çekildi. Seyahat süresinin 61 dakika oluştu**|
|61|2016-01-17 09:28:10|74|42.373268|-71.118579|Müşteri|Müşterinin bir bisiklet biriminden 74, lat/uzun (42.373,-71.119) 17 Ocak 2016 yaklaşık 9'da Çekildi. Seyahat süresinin 61 dakika oluştu|
|62|2016-01-25 08:10:26|176|42.386748020450561|-71.119018793106079|Abone|Bir abonenin bir bisiklet biriminden 176, lat/uzun (42.387,-71.119) 25 Ocak 2016 yaklaşık 8 sabah Çekildi. Seyahat süresinin 62 dakika oluştu|
|63|2016-01-08 10:10:29|107|42.3625|-71.08822|Abone|Abone biriminden 107, lat/uzun (42.363,-71.088) 08 Ocak 2016 yaklaşık 10'da bir bisiklet süreniz uzar. Seyahat süresinin 63 dakika oluştu|
|64|2016-01-15 19:42:08|68|42.36507|-71.1031|Abone|Abone biriminden 68, lat/uzun (42.365,-71.103) 15 Ocak 2016 yaklaşık 7 PM, bisiklet Çekildi. Seyahat süresinin 64 dakika oluştu|
|64|2016-01-22 18:16:13|115|42.387995|-71.119084|Abone|Abone biriminden 115 lat/uzun (42.388,-71.119), 22 Ocak 2016 yaklaşık 6 PM, bisiklet Çekildi. Seyahat süresinin 64 dakika oluştu|
|68|2016-01-18 09:51:52|178|42.359573201090441|-71.101294755935669|Abone|Abone biriminden 178, lat/uzun (42.360,-71.101) 18 Ocak 2016 yaklaşık 9'da bir bisiklet süreniz uzar. Seyahat süresinin 68 dakika oluştu|
|69|2016-01-14 08:57:55|176|42.386748020450561|-71.119018793106079|Abone|Bir abonenin bir bisiklet biriminden 176, lat/uzun (42.387,-71.119) 14 Ocak 2016 yaklaşık 8 sabah Çekildi. Seyahat süresinin 69 dakika oluştu|
|69|2016-01-13 22:12:55|141|42.363560158429884|-71.08216792345047|Abone|Abone biriminden 141, lat/uzun (42.364,-71.082) 13 Ocak 2016 yaklaşık 10 PM, bisiklet süreniz uzar. Seyahat süresinin 69 dakika oluştu|
|69|2016-01-15 08:13:09|176|42.386748020450561|-71.119018793106079|Abone|Bir abonenin bir bisiklet biriminden 176, lat/uzun (42.387,-71.119) 15 Ocak 2016 yaklaşık 8 sabah Çekildi. Seyahat süresinin 69 dakika oluştu|


## <a name="technical-notes"></a>Teknik notlar

### <a name="conditional-transformations"></a>Koşullu dönüşümleri
Bazı durumlarda, belirli örnekler karşılayan tek bir dönüştürmeyi bulunamıyor. Böyle durumlarda, bazı deseni temel alınarak girişleri grup ve her grup için ayrı dönüştürme öğrenmek sütunu örneğe göre örnek dönüştürme türet çalışır. Bu diyoruz **koşullu dönüştürme**. **Koşullu dönüştürme** yalnızca tek bir giriş sütun ile dönüştürmeler için denenir. 

### <a name="reference"></a>Başvuru
Örnek teknoloji dize dönüşümü hakkında daha fazla bilgi bulunabilir [bu yayında](https://www.microsoft.com/research/publication/automating-string-processing-spreadsheets-using-input-output-examples/).
