---
title: Azure Güvenlik Merkezi'nde güvenlik yapılandırmalarını düzeltme | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi önerisi, "Düzelt güvenlik yapılandırmalarını." nasıl uygulanacağını gösterir
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2018
ms.author: terrylan
ms.openlocfilehash: 3c56abcec37bb6abcb77ec8cc443b0656bd69932
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990804"
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik yapılandırmalarını düzeltme
Azure Güvenlik Merkezi, sanal makineleri (VM'ler) ve Vm'leri hale getirebilecek yapılandırması için ve saldırı karşısında daha savunmasız bilgisayarları işletim sistemi (OS) günlük analiz eder. Güvenlik Merkezi, işletim sistemi yapılandırması önerilen güvenlik yapılandırması kurallarını eşleşmiyor ve bu güvenlik açıklarına değinen yapılandırma değişiklikleri önerir, güvenlik açıklarını gidermek önerir.

İzlenmekte olan belirli yapılandırmalar hakkında daha fazla bilgi için bkz. [önerilen yapılandırma kuralları listesini](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Güvenlik Yapılandırma değerlendirmeleri özelleştirme öğrenmek için bkz [özelleştirme işletim sistemi güvenlik yapılandırmaları (Önizleme) Azure Güvenlik Merkezi'nde](security-center-customize-os-security-config.md).

## <a name="implement-the-recommendation"></a>Önerisini uygulama
"Güvenlik yapılandırmalarını düzeltme" Güvenlik Merkezi'nde bir öneri olarak sunulmuştur. Önerinin altında görüntülenen **önerileri** > **işlem ve uygulamalar**.

Bu örnekte, "güvenlik yapılandırmalarını düzeltme" önerinin altında ele alınmaktadır **işlem ve uygulamalar**.
1. Güvenlik Merkezi'nde, sol bölmede seçin **işlem ve uygulamalar**.  
  **İşlem ve uygulamalar** penceresi açılır.

   ![Güvenlik yapılandırmalarını düzeltme][1]

2. Seçin **güvenlik yapılandırmalarını düzeltme**.  
  **Güvenlik yapılandırmalarını** penceresi açılır.

   !["Güvenlik yapılandırmalarını" penceresi][2]

  Üst kısmında Pano görüntüler:

  - **Önem derecesine göre başarısız olan kurallar**: işletim sistemi yapılandırması kullanarak Vm'lerinizdeki ve bilgisayarlarınızdaki, önem derecesine göre ayrıştırılmış arasında başarısız kuralları toplam sayısı.
  - **Başarısız olan kurallar türüne göre**: işletim sistemi yapılandırması kullanarak Vm'lerinizdeki ve bilgisayarlarınızdaki, türüne göre ayrıştırılmış arasında başarısız kuralları toplam sayısı.
  - **Başarısız olan Windows kurallar**: başarısız Windows işletim sistemi yapılandırmalarınızı kurallarının toplam sayısı.
  - **Başarısız olan Linux kurallar**: Linux işletim sistemi yapılandırmalarınızı başarısız kurallarının toplam sayısı.

  Panonun alt bölümünde başarısız olan tüm kurallar, VM'ler ve bilgisayarlar ve eksik güncelleştirmenin önem derecesi için listeler. Listeye aşağıdaki öğeleri içerir:

  - **CCEID**: CCE kuralı için benzersiz tanımlayıcı. Güvenlik Merkezi, benzersiz tanımlayıcıları için yapılandırma kuralları atamak üzere Common Configuration Enumeration (CCE) kullanır.
  - **Ad**: başarısız olan kuralın adı.
  - **Kural türü**: *kayıt defteri anahtarı*, *Güvenlik İlkesi*, *Denetim İlkesi*, veya *IIS* kural türü.
  - **Hayır VM'ler ve bilgisayarların**: sanal makineleri ve başarısız kuralın uygulanacağı bilgisayarların toplam sayısı.
  - **Kural önem derecesi**: CCE değer *kritik*, *önemli*, veya *uyarı*.
  - **Durum**: önerinin geçerli durumu:

    - **Açık**: Öneri henüz ele alınmadı.
    - **Devam eden**: öneri şu anda kaynaklara uygulanıyor ve herhangi bir işlem yapmanıza gerek yoktur.
    - **Çözümlenen**: öneri uygulandı. Sorun çözüldüğünde girdi soluklaşır.

3. Başarısız bir kurala ayrıntılarını görüntülemek için listeden seçin.

   ![Başarısız yapılandırma kuralı ayrıntılı görünümü][3]

   Ayrıntılı görünümde, aşağıdaki bilgileri görüntüler:

   - **Ad**: kuralın adı.
   - **CCIED**: CCE kuralı için benzersiz tanımlayıcı.
   - **İşletim sistemi sürümü**: sanal makine veya bilgisayar işletim sistemi sürümü.
   - **Kural önem derecesi**: CCE değer *kritik*, *önemli*, veya *uyarı*.
   - **Tam açıklama**: Kural açıklaması.
   - **Güvenlik Açığı**: güvenlik açığı veya kuralı uygulanmaz, risk açıklaması.
   - **Olası etkisini**: Kural uygulandığında iş etkisi.
   - **Karşı önlem**: düzeltme adımları.
   - **Beklenen değer**: Güvenlik Merkezi, sanal makine işletim sistemi yapılandırması kural karşı analiz edilirken beklenen değer.
   - **Gerçek değer**: VM'nin işletim sistemi yapılandırma kuralı karşı analizini kayıttan sonra döndürülen değer.
   - **Kural işlemi**: VM'nin işletim sistemi yapılandırma kuralı karşı analiz sırasında Güvenlik Merkezi tarafından kullanılan kural işlemi.

4. Ayrıntılı Görünüm penceresinin en üstünde seçin **arama**.  
  Arama, Vm'leri ve seçili güvenlik yapılandırmalarını uyumsuzluğu bilgisayarlarla olan çalışma alanlarının bir listesini açar. Çalışma alanı seçimi yalnızca seçilen kuralı farklı çalışma alanına bağlı olan birden çok VM için geçerli olup olmadığını gösterilir.

   ![Listelenen çalışma alanları][4]

5. Bir çalışma alanı seçin.  
  Güvenlik yapılandırmaları uyumsuzluğu ile çalışma alanına filtrelenmiş bir Log Analytics arama sorgu açılır.

   ![İşletim sistemi güvenlik açığı ile çalışma][5]

6. Listeden bir bilgisayar seçin.  
  Filtre yalnızca bu bilgisayar için bilgi içeren yeni bir arama sonucu açılır.

   ![Seçilen bilgisayarla ilgili ayrıntılı bilgiler][6]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'nin önerisini "Düzelt güvenlik yapılandırmalarını." uygulama nasıl oluşturulacağını gösterir Güvenlik Yapılandırma değerlendirmeleri özelleştirme öğrenmek için bkz [özelleştirme işletim sistemi güvenlik yapılandırmaları (Önizleme) Azure Güvenlik Merkezi'nde](security-center-customize-os-security-config.md).

İzlenmekte olan belirli yapılandırmalar gözden geçirmek için bkz: [önerilen yapılandırma kuralları listesini](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Güvenlik Merkezi, benzersiz tanımlayıcıları için yapılandırma kuralları atamak üzere Common Configuration Enumeration (CCE) kullanır. Daha fazla bilgi için Git [CCE](https://nvd.nist.gov/cce/index.cfm) site.

Güvenlik Merkezi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* Desteklenen Windows ve Linux Vm'leri listesi için bkz. [desteklenen platformlar Azure Güvenlik Merkezi'nde](security-center-os-coverage.md).
* Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinmek için [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md).
* Öneriler, Azure kaynaklarınızı korumanıza nasıl yardımcı bilgi edinmek için [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md).
* Azure kaynaklarınızın durumunu izlemek öğrenmek için bkz: [güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md).
* Yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinmek için [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md).
* İş ortağı çözümlerinizin sistem durumunu izlemek öğrenmek için bkz: [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md).
* Hizmet kullanımı ile ilgili sık sorulan soruların yanıtları için bkz: [Azure Güvenlik Merkezi SSS](security-center-faq.md).
* Azure güvenliği ve uyumluluğu ile ilgili blog yazıları için bkz. [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/).

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
