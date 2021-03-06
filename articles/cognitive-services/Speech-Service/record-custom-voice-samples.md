---
title: Özel ses oluşturma için ses kaydetme örnekleri hakkında
titleSuffix: Microsoft Cognitive Services
description: Üretim kalitesindeki costum ses, sağlam bir komut dosyası hazırlanıyor, iyi sesli beceri işe ve profesyonel kaydını yapın.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/5/2018
ms.author: v-jerkin
ms.openlocfilehash: 80b65b8bc2e2947575004269f45922e2306dd32e
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284175"
---
# <a name="how-to-record-voice-samples-for-a-custom-voice"></a>Nasıl yapılır özel sesli için ses kaydetme örnekleri

Yüksek kaliteli üretim özel sesli sıfırdan oluşturma, sıradan bir iş değildir. Özel ses bileşeni İnsan konuşma ses örnekleri büyük bir koleksiyonudur. Bu ses kayıtları yüksek kaliteli olması önemlidir. Bu tür bir kayıt yapma deneyimi ses beceri seçin ve bunları profesyonel ekipman kullanarak yetkin kaydı mühendisi tarafından kaydedilmiş.

Ancak, bu kayıtları yapmadan önce bir komut dosyası gerekir: Ses örneği oluşturmak için ses beceri tarafından konuşulan sözcükleri. En iyi sonuçlar için iyi fonetik kapsamı ve özel ses modeli eğitmek için yeterli çeşitli betiğinizi olması gerekir.

Birçok küçük ancak önemli ayrıntıları profesyonel ses kaydı oluşturma uygulamasına gidin. Bu kılavuz, iyi ve tutarlı sonuçlar elde etmenize yardımcı olacak bir işlem için bir yol haritası içindir.

> [!TIP]
> Yüksek kaliteli sonuçlar için özel sesli geliştirmenize yardımcı olmak amacıyla Microsoft ilgi çekici göz önünde bulundurun. Microsoft, Cortana ve Office gibi kendi ürünler için yüksek kaliteli ses üretme kapsamlı bir deneyimi vardır.

## <a name="voice-recording-roles"></a>Ses kaydını rolleri

Özel ses kaydı projede dört temel roller vardır.

Rol|Amaç
-|-
Ses beceri        |Bu kişinin ses, özel sesli temelini oluşturacak.
Kayıt mühendisi  |Kayıt teknik yönlerini denetleyen ve kayıt donanım çalışır.
Direktörü            |Komut dosyası hazırlar ve ses beceri'nın performans coaches.
Düzenleyici              |Ses dosyalarını sonlandırır ve karşıya yükleme özel sesli portalına hazırlayabilirsiniz.

Bireysel birden fazla rol doldurabilirsiniz. Bu kılavuzda yöneticisi rolü öncelikle doldurma ve işe ses beceri ve bir kayıt mühendisi varsayılmaktadır. Kayıt yapmak istemeniz durumunda kaydı bir mühendislik rolü hakkında bazı bilgiler olduğu kendiniz.

## <a name="choosing-voice-talent"></a>Ses beceri seçme

Voiceover ya da ses karakter çalışma deneyimiyle aktörler iyi özel sesli beceri olun. Ayrıca sık announcers ve haber okuyucular arasında uygun beceri bulabilirsiniz.

Ses yetenek, doğal ses seçin ister. Benzersiz "character" sesleri oluşturmak mümkündür, ancak tutarlı bir şekilde gerçekleştirmek çoğu beceri çok güçtür ve çabayı ses yükü neden olabilir.

> [!TIP]
> Genellikle, özel sesli oluşturmak için tanınan kişilerden daha fazlasını kullanarak önlemek — Elbette ünlü ses üretmek için amacınız olmadığı sürece. Daha az bilinen sesleri kullanıcılara genellikle daha az dikkat dağıtıcı.

Ses beceri seçme tek en önemli faktör tutarlılık ' dir. Aynı odada aynı günde yapılan gibi kayıtlarınızı tüm ses. Bu ideal iyi kayıt yöntemleri ve mühendislik yaklaşımı. 

Eşitlik diğer yarısı, sesli yeteneğiniz olur. İzinli tutarlı oranı, ses düzeyi, aralık ve sesi konuşabilirsiniz olmalıdır. NET diction zorunluluktur. Ayrıca, yetenek kendi sıklık değişimi, duygusal etkiler ve konuşma veren davranışların kesin denetim sahibi olması gerekir.

Özel ses örnekleri kaydı diğer tür üslup çalışmalar daha fazla fatiguing olabilir. Çoğu ses beceri günde iki veya üç saat kaydedebilirsiniz. Üç veya dört haftada bir gün ile oturumlarını devre dışı arasında mümkünse sınırlayın.

Bir ses modeli için yapılan kayıtları bağın bağımsız olmalıdır. Diğer bir deyişle, Üzgün utterance Üzgün şekilde okunur olmamalıdır. Sunu yaparken izleyicilerinizin prosody denetimleri üzerinden Sentezlenen konuşma eklenebilir. "Özel ses genel ses ve duygusal dilini tanımlar kişi" geliştirmek için sesli beceri ile çalışır. İşlemde, "belirsiz" ne benzer bu kişi için saptayın.

Bir kişi, örneğin, bir doğal olarak upbeat kişilik olabilir. Bu nedenle "their" ses optimism Not bile neutrally konuşurken sahip. Ancak, bu tür bir kişilik nitelik zarif ve tutarlı olmalıdır. Okumaları için amaçlayan hakkında bir fikir edinmek için var olan kişilerden daha fazlasını tarafından dinleyin.

> [!TIP]
> Genellikle, yaptığınız ses kayıtları sahip olmasını istersiniz. Ses yetenek, tfs'deki proje için bir iş için işe alma sözleşmesi olması gerekir.

## <a name="creating-a-script"></a>Kod oluşturma

Başlangıç oturumu kaydetme herhangi bir özel sesli, sesli beceri tarafından söylenir konuşma içeren betik noktasıdır. ("Konuşma" terimi, tam cümlelerden hem kısa tümcecikleri kapsar.)

Betiğinizde konuşma yerden gelebilir: kurgu kurgu olmayan, konuşmalarını, haber raporlar ve bulunan başka herhangi bir şey dökümleri yazdırılan form. De (örneğin, TIP terminolojisi veya programlama terminolojisinin) bir kelimelerin belirli tür üzerinde sesinizi desteklediğinden emin olmak istiyorsanız, cümleleri akademik incelemeler veya teknik belgelerden dahil etmek istediğiniz. (Ancak, [Legalities](#legalities) aşağıda.) Ayrıca, kendi metninizi yazabilirsiniz.

Konuşma aynı kaynağa veya kaynak aynı türde birlikte gelmeyen. Birbiriyle yapmak için herhangi bir şey olması bile gerekmez. Ancak, eğer kullanım ayarlamak ifadeleri (örneğin, ", başarılı oturum açma"), konuşma uygulamanızda betiğinizde eklediğinizden emin olun. Bu, özel sesli bu tümcecikleri iyi pronouncing, daha iyi bir fırsat sunar. Ve Sentezlenen konuşma yerine bir kaydı kullanmaya karar verirseniz zaten aynı sesinizi Sentezlenen konuşmanızı olarak sahip.

Tutarlılık ses beceri seçme içinde anahtar olsa da, çeşitli iyi bir betiğin günlerinden ' dir. Betiğinizi birçok farklı sözcükleri ve tümceleri cümle uzunlukları, yapılar ve ruh çeşitli içermelidir. Her ses dilde gösterilen birden çok kez ve çeşitli bağlamlarda olmalıdır (adlı *fonetik kapsamı).* 

Ayrıca, metnin belirli bir ses yazılı olarak temsil edilir ve her ses cümleleri çeşitli yerlerde yerleştirin tüm yolları eklemeniz gerekir. Bildirim temelli cümleler hem sorular bulunan ve uygun tonlama ile okuyun.

Sağlayan bir komut dosyası yazmak zordur *yeterli* iyi sesli oluşturmak özel konuşma tanıma portalı izin vermek için veri. Uygulamada güçlü fonetik kapsamı elde eden bir betik yapmak en kolay yolu çok sayıda örnekleri eklemektir. Microsoft'un standart sesleri binlerce konuşma oluşturulmuştur. Bir üretim kalitesinde özel sesli oluşturmak için birkaç bin konuşma birkaç kaydetmek hazırlıklı olmalıdır.

Betik hataları için dikkatli bir şekilde denetleyin. Mümkünse, çok denetleyin başka birisi vardır. Komut dosyası, beceri ile çalıştırdığınızda, birkaç daha fazla hata büyük olasılıkla catch.

### <a name="script-format"></a>Komut dosyası biçimi

Microsoft Word içinde betiğinizi yazabilirsiniz. Bunu çalışmak kolay bir şekilde ayarlayabilirsiniz bu nedenle betiği kayıt oturumu sırasında kullanılır. Özel ses portal tarafından ayrı ayrı gereken metin dosyası oluşturun.

Temel betik biçimi üç sütunu içerir:

* 1'den başlayarak utterance sayısı. Numaralandırma kolaylaştırır herkes için belirli bir utterance başvurmak için Studio ("sayı 356 yeniden deneyelim"). Tablonun satırlarını otomatik olarak numaralandırmak için özellik numaralandırma Word'ün paragraf kullanabilirsiniz.
* Burada Al sayısında yazma veya tamamlanmış kayıt bulmanıza yardımcı olmak için her utterance kodunu zaman bir boş sütun.)
* Utterance metni.

![Örnek betik](media/custom-voice/script.png)

> [!NOTE]
> Çoğu studios kısa Segment olarak bilinen kayıt *alır.* Her genellikle on-24 konuşma içerir. Yalnızca alma sayısı belirtmeye bir utterance daha sonra bulmak yeterli olacaktır. Uzun kayıtları yapmak için tercih ettiği bir Studio'da kaydediyorsanız, bunun yerine bir zaman kodundan Not isteyebilirsiniz. Studio belirgin süre görüntü olacaktır.

Her satır sonra notları yazmak için yeterli alan bırakın. Hiçbir utterance sayfalar arasında bölünür emin olun. Sayfa numarası ve kağıt bir tarafında kodunuzu yazdırın.

Komut dosyası üç kopyasını yazdırma: bir yetenek, mühendislik için diğeri için Müdürü (,). Kağıt küçük staples yerine kullanın: deneyimli ses sanatçı sayfaları sayfaları açık olarak gürültü yapmaktan kaçınmak için ayrılır.

### <a name="legalities"></a>Legalities

Telif hakkı yasaları altında bir aktörün okuma telif haklı metin iş yazarı dengelenebilecek bir performans olabilir. Bu performans yer alan son ürünü, özel sesli tanınabilir olmayacaktır. Buna rağmen yasallığı, telif haklı bir çözüm bu amaçla kullanma, tanınmış değil. Microsoft, bu sorunla ilgili yasal öneriler sağlayamaz; kendi Konseyi başvurun.

Neyse ki, bu sorunları tamamen önlemek mümkündür. Metin izni veya lisans kullanabileceğiniz birçok kaynak vardır.

|Metin kaynak|Açıklama|
|-|-|
|[CMU kutup gövde](http://festvox.org/cmu_arctic/)|Yaklaşık 1100 cümleler çıkış, telif hakkı works konuşma sentezi projelerinde kullanılmak üzere özel olarak seçilir. Harika bir başlangıç noktası.|
|Artık çalışır<br>Telif hakkı altında|Genellikle works önce 1923 yayımladı. İngilizce, [proje Gutenberg](https://www.gutenberg.org/) on binlerce gibi çalışır sunar. Yeni çalışır, dil için modern İngilizce yakın olarak odaklanmak isteyebilirsiniz.|
|Kamu&nbsp;çalışır|Kamu diğer ülkelerde telif hakkı talep ancak ABD hükümeti tarafından oluşturulan çalışır Amerika Birleşik Devletleri'nde telif hakkı vardır değil.|
|Genel etki alanı|Hangi telif hakkı açıkça elverişlilik veya Works ortak etki alanı için ayrılmış. (Bu tamamen bazı telif hakkı ayları mümkün olmayabilir.)|
|Permissively lisanslı çalışır|Creative Commons veya GNU ücretsiz belgeleri lisans gibi dağıtılmış bir lisansı altında çalışır. Wikipedia GFDL kullanır. Bazı lisans ancak bir özel sesli modeli oluşturulmasını etkileyebilecek lisanslı içerik performansının kısıtlamaları dayatır şekilde okuyun lisans dikkatli bir şekilde.|

## <a name="recording-your-script"></a>Betiğinizi kaydetme

Tercihen, sesli işlerinde betiğinizi uzmanlaşmış bir profesyonel kaydı studio adresindeki kaydetmeniz gerekir. Bunlar, bir kaydı standına, doğru donanım ve işletmek için doğru kişilere sahip olacaksınız. Bu kayıt skimp değil öder.

Studio'nun kaydı mühendisiyle, projeyi tartışmak ve kendi öneriler için dinleyin. Kayıt, çok az kayıpla veya hiç dinamik aralık sıkıştırma (en fazla 4:1) sahip olmalıdır. Ses birimi tutarlı ve yüksek bir sinyal/gürültü oranına istenmeyen sesler ücretsiz olmanın yanı sıra olduğunu önemlidir.

### <a name="doing-it-yourself"></a>Bunu yapmayı

Kayıt yapmak istiyorsanız kendiniz kaydı studio uygulamasına gidip yerine İşte kısa öncü. Giriş kaydı ve pod yayımlama Yükselişi sayesinde iyi kayıt talimatı ve kaynakları çevrimiçi bulmak için her zamankinden daha kolaydır.

Küçük bir oda belirgin Yankı veya "odası ton.", "kayıt standına" olmalıdır Sessiz ve mümkün olduğunca soundproof olmalıdır. Yankı azaltmak ve nötrleştirmek ya da "yer sesi deaden" perdeleri duvarlarının üzerinde kullanılabilir.

Ses kaydetmek için hedeflenen yüksek kaliteli studio Kondansatör mikrofon (kısaca "MIC") kullanın. Sennheiser AKG ve hatta yeni yakınlaştırma mikrofonlardan iyi sonuçlara yol açabilir. Bir MIC satın alın veya yerel işitsel kiralama kesin bir kira. USB arabirimi ile arayın. Bu tür bir MIC rahatça mikrofon öğesi, preamp ve -sayısala dönüştürücü birleştirme basitleştirme bir pakete birleştirir.

Bir analog mikrofon de kullanabilirsiniz. Birçok kiralama görev açısından kritik uygulamaları için kendi ses karakter ünlü "Hasat Yılı" mikrofonlar sunar. Profesyonel analog dişli dengeli XLR bağlayıcıları kullanan yerine 1/4" Tak tüketici ekipmanını kullanılan unutmayın. Aynı zamanda analog giderseniz, bir preamp ve bu bağlayıcıları olan bir bilgisayarda ses arabirimi gerekir.

Bir yedek veya ok mikrofon yükleyin ve önüne "plosive" benzer "p" ve "b" paraziti ortadan kaldırmak için mikrofon pop filtre yükleyin Bazı mikrofonlar bunları vibrations yararlıdır bağımsız olarak öğesinden ayırır bir askıya alma bağlama gelir.

Ses beceri mikrofondan tutarlı bir uzaklıkta kalması gerekir. Bant katında olduğu göze işaretlemek için kullanın. Beceri tercih durumda kalmaya devam ediyorsa, MIC uzaklık izlemek ve sandalye gürültüsünü önlemek için özel dikkat edin.

Betiğini tutmak için kullanın. Böylece doğru mikrofon ses yansıtabilir öne angling kaçının.

İşletim kaydı ekipman kişi — mühendislik — bazı yolunu kaydı standına içinde beceri konuşmak talent, ayrı bir oda olmalıdır (bir *talkback devre).*

Kayıt bir 80 db sinyal/gürültü oranına veya daha iyi bir hedef ile mümkün olduğunca az gürültü olarak içermelidir.

İçinde "standına," Burada tüm gürültü geldiğini ve neden kaldırın şekil sessizlik kaydını yakından dinlemek. Ortak kaynakları gürültü olasılığını hava olaylarını, floresan ışık ballasts, trafiği yollar ve donanım fanlar (hatta dizüstü bilgisayarlar, fanlar olabilir) yakındaki üzerindedir. Mikrofon ve kablolarını, yakındaki AC kablo, genellikle bir sesleri elektrik paraziti yerden devam edebiliyorduk veya haberleri.

> [!TIP]
> Bazı durumlarda, her zaman, kaynakta durdurmak en iyi olsa, parazit, sonuçlarından kaldırılmasına yardımcı olması için bir dengeleyici veya gürültü azaltma yazılım eklentisini kullanmanız mümkün olabilir.

Bu nedenle overdriving olmadan dijital kaydı kullanılabilir dinamik aralığının en çok kullanılan düzeyleri ayarlanması gerekir. Sesli ancak sesli değil kadar ses deforme eder anlamına gelir. İyi bir kayıt dalga örneği aşağıdadır.

![iyi kayıt oluşturulan dalga biçiminin](media/custom-voice/good-recording.png)

Burada, çoğu (yükseklik) aralığının kullanılmaktadır, ancak üst veya alt pencerenin en yüksek en yüksek sayılar sinyal erişmez. Sessizlik kayıtta ince bir yatay çizgi yakın düşük gürültü kat belirten atabilirsiniz. Bu kayıt, kabul edilebilir dinamik aralık ve sinyal/gürültü oranına sahiptir.

Kayıt yüksek kaliteli ses arabirimi ya da bir USB bağlantı noktası'nı kullanarak bilgisayara doğrudan bağlı olarak MIC kullanmakta olduğunuz. Ses zinciri analog için basit tutun: MIC, preamp, ses arabirimi, bilgisayar. Her ikisi de [Avid uzmanı araçlarını](http://www.avid.com/en/pro-tools) ve [Adobe Audition](https://www.adobe.com/products/audition.html) makul ücret ödemeden aylık lisanslanabilir. Bütçenizi çok sıkı olması durumunda, ücretsiz olarak deneyin [Audacity](https://www.audacityteam.org/).

44,1 KHz 16 bit monophonic (CD kalitede) kaydettiğinizden ya da daha iyi. Donanımınızı destekliyorsa, geçerli durumu-ürünü 48 KHz 24-bit, ' dir. Özel ses portala göndermeden önce alt örnekleyin 16 KHz 16-bit ses olur. Yine de düzenlemeleri gerekli olayda bir yüksek kaliteli özgün kaydı öder.

İdeal olarak, Hizmet Yöneticisi, mühendislik ve beceri rollerinde farklı kişilerin sahip. Bunu yapmanın çalışmayın tüm bunları kendiniz! Bir tabletinizde bir kişinin Yöneticisi ve mühendislik olabilir.

### <a name="before-the-session"></a>Oturumdan önce

Studio zaman harcamamak için önce kayıt oturumu aracılığıyla, sesli beceri komut dosyasını çalıştırın. Ses beceri metinle tanıdık olurken, isterse tanınmayan herhangi bir kelimelerin telaffuz açıklık getirebilirsiniz.

> [!NOTE]
> Çoğu kayıt studios kaydı standına betiklerde elektronik görünümünü sunar. Bu durumda, sınama yayınını notlarınıza doğrudan betiğin belgeye yazın. Yine de not oturumu boyunca, ancak almak için basılı kopya istersiniz. Çoğu mühendisleri bir belgesiyse çok isteyeceksiniz. Ve üçüncü kopya bilgisayar kapalı durumda yetenek için yedek olarak yazdırılır. yine de istersiniz!

İçinde bir utterance istediğiniz Word'ün vurgulanmış, sesli beceri isteyebilir. Bu aktörler "operative sözcüğünü." çağırın Hiçbir belirli Vurgu ile doğal bir okuma istediğiniz söyleyin. Konuşma oluşturulan Vurgu eklenebilmesi için; özgün kaydın parçası olmamalıdır.

Sözcükleri sonuçlanmaz telaffuz edilir beceri yönlendirir. Her sözcüğün betik yazıldığı gibi telaffuz. Ses atlanırsa veya birlikte, rastgele okuma özelliği, yaygın olarak bulunur slurred *sürece bu şekilde, betikte yazılmıştır.*

|Yazılan metin|İstenmeyen sıradan söylenişi|
|-|-|
|hiçbir zaman size gidip|hiçbir zaman size gidip|
|dört ışıklar vardır.|dört ışıkları üzeresiniz|
|nasıl hava durumu hemen mi|th'nasıl olduğunu ' Bugün hava durumu|
|küçük arkadaşım Merhaba deyin|Örneğin Belgelerim lil hello' friend|

Beceri gereken *değil* kelimeler arasındaki farklı duraklamaları ekleyin. Cümlenin hala doğal olarak, daha küçük bir resmi uyarabilir sırasında akışı. Bu iyi ayrım doğru hale getirmek için uygulama alabilir.

### <a name="the-recording-session"></a>Kayıt oturumu

Kaydı, bir başvuru veya *eşleşme dosyası* tipik bir utterance oturumunun başlangıcında. Bu satırı yinelenecek beceri her veya sayfası isteyin buçuk. Her zaman başvurmak için yeni kayıt karşılaştırın. Bu yöntem, birim, temposu, aralık ve tonlama tutarlı kalmasını beceri yardımcı olur. Bu arada, mühendislik eşleşme dosyasını bir başvuru olarak düzeyleri ve ses genel tutarlılık için kullanabilirsiniz.

Eşleşen dosyanın sonra bir kesme veya başka bir günlük kaydı devam ettirme, özellikle önemlidir. Beceri için birkaç kez yürütmek ve bunları her zaman iyi eşleşen kadar yinelemek istersiniz.

Derin neden olan aktivitelere ve bir süre önce her utterance duraklatılıyor almak, beceri koltuk. Birkaç saniye arasında konuşma sessizlik kaydedin. Sözcükleri telaffuz aynı şekilde her zaman bunlar görünür, bağlam göz önünde bulundurularak: "kayıt" gibi farklı "kaydından" bir fiil ifade eden isim olarak belirgin olur.

Bir iyi beş "odası ton." yakalamak için saniye önce ilk kaydı sessizlik kaydedin Bu kayıtlar, kalan tüm gürültü dengelemek özel sesli portalı yardımcı olur.

> [!TIP]
> Gerçekten ihtiyacınız olan ses beceri ait satırları, kendi satırların monophonic (kanal tek) kaydı anlamanızı sağlayacak. Ancak stereo olarak kaydederseniz, ikinci kanalı chatter belirli satırlar tartışılması yakalamak için Denetim odasında kaydetmek için kullanabilirsiniz veya alır. Bu izleme, özel sesli Portalı'na yüklenen sürümü kaldırın.

Yakın ses beceri'nın performans yerdeyseniz dinleyin. İyi ancak doğal diction, doğru telaffuz ve istenmeyen sesleri eksikliği bakıyorsunuz. Bu standartlar sağlamayan bir utterance yeniden kaydetmek için beceri isteyin çekinmeyin. 

> [!TIP] 
> Konuşma yüksek hacimli kaydederken tek utterance elde edilen özel sesli bir fark edilebilir etkisi olmayabilir. Bu nedenle, sadece sorunları olan herhangi bir konuşma unutmayın, veri kümesinden dışlamak ve nasıl özel sesinizi ettik görmek daha gerekebilir. Her zaman Studio'ya geri dönün ve daha sonra kaçırılan örnekleri kaydedin.

Alma sayısı bir yere not alın veya betiğinizi kodu için her utterance zaman. Bunlar her utterance sayfasındaki kaydı ait meta verileri veya işaret de işaretlemek, mühendislik isteyin.

Normal sonları alabilir ve kendi ses iyi durumda tutmak, sesli yetenek sağlayacak bir içecek sağlayın.

### <a name="after-the-session"></a>Sonra oturumu

Modern kaydı studios bilgisayarlarda çalıştırın. Oturum sonunda, bir veya daha fazla ses dosyaları, bir bant alırsınız. Bu dosyalar, büyük olasılıkla CD kalite (44,1 KHz 16-bit) ya da WAV AIFF biçiminde olması veya daha yüksek. Sık kullanılan ve istenen 48 kHz 24 bit. 96 KHz gibi daha yüksek bir örnekleme oranı genellikle gerekli değildir.

Özel ses portal her sağlanan utterance kendi dosyasında olmasını gerektiriyor. Birden çok konuşma studio tarafından her teslim ses dosyalarını içerir. Bu nedenle birincil üretim sonrası bölme kayıtlarını ve bunları gönderimi için hazırlamak için bir görevdir. Kayıt mühendisi işaretçileri dosyasına yerleştirilir (veya olabilir sağlanan ayrı işaret listesi) her utterance başladığı belirtmek için.

Notlarınızı tam alan bulmak için istediğiniz sonra yardımcı programı gibi bir ses kullanmak kullanım [Avid uzmanı araçlarını](http://www.avid.com/en/pro-tools), [Adobe Audition](https://www.adobe.com/products/audition.html), ya da ücretsiz [Audacity](https://www.audacityteam.org/) her utterance kopyalamak için Yeni bir dosyaya.

Yalnızca yaklaşık 0.2 saniye sessizlik başlangıcına ve sonuna kadar her küçük ilk dışında bırakın. Bu dosyanın tam beş saniye sessizlik başlamanız gerekir. "Sıfır çıkış" sessiz dosyanın parçalarını ses düzenleyiciye kullanmayın. "Odası sesi" gibi algoritmalar için tüm kalan arka plan gürültüsü dengelemek özel sesli yardımcı olur.

Her dosya için dikkatli bir şekilde dinler. Bu aşamada bir satırından önce bir hafif LIP smack gibi kayıt sırasında kaçırdığınıza küçük istenmeyen ses çıkış düzenleyebilir, ancak herhangi bir gerçek konuşma kaldırmamanız dikkatli olun. Bir dosya onaramazsa veri bunu gerçekleştirmiş Not yapma, kümesinden kaldırın.

Her dosyayı kaydetmeden önce 16 bit ve bir örnek oranını 16 KHz dönüştürün ve studio chatter kaydettiyse, ikinci kanalı kaldırın. WAV biçimde betiğinizi utterance numarası dosyalarını adlandırma, her bir dosyayı kaydedin.

Son olarak, oluşturma *döküm* , her bir WAV dosyası karşılık gelen utterance metin sürümü ile ilişkilendirir. [Özel ses tipi oluşturma](how-to-customize-voice-font.md) gerekli biçime ayrıntılarını içerir. Doğrudan komut dosyasından metin kopyalayabilirsiniz. WAV dosyalarını ve metin dökümü bir ZIP dosyası oluşturun.

Güvenli bir yerde özgün kayıtları daha sonra gerektiği durumlarda arşivleme. Betik ve notlar çok korur.

## <a name="next-steps"></a>Sonraki adımlar

Kayıtlarınızı karşıya yüklemek ve özel sesinizi oluşturmak hazırsınız!

> [!div class="nextstepaction"]
> [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
