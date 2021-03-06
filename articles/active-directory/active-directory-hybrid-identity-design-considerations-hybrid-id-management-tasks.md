---
title: Karma kimlik tasarımı - yönetim görevleri Azure | Microsoft Docs
description: Koşullu erişim denetimi ile Azure Active Directory kullanıcı doğrulanırken ve uygulamaya erişimine izin vermeden önce çekme belirli koşullar denetler. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 5fcb229690827c7bd508aa0d27dd68b169b4e25e
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801335"
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Karma kimlik yaşam döngüsü planlaması
Kimlik, enterprise mobility ve uygulama erişim stratejilerin temellerinden biridir. Mobil cihaz veya SaaS uygulaması için imzalama olup olmadığını kimliğinizi her şeyi erişmesini için anahtardır. En yüksek düzeyde otomatikleştirmek ve kaynakları sağlama işlemidir merkezileştirme içeren birleştirin ve, kimlik depoları arasında eşitleniyor bir kimlik yönetimi çözümü kapsar. Kimlik çözümü, şirket içi ve bulut arasında merkezi bir kimlik olabilir ve aynı zamanda merkezi kimlik doğrulaması ve güvenli bir şekilde paylaşmak koruyup dış kullanıcılar ve işletmelerin işbirliği yapmak için Kimlik Federasyonu çeşit kullanmanız gerekir. Kaynakları işletim sistemlerini ve uygulamaları kişilere çeşitli ya da bir kuruluşla ilişkili. Organizasyon yapısı sağlama ilkeleri ve yordamlarıyla uyum sağlayacak şekilde değiştirilebilir.

Kullanıcılarınızın verimli olmasını sağlamak için Self Servis deneyimleriyle sağlayarak güçlendirmeniz yönelik bir kimlik çözümü olması önemlidir. Kimlik çözümünüzü tüm kaynaklar arasında kullanıcılar için çoklu oturum açmayı etkinleştirir erişmesi gereken daha sağlamdır. Tüm düzeylerdeki yöneticileri, kullanıcı kimlik bilgilerini yönetmek için standartlaştırılmış yordamları kullanabilirsiniz. Bazı yönetim düzeylerini azaltılmış veya ortadan, sağlama yönetim çözümü avantajlarına bağlı olarak. Ayrıca, güvenli bir şekilde yönetim yetenekleri el ile veya otomatik olarak, çeşitli kuruluş arasında dağıtabilirsiniz. Örneğin, bir etki alanı yöneticisi, yalnızca kişileri ve bu etki alanındaki kaynaklara hizmet verebilir. Bu kullanıcı sağlama ve yönetim görevlerini gerçekleştirebilir, ancak iş akışları oluşturma gibi yapılandırma görevleri gerçekleştirmek için yetkili değil.

## <a name="determine-hybrid-identity-management-tasks"></a>Karma kimlik yönetimi görevleri belirleme
Kuruluşunuzdaki yönetim görevlerini dağıtma doğruluğunu ve yönetim verimliliğini artırır ve iş yükü, bir kuruluşun bakiyesini artırır. Aşağıda, güçlü kimlik yönetimi sistemi tanımlamak özetlere verilmiştir.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)

Karma kimlik yönetimi görevleri tanımlamak için karma kimlik benimsediği kuruluşun temel bazı özellikler anlamanız gerekir. Kimlik kaynakları için kullanılan geçerli depoları anlamak önemlidir. Bu çekirdek öğeleri bilerek tarafından temel gereksinimleri vardır ve kimlik çözümünüz için daha iyi bir tasarım kararına götürür daha ayrıntılı sorular sormak gerekir dayanır.  

Bu gereksinimleri tanımlarken, en az emin aşağıdaki soruları yanıtlanır

* Sağlama seçenekleri: 
  
  * Karma kimlik çözümü, güçlü bir hesap erişim yönetimi ve sağlama sistem destekliyor mu?
  * Kullanıcıları, grupları ve yönetilecek giderek parolaları nasıl misiniz?
  * Kimlik yaşam döngüsü yönetimi esnek mi? 
    * Parola güncelleştirmeleri hesabı askıya ne kadar sürer?
* Lisans Yönetimi: 
  
  * Karma kimlik çözümü tanıtıcıları Lisans Yönetimi mu?
    * Yanıt Evet ise, hangi özellikler sağlanıyor?
* Çözüm grup tabanlı lisans yönetimi ele alıyor? 
  
      - Yanıt Evet ise, bir güvenlik grubu atamak mümkün mü? 
       - Yanıtınız evet ise, bulut dizini lisansları grubun tüm üyelerine otomatik olarak ata? 
        - Bir kullanıcı daha sonra eklenen veya gruptan kaldırılmış, bir lisans otomatik olarak atanmış veya kaldırılacak uygun şekilde kaldırılan neler? 
* Diğer üçüncü taraf kimlik sağlayıcıları ile tümleştirme:
* Bu karma çözüm çoklu oturum açmayı uygulamak için üçüncü taraf kimlik sağlayıcıları ile tümleştirilebilir?
* Tüm farklı kimlik sağlayıcıları bağlı kimlik sisteme bütünleştirin mümkün mü?
* Yanıt Evet ise, nasıl ve bunlar ve hangi özellikler sağlanıyor?

## <a name="synchronization-management"></a>Eşitleme Yönetimi
Tüm kimlik sağlayıcıları getirebilir ve kalmalarını olmasını hedeflerinden biri Identity manager eşitlendi. Eşitlenen veriler tutmak bir yetkili ana kimlik sağlayıcısını temel. Eşitlenmiş yönetim modeliyle bir karma kimlik senaryosunda bir şirket içi sunucu tüm kullanıcı ve cihaz kimliklerini yönetmek ve hesapları ve, isteğe bağlı olarak, parolaları buluta eşitleyin. Kullanıcı aynı parola şirket içi bulutta yapın ve oturum açma sırasında parola kimlik çözümü tarafından doğrulanır olarak girer. Bu model, directory eşitleme aracını kullanır.

![](./media/hybrid-id-design-considerations/Directory_synchronization.png) Uygun tasarıma karma kimlik çözümü eşitlenmesini sağlamak aşağıdaki soruları yanıtlanır: • için karma kimlik çözümü kullanılabilir eşitleme çözümleri nelerdir?
• Çoklu oturum açma kullanılabilen özellikleri nelerdir?
• B2B B2C arasındaki Kimlik Federasyonu için seçenekleri nelerdir?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimini benimseme stratejinizi belirleme](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

