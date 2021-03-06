---
title: Ad için Grup İlkesi ayarları Office 365 grupları Azure Active Directory'de (Önizleme) | Microsoft Docs
description: Azure Active Directory (Önizleme) içinde Office 365 grupları için sona erme kurma
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 05/21/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 111be7d3ee00f2b40ace3bfe4efdacc5029ccf77
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239143"
---
# <a name="enforce-a-naming-policy-for-office-365-groups-in-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) içinde Office 365 grupları için bir adlandırma ilkesini zorlama

Oluşturduğunuz veya düzenlediğiniz kullanıcılarınız tarafından Office 365 grupları için tutarlı adlandırma kuralları zorlamak için ilke Azure Active Directory (Azure AD) kiracılarınız için bir adlandırma ayarlayın. Örneğin, bir grup üyeliği, coğrafi bölgeyi işlevi iletişim kurmak için adlandırma ilkesi kullanabilir veya grup oluşturan. Kullanıcının adres defterinde grupları kategorilere ayırmaya yardımcı olacak adlandırma İlkesi'ni de kullanabilirsiniz. Belirli sözcükleri grubu adları ve diğer adları kullanılmasını engellemek için ilke kullanabilirsiniz.

> [!IMPORTANT]
> Office 365 grupları adlandırma ilkesi preview kullanarak bir veya daha fazla Office 365 gruplarının üyesi olan her benzersiz kullanıcının Azure AD temel EDU lisansları veya Azure Active Directory Premium P1 lisansı gerektirir.

Adlandırma ilkesi oluşturma ve düzenleme (örneğin, Outlook, Microsoft Teams, SharePoint, Exchange veya Planner) iş yüklerinde oluşturulan grupların uygulanır. Grup adı ve grup diğer adı için uygulanır. Azure AD'de, adlandırma ilkesi ayarlayın ve varolan bir Exchange Grup adlandırma ilkesi varsa, Azure AD adlandırma ilkesi uygulanır.

## <a name="naming-policy-features"></a>Adlandırma ilkesi özellikleri
Office 365 grupları için adlandırma ilkesi iki farklı şekillerde uygulayabilir:

-   **Önek sonek adlandırma ilkesi** ön eklerin veya soneklerin sonra otomatik olarak gruplarınızı üzerinde bir adlandırma kuralı uygulamak için eklenen tanımlayabilirsiniz (örneğin, grup adı olarak "GRP\_JAPONYA\_grubunuza\_ Mühendisliğe"GRP\_JAPONYA\_ öneki ve \_mühendislik soneki olan). 

-   **Özel engellenen sözcük** kullanıcılar (örneğin "CEO, bordro, ik") tarafından oluşturulan grupları engellenmesi, kuruluşunuz için engellenen sözcük belirli bir dizi karşıya yükleyebilirsiniz.

### <a name="prefix-suffix-naming-policy"></a>Önek sonek adlandırma ilkesi

Adlandırma kuralı genel yapısını 'Öneki [GroupName] soneki' dir. Birden çok önek ve sonek tanımlayabilirsiniz, ancak yalnızca ayar [GroupName] bir örneği olabilir. Ön eklerin veya soneklerin sabit dizeler ya da kullanıcı öznitelikleri gibi olabilir \[departmanı\] , yerine grubu oluşturan kullanıcıya göre. Toplam izin verilen en fazla karakter birleştirilmiş, önek ve sonek dizeleri 53 karakterdir. 

Önek ve sonek grubu adı ve grup diğer adının desteklenen özel karakterler içerebilir. Grup diğer desteklenmeyen herhangi bir karakter önek veya sonek olarak hala uygulanan Grup adı ', ancak Grup diğer addan kaldırıldı. Bu kısıtlama nedeniyle, önek ve sonek grubun adını uygulanan Grup diğer adının için uygulanan farklı olabilir. 

#### <a name="fixed-strings"></a>Sabit Dizeler

Dizeleri tarama ve genel adres listesinde ve sol gezinti bağlantıları grubu iş yükü gruplarını birbirinden daha kolay hale getirmek için kullanabilirsiniz. Bazı ortak ön ekleri gibi anahtar sözcükler ' Grp\_adı ', '\#adı ','\_adı '

#### <a name="user-attributes"></a>Kullanıcı öznitelikleri

Yardımcı olabilecek öznitelikleri kullanabilirsiniz ve kullanıcılarınızın departmanı, office veya grubunun oluşturulduğu coğrafi bölgeyi tanımlar. Örneğin, adlandırma ilkenizi olarak tanımlarsanız `PrefixSuffixNamingRequirement = “GRP [GroupName] [Department]”`, ve `User’s department = Engineering`, bir uygulanan Grup adı "GRP grubum Engineering" gelebilir Desteklenen Azure AD öznitelikleri \[departmanı\], \[şirket\], \[Office\], \[Eyaletveİl\], \[CountryOrRegion \], \[Başlık\]. Desteklenmeyen kullanıcı öznitelikleri, sabit dize olarak kabul edilir; Örneğin, "\[postalCode\]". Uzantı öznitelikleri ve özel öznitelikler desteklenmez.

Kuruluşunuzdaki tüm kullanıcılar için doldurulmuş değerler ve uzun değerli öznitelikleri kullanmayın öznitelikleri kullanmanızı öneririz.

### <a name="custom-blocked-words"></a>Özel engellenen sözcük

Tümce grubu adları ve diğer adları engellediği için virgülle ayrılmış bir liste engellenen sözcük listesidir. Hiçbir alt dize arama gerçekleştirilir. Grup adı ve bir veya daha fazla özel engellenen bir kelimelerin arasında bir tam eşleşme başarısız tetiklemek için gereklidir. Engellenen bir sözcük 'sınıf' olsa bile kullanıcılar 'Class' gibi ortak kelimeler kullanabilirsiniz, böylece alt dize araması yapılamaz.

Engellenen sözcük listesi kuralları:
- Engellenen bir sözcük büyük/küçük harfe duyarlı değildir.
- Bir kullanıcı grubu adı bir parçası olarak engellenen bir sözcük girdiğinde, engellenen sözcük içeren bir hata iletisi görürler.
- Engellenen bir sözcük hiçbir karakter kısıtlamaları vardır.
- Engellenen bir sözcük listesinde yapılandırılabilir 5000 tümcecikleri üst sınır yoktur. 

### <a name="administrator-override"></a>Yönetici geçersiz kılma

Seçili yöneticileri bu ilkeleri, tüm Grup iş yükleri ve uç noktaları, böylece bunlar engellenen sözcük kullanarak gruplar oluşturabilir ve kendi adlandırma kuralları ile muaf tutulabilir. Muaf tutulan gruptan adlandırma ilkesinin yönetici rolleri listesi verilmiştir.

- Genel yönetici
- İş ortağı Katman 1 destek
- İş ortağı Katman 2 Destek
- Kullanıcı hesabı yöneticisi
- Dizin yazıcılar

## <a name="install-powershell-cmdlets-to-configure-a-naming-policy"></a>Bir adlandırma ilkesini yapılandırmak için PowerShell cmdlet'leri yükleme

Graf için Windows PowerShell modülü için Azure Active Directory PowerShell herhangi bir eski sürümünü kaldırın ve yüklemek mutlaka [Grafı - genel Önizleme sürümü 2.0.0.137 için Azure Active Directory PowerShell](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) çalıştırmadan önce PowerShell komutları. 

1. Windows PowerShell uygulamasını yönetici olarak açın.
2. AzureADPreview önceki sürümlerini kaldırın.
  
  ````
  Uninstall-Module AzureADPreview
  ````
3. AzureADPreview en son sürümünü yükleyin.
  
  ````
  Install-Module AzureADPreview
  ````
Güvenilmeyen bir depoya erişme hakkında istenirse türü **Y**. Bu yeni modülünü yüklemek birkaç dakika sürebilir.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-ad-powershell"></a>Azure AD PowerShell kullanarak bir kiracı için adlandırma ilkesinin grubu yapılandırma

1. Bilgisayarınızda bir Windows PowerShell penceresi açın. Yükseltilmiş ayrıcalıklar olmadan açabilirsiniz.

2. Bu cmdlet'leri çalıştırmak hazırlamak için aşağıdaki komutları çalıştırın.
  
  ````
  Import-Module AzureADPreview
  Connect-AzureAD
  ````
  İçinde **hesabınızda oturum** yönetici hesabı ve parola hizmetinize bağlanmak ve seçmek için açılır, ekran girin **oturum**.

3. Bağlantısındaki [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md) bu Kiracı için Grup ayarları oluşturmak için.

### <a name="view-the-current-settings"></a>Geçerli ayarları görüntüleyebilir

1. Geçerli ayarları görüntülemek için geçerli adlandırma ilkesi getirin.
  
  ````
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ````
  
2. Geçerli Grup ayarlarını görüntüler.
  
  ````
  $Setting.Values
  ````
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Özel engellenen sözcükler ve adlandırma ilkesi ayarlama

1. Grup adı önek ve sonek Azure AD PowerShell'de ayarlayın.
  
  ````
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ````
  
2. Sınırlamak istediğiniz özel engellenen sözcük ayarlayın. Aşağıdaki örnekte, kendi özel sözcükler nasıl ekleyebileceğiniz gösterilmektedir.
  
  ````
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ````
  
3. Aşağıdaki örnekte, gibi kullanılabilmesi için yeni ilke için ayarları kaydedin.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ````
  
Bu kadar. Adlandırma ilkenizi ve engellenen kelimeleriniz eklendi.

## <a name="export-or-import-the-list-of-custom-blocked-words"></a>Özel engellenen sözcüklerin listesi içeri veya dışarı aktarma

Daha fazla bilgi için bkz [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md).

Birden çok engellenen sözcük dışarı aktarmak için bir PowerShell Betiği örneği aşağıda verilmiştir:

````
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
````

Birden çok engellenen sözcük içeri aktarmak için PowerShell Betiği bir örnek aşağıda verilmiştir:

````
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
$Settings["EnableMSStandardBlockedWords"] = $True
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
````

## <a name="naming-policy-experiences-across-office-365-apps"></a>Office 365 uygulamalarında adlandırma ilkesi deneyimleri

Bir kullanıcı grubu bir Office 365 uygulama oluşturduğunda, Azure AD'de bir grup adlandırma ilkesi ayarladıktan sonra görürler: 

* Grup adı, kullanıcı türleri olan en kısa sürede Önizleme adlandırma ilkenizle (önek ve sonek) göre adı A
* Kullanıcı, engellenen bir sözcük girerse, bunlar engellenen sözcük kaldırabilmeniz için bir hata iletisi görürler.

İş yükü | Uyumluluk
----------- | -------------------------------
Azure Active Directory portalları | Kullanıcı, grup adını oluştururken ya da bir grubu düzenleme yazdığında Azure AD erişim paneli portalı ve adlandırma zorlanan ilke adını gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, böylece kullanıcı kaldırabilirsiniz engellenen sözcük içeren bir hata iletisi görüntülenir.
Outlook Web Access (OWA) | Kullanıcı grubu adı ya da grup diğer adının yazdığında, outlook Web Access adlandırma ilkesi gösterir adı zorunlu. Bir kullanıcı özel engellenen bir sözcük girdiğinde, kullanıcı kaldırabilirsiniz, böylece bir hata iletisi engellenen sözcük yanı sıra kullanıcı Arabirimi gösterilir.
Outlook Masaüstü | Outlook Masaüstü oluşturulan grupların adlandırma ilkesi ayarları ile uyumlu olması gerekir. Outlook masaüstü uygulaması henüz Önizleme uygulanan Grup adının göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hata döndürmez. Ancak, adlandırma ilkesi oluştururken veya düzenlerken bir grubu otomatik olarak uygulanır ve grup adını ya da diğer özel engellenen sözcük yoksa, kullanıcılar hata iletilerini görmek.
Microsoft Teams | Microsoft Teams zorlanan ilke adı, kullanıcı bir takım adı girdiğinde adlandırma grubu gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, böylece kullanıcı kaldırabilirsiniz engellenen sözcük yanı sıra bir hata iletisi gösterilir.
SharePoint  |  SharePoint site bir kullanıcı türleri ad veya e-posta adresi Grup adlandırma zorlanan ilke adını gösterir. Kullanıcıyı kaldırmak için bir kullanıcı özel engellenen bir sözcük olan bir hata iletisi girdiğinde, engellenen bir sözcük ile birlikte gösterilir.
Microsoft Stream | Microsoft Stream, kullanıcı grubu adı ya da Grup e-posta diğer adı yazdığında zorlanan ilke adı adlandırma gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, kullanıcıyı kaldırmak için engellenen bir sözcük ile bir hata iletisi gösterilir.
Outlook iOS ve Android uygulaması | Outlook uygulamalarında oluşturulan grupları yapılandırılmış adlandırma ilkesi ile uyumlu olması gerekir. Outlook mobil uygulamasının henüz adlandırma zorlanan ilke adı önizlemesiyle göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hata döndürmez. Ancak, adlandırma ilkesi Oluştur/Düzenle'ye tıkladığınızda otomatik olarak uygulanır ve grup adını ya da diğer özel engellenen sözcük yoksa, kullanıcılar hata iletilerini görmek.
Mobil uygulama grupları | Grupları mobil uygulamasında oluşturulan grupların adlandırma ilkesi ile uyumlu olması gerekir. Mobil uygulama grupları Önizleme adlandırma ilkesinin göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hata döndürmez. Ancak adlandırma ilkesi oluştururken veya düzenlerken bir grubu otomatik olarak uygulanır ve kullanıcı grubu adı veya diğer özel engellenen sözcük varsa uygun hatalarla sunulur.
Planner | Planner adlandırma ilkesi ile uyumludur. Plan adı girerken, Planner adlandırma ilkesi Önizleme gösterir. Bir kullanıcı özel engellenen bir sözcük girdiğinde, planı oluşturulurken bir hata iletisi gösterilir.
Dynamics 365 müşteri katılımı için | Dynamics 365 müşteri katılımı için adlandırma ilkesi ile uyumludur. Kullanıcı grubu adı ya da Grup e-posta diğer adı yazdığında Dynamics 365 adlandırma zorlanan ilke adını gösterir. Kullanıcıyı kaldırmak için bir hata iletisi, kullanıcı özel engellenen bir sözcük girdiğinde, engellenen bir sözcük ile gösterilir.
School Data Sync'i (SDS) | SDS ile oluşturulan grupların adlandırma ilkesi ile uyumlu, ancak adlandırma ilkesi otomatik olarak uygulanmaz. SDS yöneticilerinin, önek ve sonek gruplarının oluşturulması ve ardından SDS için karşıya gerekir sınıf adları eklemek vardır. Grup oluşturma veya düzenleme, aksi takdirde başarısız olur.
Outlook Customer Manager'a (OCM) | Outlook Customer Manager, Outlook Customer Manager'a oluşturduğunuz gruba otomatik olarak uygulanan adlandırma ilkesi ile uyumludur. Özel engellenen bir sözcük algılanırsa, grup oluşturma OCM'deki engellenir ve kullanıcı OCM uygulama kullanımından engellenir.
Classroom uygulaması | Classroom uygulamasında oluşturulan grupların adlandırma ilkesiyle uyumlu ancak adlandırma ilkesi otomatik olarak uygulanmaz ve bir sınıf grup adı girerken, adlandırma ilkesi Önizleme kullanıcılara gösterilen değil. Kullanıcılar, önek ve sonek zorlanan classroom grup adıyla girmeniz gerekir. Aksi halde classroom grubu oluşturma veya düzenleme hatalarla işlemi başarısız.
Power BI | Power BI çalışma alanları adlandırma ilkesi ile uyumlu olması gerekir.    
Yammer | Azure Active Directory hesaplarıyla Yammer'a açan bir kullanıcı bir grup oluşturur ya da bir grup adı düzenler adlandırma ilkesinin grup adı uyacaktır. Bu, hem Office 365 bağlı grupları ve diğer tüm Yammer grupları için geçerlidir.<br>Grup adı otomatik olarak bir Office 365 bağlı Grup adlandırma ilkesi yerinde önce oluşturulduysa adlandırma ilkeleri izlenmez. Bir kullanıcı grubu adı düzenlediğinde öneki ve soneki eklemeniz istenir.
StaffHub  | StaffHub takımlar adlandırma ilkesi izlemeyin, ancak temel alınan Office 365 grubu yok. StaffHub takım adı bir önek ve sonek geçerli değildir ve özel engellenen sözcükler için denetlemez. Ancak StaffHub önek ve sonek geçerli ve engellenen bir sözcük temel alınan bir Office 365 grubundan kaldırır.
Exchange PowerShell | Exchange PowerShell cmdlet'leri adlandırma ilkesi ile uyumlu olması gerekir. Grup adı ve grup diğer adının (mailNickname) adlandırma ilkesi izlerseniz yoksa, kullanıcılar önerilen önek ve sonek ve özel engellenen sözcükler için uygun hata iletileri alır.
Azure Active Directory PowerShell cmdlet'leri | Azure Active Directory PowerShell cmdlet'lerini adlandırma ilkesi ile uyumlu olması gerekir. Kullanıcılar grubu adları ve grup diğer adının adlandırma kuralını takip etmiyorsa önerilen önek ve sonek ve özel engellenen sözcükler için uygun hata iletilerini alır.
Exchange yönetici merkezini | Exchange yönetici merkezini adlandırma ilkesi ile uyumludur. Grup adı ve grup diğer adının adlandırma kuralı izlerseniz yoksa, kullanıcılar önerilen önek ve sonek ve özel engellenen sözcükler için uygun hata iletileri alır.
Office 365 Yönetim Merkezi | Office 365 Yönetim Merkezi ilke adlandırma ile uyumludur. Ne zaman bir kullanıcı oluşturur veya düzenlemeleri grup adlarını adlandırma ilkesi otomatik olarak uygulanır ve bunlar özel engellenen sözcük girdiğinizde kullanıcılar uygun hataları alırsınız. Office 365 Yönetim Merkezi önizlemesi adlandırma ilkesi henüz göstermez ve kullanıcı grubu adı girdiğinde özel engellenen sözcük hataları dönmez.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleler, Azure AD grupları hakkında ek bilgi sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Office 365 gruplarının süre sonu ilkesi](groups-lifecycle.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyelerini yönetme](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
