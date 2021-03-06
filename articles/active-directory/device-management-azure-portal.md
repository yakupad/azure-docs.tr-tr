---
title: Azure portalını kullanarak cihazları yönetme | Microsoft Docs
description: Cihazları yönetmek için Azure portalını kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 98ab398d2ab1dc3b7cb12a7d13a841f6d713b57c
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308584"
---
# <a name="managing-devices-using-the-azure-portal"></a>Azure portalını kullanarak cihazları yönetme


İle cihaz Yönetimi Azure Active Directory'de (Azure AD) güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlardan kullanıcılarınızın kaynaklarınızı eriştiğiniz emin olabilirsiniz. 

Bu makalede:

- Aşina olduğunuzu varsayar [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)

- Azure portalını kullanarak cihazları yönetme hakkında bilgi sağlar

## <a name="manage-devices"></a>Cihazları yönetme 

Azure portalı, cihazlarınızı yönetmek için merkezi bir konum sağlar. Bu konuma ya da kullanarak alabilirsiniz bir [doğrudan bağlantı](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) veya el ile şu adımları izleyin:

1. Oturum [Azure portalında](https://portal.azure.com) yönetici olarak.

2. Sol gezinti tıklatın **Active Directory**.

    ![Cihaz ayarlarını yapılandır](./media/device-management-azure-portal/01.png)

3. İçinde **Yönet** bölümünde **cihazları**.

    ![Cihaz ayarlarını yapılandır](./media/device-management-azure-portal/11.png)
 
**Cihazları** sayfası, olanak tanır:

- Cihaz yönetim ayarlarınızı yapılandırın

- Cihazlar bulun

- Cihaz yönetim görevlerini gerçekleştirme

- İlgili cihaz yönetimi gözden geçirme denetim günlükleri  
  

## <a name="configure-device-settings"></a>Cihaz ayarlarını yapılandır

Azure portalını kullanarak cihazlarınızı yönetmek için cihazlarınızı aşağıdakilerden biri olması gereken [kaydedilen veya bu hizmete katılan](device-management-introduction.md#getting-devices-under-the-control-of-azure-ad) Azure AD'ye. Yönetici olarak, kaydetme ve aygıt ayarlarını yapılandırarak cihazları birleştirme işlemini hassas ayarlamalar yapabilirsiniz. 

![Cihaz ayarlarını yapılandır](./media/device-management-azure-portal/22.png)

Cihaz ayarları sayfasına yapılandırmanızı sağlar:

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/21.png)


- **Kullanıcılar cihazları Azure AD'ye Katıl** -Bu ayar için kullanıcıları seçmenizi sağlar [katılın cihazları](device-management-introduction.md#azure-ad-joined-devices) Azure AD'ye. Varsayılan değer **tüm**.

- **Ek yerel Yöneticiler Azure AD alanına katılmış cihazlar** -bir cihazda yerel yönetici hakları verilen kullanıcılar seçebilirsiniz. Buraya eklenen kullanıcılar için eklendiğinde *cihaz yöneticileri* Azure AD'de rol. Azure AD'de genel Yöneticiler ve varsayılan olarak cihaz sahiplerine yerel yönetici hakları verilir. Bu seçenek, bir premium edition Özelliği Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilir. 

- **Kullanıcıların cihazlarını Azure AD'ye kaydetme** -olmasını cihazlara izin vermek için bu ayarı yapılandırmanız gereken [kayıtlı](device-management-introduction.md#azure-ad-registered-devices) Azure AD ile. Seçerseniz **hiçbiri**, cihazları Azure AD'ye katılmış olmadığı durumlarda kaydetme veya hibrit Azure AD'ye katılmış için izin verilmez. Office 365 için Microsoft Intune veya mobil cihaz Yönetimi (MDM) ile kayıt kaydı gerektirir. Bu hizmetlerin herhangi birini yapılandırdıysanız **tüm** seçilir ve **NONE** kullanılabilir değil.

- **Cihazları eklemek multi-Factor Auth gerektir** -kullanıcı için ikinci bir kimlik doğrulama faktörü sağlamalarının gerekip gerekmediğini seçin [birleştirme](device-management-introduction.md#azure-ad-joined-devices) cihazlarını Azure AD'ye. Varsayılan değer **Hayır**. Bir cihaz kaydederken çok faktörlü kimlik doğrulaması gerektiren öneririz. Bu hizmet için multi-Factor authentication'ı etkinleştirmeden önce multi-Factor authentication, kullanıcıların aygıtlarını kaydetmesini kullanıcılar için yapılandırıldığından emin olmanız gerekir. Farklı Azure multi-Factor authentication hizmetlerinin hakkında daha fazla bilgi için bkz. [Azure multi-Factor authentication ile çalışmaya başlama](authentication/concept-mfa-whichversion.md). 

- **En fazla cihaz sayısını** -Bu ayar, kullanıcının Azure AD'de sahip olabileceği cihaz sayısının seçmenize olanak sağlar. Bir kullanıcı bu kotaya ulaştığında, bunlar olan yapamaz kadar ek cihazları eklemek veya mevcut cihazların daha fazla kaldırılır. Azure AD'ye katılmış veya Azure AD şu anda kayıtlı olan tüm cihazlar için cihaz teklifi kabul edilir. Varsayılan değer **20**.

- **Kullanıcılar eşitleme ayarları ve uygulama verilerini cihazlarda** -varsayılan olarak, bu ayar **NONE**. Belirli kullanıcılar veya gruplar veya tüm seçilmesi, kullanıcının ayarları ve uygulama verilerini, Windows 10 cihazlarınız arasında eşitlemeye izin verir. Windows 10'da eşitleme birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin.
Bu seçenek, bir premium özelliği, Azure AD Premium veya Enterprise Mobility Suite (EMS) gibi ürünler aracılığıyla kullanılabilir.
 




## <a name="locate-devices"></a>Cihazlar bulun

Kayıtlı ve alanına katılmış cihazları bulmak için iki seçeneğiniz vardır:

- **Tüm cihazlar** içinde **Yönet** bölümünü **cihazları** sayfası  

    ![Tüm cihazlar](./media/device-management-azure-portal/41.png)


- **Cihazları** içinde **Yönet** bölümünü bir **kullanıcı** sayfası
 
    ![Tüm cihazlar](./media/device-management-azure-portal/43.png)



Her iki seçenek ile bir görünüm elde edebilirsiniz:


- Filtre olarak görünen adı kullanan cihazlar için aramanızı sağlar.

- İle kaydedilen ve alanına katılmış cihazların ayrıntılı genel bakış sağlar

- Genel cihaz yönetim görevlerini gerçekleştirmenize olanak sağlar
   

![Tüm cihazlar](./media/device-management-azure-portal/51.png)

Bazı iOS cihazlarınız için kesme içeren cihaz adları, potansiyel olarak kesme gibi görünen farklı karakter kullanabilirsiniz. Bu tür cihazlar aranıyor değil görüyorsanız biraz karmaşık - olacak şekilde arama sonuçları doğru olun arama dizesi eşleşen kesme işareti karakter içeriyor.

## <a name="device-management-tasks"></a>Cihaz yönetim görevleri

Bir yönetici olarak kayıtlı veya alanına katılmış cihazları yönetebilirsiniz. Bu bölümde, genel cihaz yönetim görevleri hakkında bilgi sağlar.


### <a name="manage-an-intune-device"></a>Bir Intune cihaz yönetme

Bir Intune Yöneticisi olarak, istiyorsanız olarak işaretlenmiş cihazlarını yönetebilmeniz için **Intune**. Yönetici ek cihaz görebilirsiniz. 

![Bir Intune cihaz yönetme](./media/device-management-azure-portal/31.png)


### <a name="enable--disable-an-azure-ad-device"></a>Bir Azure AD cihaz devre dışı bırak / etkinleştir

Bir cihazı devre dışı bırakmak / etkinleştirmek için iki seçeneğiniz vardır:

- Görevler menüsünde ("...") **tüm cihazlar** sayfası

    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/71.png)

- Araç çubuğunda **cihazları** sayfası

    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/32.png)


**Notlar:**

- Bir cihazı devre dışı bırakmak / etkinleştirmek için Azure AD'de genel yönetici olmanız gerekir. 
- Bir cihazı devre dışı bırakma, bir cihaz Azure AD'ye kaynaklarınıza erişmesini engeller. 



### <a name="delete-an-azure-ad-device"></a>Bir Azure AD cihaz silme

Bir cihazı silmek için iki seçeneğiniz vardır:

- Görevler menüsünde ("...") **tüm cihazlar** sayfası

    ![Bir Intune cihaz yönetme](./media/device-management-azure-portal/72.png)

- Araç çubuğunda **cihazları** sayfası

    ![Bir cihazı silme](./media/device-management-azure-portal/34.png)


**Notlar:**

- Bir cihazı silmek için Azure AD'de bir genel yönetici veya Intune yönetici olmanız gerekir.

- Bir cihazı silme:
 
    - Bir cihazı Azure AD'ye kaynaklarınıza erişmesini engeller. 

    - Tüm cihaza, örneğin eklenmiş ayrıntıları, Windows cihazları için BitLocker anahtarlarını kaldırır.  

    - Kurtarılamaz bir etkinliği temsil eder ve gerekli olmadığı sürece önerilmez.

Bir cihaz başka bir yönetim yetkilisi (örneğin, Microsoft Intune) tarafından yönetiliyorsa, lütfen cihaz silinebilen / Azure AD'de cihaz silmeden önce devre dışı olduğunu emin olun.

 


### <a name="view-or-copy-device-id"></a>Görüntüleme veya cihaz Kimliğini kopyalama

Bir cihaz kimliği, cihaz kimliği ayrıntılarını cihazında veya sorun giderme sırasında PowerShell kullanarak doğrulamak için kullanabilirsiniz. Kopyalama seçeneği erişmek için cihaz seçeneğine tıklayın.

![Bir cihaz kimliği görüntüleyin](./media/device-management-azure-portal/35.png)
  

### <a name="view-or-copy-bitlocker-keys"></a>Görüntüleme veya BitLocker anahtarlarını kopyalama

Görüntüleyebilir ve bunların şifreli sürücüyü kurtarmasına imkan kullanıcılara yardımcı olmak için BitLocker anahtarları kopyalayın. Bu anahtarlar yalnızca şifrelenmiş Windows cihazlar için kullanılabilir ve anahtarlarını, Azure AD'de depolanan sahip. Cihaz ayrıntılarını erişirken, bu anahtarlar kopyalayabilirsiniz.
 
![BitLocker anahtarlarını görüntüle](./media/device-management-azure-portal/36.png)

BitLocker anahtarları kopyalayın ya da görüntülemek için cihaz sahibi veya aşağıdaki rolleri atanmış en az birine sahip bir kullanıcı olması gerekir:

- Genel yöneticiler
- Yardım Masası yöneticileri
- Güvenlik yöneticileri
- Güvenlik okuyucuları
- Intune hizmet yöneticileri

> [!NOTE]
> Hibrit Azure AD'ye katılmış Windows 10 cihazlarını bir sahibi yoktur. Bir cihaz sahibi tarafından arayan ve bulunamadı, bu nedenle, cihaz kimliğine göre arama


## <a name="audit-logs"></a>Denetim günlükleri


Cihaz etkinliklerini, etkinlik günlükleri kullanılabilir. Bu, cihaz Kayıt Hizmeti'ni ve kullanıcılar tarafından tetiklenen etkinlikleri içerir:

- Cihaz oluşturma ve sahipleri ekleme / cihazdaki kullanıcılar

- Cihaz ayarlarında yapılan değişiklikler

- Bir cihaz güncelleştirme veya silme gibi cihaz işlemleri
 
Denetim verilerine giriş noktanız, **denetim günlükleri** içinde **etkinlik** bölümünü **cihazları** sayfası.

![Denetim günlükleri](./media/device-management-azure-portal/61.png)


Denetim günlüklerinin aşağıdakileri gösteren bir varsayılan liste görünümü vardır:

- Tarih ve saat oluşum

- Hedefleri

- Başlatıcısı / aktörü (kim) bir etkinlik

- Etkinlik (ne)

![Denetim günlükleri](./media/device-management-azure-portal/63.png)

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.
 
![Denetim günlükleri](./media/device-management-azure-portal/64.png)


Raporlanan verileri istediğiniz düzeye gelecek şekilde daraltmak için, aşağıdaki alanları kullanarak denetim verilerini filtreleyebilirsiniz:

- Kategori
- Etkinlik kaynak türü
- Etkinlik
- Tarih aralığı
- Hedef
- Başlatan (Aktör)

Filtreler yanı sıra belirli girdiler için arama yapabilirsiniz.

![Denetim günlükleri](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)



