---
title: Market’te Azure yönetilen uygulamalar | Microsoft Docs
description: Market’te kullanılabilir olan Azure yönetilen uygulamalarını tanımlar.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 07/10/2018
ms.author: tomfitz
ms.openlocfilehash: 8f35bda8c6925bdc10097ac6d180f5998bd5cf1d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989796"
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Market’teki Azure yönetilen uygulamaları

Satıcılar, çözümlerini tüm Azure Market müşterilerine sunmak üzere Azure yönetilen uygulamalarını kullanabilir. Bu satıcılar yönetilen hizmet sağlayıcıları (MSP’ler), bağımsız yazılım satıcıları (ISV’ler) ve sistem tümleştiricilerini (SI’lar) dahil edebilir. Yönetilen uygulamalar müşteriler için bakım ve servis masraflarını azaltır. Satıcılar market aracılığıyla altyapı ve yazılım satar. Yönetilen uygulamalara hizmetler ve işletimsel destek ekleyebilir. Daha fazla bilgi için bkz. [Yönetilen uygulamaya genel bakış](overview.md).

Bu makale bir uygulamayı markette yayımlamayı ve müşterilerin kullanımına açmayı açıklamaktadır.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Yönetilen uygulama yayınlamaya yönelik önkoşullar

Bu makaleyi tamamlamak için yönetilen uygulama tanımınıza yönelik .zip dosyasına sahip olmanız gerekmektedir. Daha fazla bilgi için bkz. [Hizmet kataloğu uygulaması oluşturma](publish-service-catalog-app.md).

Ek olarak birkaç iş önkoşulu bulunmaktadır. Bunlar:

* Şirketinizin veya yan kuruluşunun satışların market tarafından desteklendiği bir ülkede bulunması gerekir.
* Ürününüz, marketin desteklediği faturalandırma modelleriyle uyumlu olacak şekilde lisanslandırılmalıdır.
* Teknik desteği müşterilerin erişimine ticari açıdan sorumlu bir şekilde sunun. Destek ücretsiz, ücretli veya topluluk desteği aracılığıyla olabilir.
* Yazılımınızı ve tüm üçüncü taraf yazılım bağımlılıklarını lisanslandırın.
* Market’te ve Azure portalında listelenecek olan teklifinize ilişkin ölçütlere uyan içerikler sağlayın.
* Azure Market Katılım İlkeleri ve Yayımcı Sözleşmesi’nin koşullarını kabul edin.
* Kullanım Koşulları, Microsoft Gizlilik Bildirimi ve Microsoft Azure Sertifikalı Program Sözleşmesi’ne uymayı kabul edin.

## <a name="become-a-publisher"></a>Yayımcı olma

Azure Market’te yayımcı olmak için şunları yapmanız gerekir:

1. Microsoft Kimliği oluşturma - Şirketinizin etki alanına ait olan ancak tek bir bireye ait olmayan bir e-posta adresini kullanarak Microsoft hesabınızı oluşturun. Bu e-posta adresi hem Microsoft Geliştirici Merkezi hem de Bulut İş Ortağı Portalı için kullanılır. Daha fazla bilgi için bkz. [Azure Market Yayımcı Kılavuzu](https://aka.ms/sellerguide).
1. [Azure Market Adaylık Formunu](https://aka.ms/ampnomination) gönderme - For **Yayımlamayı düşündüğünüz çözüm** için **Yönetilen Uygulama**’yı seçin. Form gönderildikten sonra Markete Ekleme ekibi uygulamayı gözden geçirir ve isteği doğrular. Onay süreci bir ile üç gün sürer. Adaylığınız onaylandığında geliştirici merkezine yönelik kayıt ücretinin silinmesini sağlayan bir promosyon kodu alırsınız. Market Adaylık Formunu **doldurmazsanız** 99 $ kayıt ücreti ödemeniz istenir.
1. [Geliştirici Merkezine](http://dev.windows.com/registration?accountprogram=azure) Kaydolma - Microsoft, kuruluşunuzun kayıtlı olduğu ülkeye yönelik geçerli bir VERGİ KİMLİĞİ olan geçerli bir tüzel kişi olduğunuzu doğrular. Onay işlemi 5 ile 10 gün sürebilir. Kayıt ücretini ödememek için adaylık sürecinde size gelen e-posta ile aldığınız promosyon kodunu kullanın. Daha fazla bilgi için bkz. [Azure Market Yayımcı Kılavuzu](https://aka.ms/sellerguide).
1. [Bulut İş Ortağı Portalında](https://cloudpartner.azure.com) oturum açma - Yayımcı profilinde, Geliştirici Merkezi hesabınızı Market Yayımcı Profiliyle ilişkilendirin. Daha fazla bilgi için bkz. [Azure Market Yayımcı Kılavuzu](https://aka.ms/sellerguide).

## <a name="create-a-new-azure-application-offer"></a>Yeni bir Azure uygulaması teklifi oluşturma

İş ortağı portalı hesabınızı oluşturduktan sonra yönetilen uygulama teklifinizi oluşturmaya hazır olursunuz.

### <a name="set-up-an-offer"></a>Teklif ayarlama

Yönetilen uygulamaya yönelik teklif, bir yayımcının ürün teklifi sınıfına karşılık gelir. Markette kullanıma sunmak istediğiniz yeni bir uygulama türünüz varsa yeni bir teklif olarak ayarlayabilirsiniz. Teklif bir SKU koleksiyonudur. Tüm teklifler, marketteki kendi varlıkları olarak görünür.

1. [Bulut İş Ortağı portalında](https://cloudpartner.azure.com/) oturum açın.

1. Soldaki gezinti bölmesinde **+ Yeni teklif** > **Azure Uygulamaları**’nı seçin.

1. **Düzenleyici** görünümünde gereken formları görürsünüz. Her bir form, bu makalenin ilerleyen kısımlarında tanımlanmaktadır.

## <a name="offer-settings-form"></a>Teklif Ayarları formu

**Teklif Ayarları** formuna ilişkin alanlar şunlardır:

* **Teklif Kimliği**: Bu benzersiz tanıtıcı, bir yayımcı profilindeki teklifi tanımlar. Bu kimlik; ürün URL’leri, Kaynak Yöneticisi şablonları ve faturalandırma raporlarında görünürdür. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimlik tire ile bitemez. En fazla 50 karakter sınırı vardır. Teklif yayımlandıktan sonra bu alan kilitlenir.
* **Yayımcı Kimliği**: Bu teklifi yayımlamak istediğiniz yayımcı profilini seçmek için bu açılan listeyi kullanın. Teklif yayımlandıktan sonra bu alan kilitlenir.
* **Ad**: Teklifinize ilişkin bu görünen ad, Market’te ve portalda görünür. En fazla 50 karakter olabilir. Ürününüz için tanınabilir bir marka adı ekleyin. Pazarlanma şekli bu olmadığı sürece şirket adınızı buraya eklemeyin. Bu teklifi kendi web sitenizde pazarlıyorsanız adın, web sitenizde göründüğü şekliyle aynı olduğundan emin olun.

İşiniz bittiğinde ilerlemenizi kaydetmek için **Kaydet**’i seçin.

## <a name="skus-form"></a>SKU formu

Bir sonraki adım, teklifiniz için SKU eklemektir.

SKU, bir teklife ilişkin en küçük satın alınabilir birimdir. Şunlar arasında ayrım yapabilmek için aynı ürün sınıfı (teklif) içerisinde bir SKU kullanabilirsiniz:

* Desteklenen farklı özellikler
* Yönetilen ve yönetilmeyen teklifler
* Desteklenen faturalandırma modelleri

SKU, marketteki ana teklifin altında görünür. Azure portalında kendi başına satın alınabilir varlık olarak görünür.

1. **SKU’lar** > **Yeni SKU**’yu seçin.

1. Bir **SKU Kimliği** girin. SKU Kimliği, teklif içindeki SKU’ya yönelik bir benzersiz tanıtıcıdır. Bu kimlik; ürün URL’leri, Kaynak Yöneticisi şablonları ve faturalandırma raporlarında görünürdür. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimlik tire ile bitemez ve en fazla 50 karakterle sınırlıdır. Teklif yayımlandıktan sonra bu alan kilitlenir. Bir teklif içinde birden çok SKU’ya sahip olabilirsiniz. Yayımlamayı planladığınız her bir resim için bir SKU seçmeniz gerekir.

1. Aşağıdaki formda **SKU Ayrıntıları** bölümünü doldurun:

   Aşağıdaki alanları doldurun:

   * **Başlık**: Bu SKU için bir başlık girin. Bu başlık, bu öğe için olan galeride görünür.
   * **Özet**: Bu SKU için bir kısa özet girin. Bu metin başlığın altında görünür.
   * **Açıklama** : SKU hakkında ayrıntılı bir açıklama girin.
   * **SKU Türü**: İzin verilen değerler *Yönetilen Uygulama* ve *Çözüm Şablonları*’dır. Bu durum için *Yönetilen Uygulama*’yı seçin.
   * **Ülke/Bölge kullanılabilirliği**: Yönetilen uygulamanın kullanılabilir olduğu ülkeleri seçin.
   * **Fiyatlandırma**: Uygulamanın yönetimi için bir fiyat sağlayın. Fiyatı ayarlamadan önce kullanılabilir ülkeleri seçin.

1. Yeni bir paket ekleyin. Aşağıdaki formda yer alan **Paket Ayrıntıları** bölümünü doldurun:

   Aşağıdaki alanları doldurun:

   * **Sürüm**: Yüklediğiniz paket için bir sürüm girin. `{number}.{number}.{number}{number}` biçiminde olmalıdır.
   * **Paket dosyası (.zip)**: Bu paket .zip paketi olarak sıkıştırılmış iki gerekli dosyayı içerir. Dosyalardan biri, yönetilen uygulamaya ilişkin dağıtılacak kaynakları tanımlayan Kaynak Yöneticisi şablonudur. İkinci dosya ise portal aracılığıyla yönetilen uygulamayı dağıtan tüketiciler için [kullanıcı arayüzünü](create-uidefinition-overview.md) tanımlamaktadır. Kullanıcı arayüzünde tüketicilerin parametre değerleri sağlamasına olanak tanıyan öğeleri belirlersiniz.
   * **PrincipalId**: Bu özellik, müşterinin aboneliğindeki kaynaklara erişimin sağlandığı kullanıcı, kullanıcı grubu veya uygulamalanın Azure Active Directory (Azure AD) tanıtıcısıdır. Rol Tanımı izinleri açıklar.
   * **Rol Tanımı**: Bu özellik, Azure AD tarafından sağlanan tüm yerleşik Rol Tabanlı Erişim Denetimi (RBAC) rollerinin bir listesidir. Müşteri adına kaynakları yönetmek için kullanılması en uygun olan rolü seçebilirsiniz.
   * **İlke Ayarları**: Dağıtılan çözümlerde uyumluluk gereksinimlerini belirtmek için, yönetilen uygulamanıza bir [Azure İlkesi](../azure-policy/azure-policy-introduction.md) uygulayın. Kullanılabilir seçenekler arasından uygulanacak ilkeleri seçin. **İlke Parametreleri** için, parametre değerleriyle bir JSON dizesi sağlayın. İlke tanımları ve parametre değerlerinin biçimi için bkz. [Azure İlke Örnekleri](../azure-policy/json-samples.md).

Çeşitli yetkilendirmeler ekleyebilirsiniz. Bir AD kullanıcı grubu oluşturarak kimliğini **PrincipalId** olarak belirlemenizi öneririz. Bu şekilde SKU’yu güncelleştirme gereği olmadan kullanıcı grubuna daha fazla kullanıcı ekleyebilirsiniz.

RBAC hakkında daha fazla bilgi için bkz. [Azure portalında RBAC ile çalışmaya başlama](../role-based-access-control/overview.md).

## <a name="marketplace-form"></a>Market formu

Market formu [Azure Market](https://azuremarketplace.microsoft.com) ve [Azure portal](https://portal.azure.com/) içinde görünen alanları sorar.

### <a name="preview-subscription-ids"></a>Abonelik kimliklerini önizleme

Yayımlandıktan sonra teklife erişebilen Azure aboneliği kimliklerinin listesini girin. Yayımlamadan önce bu beyaz listelenen abonelikleri kullanarak önizlemesi yapılan teklifi test edebilirsiniz. İş ortağı portalında 100’e kadar aboneliği içeren bir beyaz liste derleyebilirsiniz.

### <a name="suggested-categories"></a>Önerilen kategoriler

Listeden, teklifinizin en iyi ilişkilendirilebileceği beşe kadar kategori seçin. Bu kategoriler, teklifinizi [Azure Market](https://azuremarketplace.microsoft.com) ve [Azure portal](https://portal.azure.com/) içinde kullanılabilir olan ürün kategorileriyle eşlemek için kullanılır.

#### <a name="azure-marketplace"></a>Azure Market

Yönetilen uygulamanızın özeti aşağıdaki alanları gösterir:

![Market özeti](./media/publish-marketplace-app/publishvm10.png)

Yönetilen uygulamanıza yönelik **Genel Bakış** sekmesi aşağıdaki alanları gösterir:

![Market’e genel bakış](./media/publish-marketplace-app/publishvm11.png)

Yönetilen uygulamanıza yönelik **Planlar + Fiyatlandırma** sekmesi aşağıdaki alanları gösterir:

![Market planları](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure portalına

Yönetilen uygulamanızın özeti aşağıdaki alanları gösterir:

![Portal özeti](./media/publish-marketplace-app/publishvm12.png)

Yönetilen uygulamanıza yönelik genel bakış aşağıdaki alanları gösterir:

![Portala genel bakış](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut İş Ortağı portalına yüklediğiniz tüm logolar için bu yönergeleri izleyin:

*   Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renklerinin sayısını sınırlandırın.
*   Portalın tema renkleri siyah ve beyazdır. Bu renkleri logonuzun arka plan rengi olarak kullanmayın. Logonuzun portalda öne çıkmasını sağlayan bir renk kullanın. Basit birincil renkleri öneririz. *Saydam bir arka plan kullanıyorsanız logo ve metnin beyaz, siyah ve mavi olmadığından emin olun.*
*   Logoda gradyan arka plan kullanmayın.
*   Şirket veya marka adınız dahil olmak üzere logoya metin yerleştirmeyin. Logonuzun genel görünümü düz olmalı ve gradyanlardan kaçınmalıdır.
*   Logonuzun esnetilmediğinden emin olun.

#### <a name="hero-logo"></a>Kahraman logosu

Kahraman logosu isteğe bağlıdır. Yayımcı bir kahraman logosu eklememeyi seçebilir. Kahraman logosu yüklendikten sonra silinemez. Bu esnada iş ortağının kahraman simgelerine yönelik Market yönergelerini izlemesi gerekir.

Kahraman logosu simgesi için şu yönergeleri izleyin:

*   Yayımcı görünen adı, plan başlığı ve teklif uzun özeti beyaz olarak gösterilir. Dolayısıyla kahraman simgesinin arka planı için açık bir renk kullanmayın. Kahraman simgeleri için siyah, beyaz ve şeffaf arka plana izin verilmez.
*   Teklif listelendikten sonra öğeler, kahraman logosunun içine program tarafından eklenir. Eklenmiş öğeler arasında yayımcı görünen adı, plan başlığı, teklif uzun özeti ve **Oluştur** düğmesi yer alır. Sonuç olarak kahraman logosunu tasarlarken hiçbir metin girmeyin. Sağda boş bir alan bırakın çünkü metin program tarafından bu alana eklenir. Metne yönelik boş alan sağ tarafta 415 x 100 piksel olmalıdır. Soldan 370 piksel kaydırılır.

    ![Kahraman logosu örneği](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Destek formu

**Destek** formunu şirketinizin destek iletişim bilgileriyle doldurun. Bu bilgiler mühendislik veya müşteri destek iletişim bilgilerini içerebilir.

## <a name="publish-an-offer"></a>Teklif yayımlama

Tüm bölümleri doldurduktan sonra teklifinizi müşterilerin kullanımına sunan işlemi başlatmak için **Yayımla**’yı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Bir Hizmet Kataloğu yönetilen uygulaması yayımlama hakkında bilgi için bkz. [Hizmet kataloğu uygulaması oluşturma ve yayımlama](publish-service-catalog-app.md).