---
title: Azure Stack ortak anahtar altyapısı sertifika gereksinimleri için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Azure Stack tümleşik sistemleri için Azure Stack PKI sertifika dağıtım gereksinimleri açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 13bc82caf5e10f5b35df29d085349ec4c80628a2
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929279"
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Azure Stack ortak anahtar altyapısı sertifika gereksinimleri

Azure Stack, küçük bir Azure Stack hizmetlerinin ve büyük olasılıkla Kiracı VM kümesine atanan harici olarak erişilebilen genel IP adresleri kullanan bir ortak altyapısı ağ vardır. Bu Azure Stack genel altyapı uç noktalar için uygun DNS adlarına sahip PKI sertifikaları, Azure Stack dağıtımı sırasında gereklidir. Bu makalede, hakkında bilgi sağlar:

- Azure Stack dağıtmak için hangi sertifikaların gereklidir
- Bu belirtimler eşleşen sertifikalarını alma işlemi
- Hazırlama, doğrulamak ve dağıtımı sırasında bu sertifikaları kullanın

> [!Note]  
> Dağıtım sırasında sertifikalar (Azure karşı AD veya AD FS) dağıtmakta olduğunuz kimlik sağlayıcısı ile eşleşen dağıtım klasörüne kopyalamanız gerekir. Tüm uç noktalar için tek bir sertifika kullanıyorsanız, bu sertifika dosyasını aşağıdaki tabloda özetlendiği gibi her dağıtım klasörüne kopyalamanız gerekir. Klasör yapısını dağıtım sanal makinede önceden oluşturulmuş ve şurada bulunabilir: C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Sertifika gereksinimleri
Aşağıdaki listede, Azure Stack dağıtmak için gerekli sertifika gereksinimleri açıklanmaktadır: 
- Bir iç sertifika yetkilisi veya bir ortak sertifika yetkilisi sertifikalarını verilmesi gerekir. Bir ortak sertifika yetkilisi kullandıysanız, temel işletim sistemi görüntüsü Microsoft güvenilir kök yetkilisi programının bir parçası olarak eklenmelidir. Tam listesini burada bulabilirsiniz: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- Azure Stack altyapınızı, sertifika yetkilisinin sertifika iptal listesi (CRL) konumuna sertifikada yayımlanan ağ erişimi olması gerekir. Bu CRL bir http uç noktası olmalıdır
- Sertifikaları döndürürken sertifikaları ya da dağıtım ya da yukarıdaki tüm ortak sertifika yetkilisinden verilen sertifikaları imzalamak için kullanılan aynı iç sertifika yetkilisinden verilen olmalıdır
- Otomatik olarak imzalanan sertifikaların kullanılması desteklenmiyor
- Azure Stack dağıtımı ve döndürme yapabilirsiniz veya tüm ad alanları sertifikanın konu adı ve konu alternatif adı (SAN) alanlarını kapsayan tek bir sertifikayı kullanın ya da kullanabilirsiniz, aşağıdaki ad alanlarının her biri için tek tek sertifikaları kullanmak için plan hizmetleri gerektirir. Not: joker gibi gerekli olduğu için uç noktaları kullanarak her iki yaklaşım gerektirir. **KeyVault** ve **KeyVaultInternal**. 
- Sertifika imza algoritması SHA1, olamaz, daha güçlü olması gerekir. 
- Ortak ve özel anahtarları Azure Stack yükleme için gerekli olduğu gibi PFX sertifika biçimi olmalıdır. 
- Sertifika pfx dosyasını bir değer "Dijital imza" ve "KeyEncipherment", "Anahtar kullanımı" alanında olması gerekir.
- Sertifika pfx dosyaları "Sunucu kimlik doğrulaması (1.3.6.1.5.5.7.3.1)" ve "İstemci kimlik doğrulaması (1.3.6.1.5.5.7.3.2)" değerlerini "Gelişmiş anahtar kullanımı" alanında olması gerekir.
- Sertifikanın "verilen:" alanı aynı olmamalıdır, "tarafından verilen:" alanı.
- Dağıtım sırasında tüm sertifika pfx dosyalarını parola aynı olmalıdır
- Karmaşık bir parola sertifika pfx parolası vardır.
- Konu adları ve konu alternatif adı uzantısı (x509v3_config) eşleşen konu diğer adları emin olun. Konu alternatif adı alanı ek konak adları (Web siteleri, IP adresleri, yaygın olarak kullanılan adları) tek bir SSL sertifikası tarafından korunacak belirtmenize olanak sağlar.

> [!NOTE]  
> Kendi kendine imza sertifikaları desteklenmez.

> [!NOTE]  
> Bir sertifika güven zinciri olduğu ara sertifika yetkililerini varlığını desteklenmiyor. 

## <a name="mandatory-certificates"></a>Zorunlu sertifikaları
Bu bölümde yer alan tabloda her iki Azure AD için gerekli olan Azure Stack genel uç noktası PKI sertifikalarını açıklar ve AD FS Azure Stack dağıtımları. Sertifika gereksinimleri, alan, aynı zamanda tarafından kullanılan ad alanları gruplandırılır ve her ad alanı için gerekli olan sertifikaları. Aşağıdaki tabloda, ayrıca, çözüm sağlayıcınızın farklı sertifikaları genel bir uç nokta başına kopyalayan klasörü açıklanmıştır. 

Her Azure Stack genel altyapı uç noktası için uygun DNS adlarına sahip sertifikaları gereklidir. Her uç noktasının DNS adı biçiminde ifade edilir:  *&lt;önek >.&lt; bölge >. &lt;fqdn >*. 

Dağıtımınız, [Bölge] ve [externalfqdn] değerleri bölge ve Azure Stack sisteminiz için seçtiğiniz dış etki alanı adlarının eşleşmesi gerekir. Örneğin bölge adı varsa, *Redmond* ve dış etki alanı adı *contoso.com*, DNS adlarını biçimi gerekir *&lt;önek >. redmond.contoso.com*. *&lt;Önek >* değerleri predesignated güvenliği sertifika ile sağlanan uç nokta tanımlamak amacıyla Microsoft tarafından. Ayrıca,  *&lt;önek >* değerler dış altyapı uç noktalarının belirli uç noktasını kullanan Azure Stack hizmeti bağlıdır. 

> [!note]  
> Tüm ad alanları tüm dizinlere kopyalanır konu ve konu alternatif adı (SAN) alanlarını da kapsayan tek joker karakter sertifika her biri için ayrı sertifikalar olarak veya uç nokta karşılık gelen dizine kopyalanır. sağlanan sertifika olabilir. Unutmayın, iki seçenek de joker karakterli sertifikalar için bitiş noktaları gibi kullanmanızı gerektirir **acs** ve gerekli nerede anahtar kasası. 

| Dağıtım klasörü | Gerekli bir sertifika konusu ve konu alternatif adları (SAN) | Kapsam (bölge başına) | Alt etki alanı ad alanı |
|-------------------------------|------------------------------------------------------------------|----------------------------------|-----------------------------|
| Genel kullanıma açık portala | Portalı. &lt;bölge >. &lt;fqdn > | Portallar | &lt;region>.&lt;fqdn> |
| Yönetici portalı | adminportal. &lt;bölge >. &lt;fqdn > | Portallar | &lt;region>.&lt;fqdn> |
| Azure Resource Manager'a genel | yönetimi. &lt;bölge >. &lt;fqdn > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| Azure Resource Manager Yöneticisi | adminmanagement. &lt;bölge >. &lt;fqdn > | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| ACSBlob | *.blob.&lt;region>.&lt;fqdn><br>(Joker SSL sertifikası) | Blob Depolama | BLOB. &lt;bölge >. &lt;fqdn > |
| ACSTable | * .table. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) | Tablo Depolama | Tablo. &lt;bölge >. &lt;fqdn > |
| ACSQueue | * .queue. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) | Kuyruk Depolama | Kuyruk. &lt;bölge >. &lt;fqdn > |
| KeyVault | * .vault. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) | Key Vault | Kasa. &lt;bölge >. &lt;fqdn > |
| KeyVaultInternal | *.adminvault. &lt;bölge >. &lt;fqdn ><br>(Joker SSL sertifikası) |  İç anahtar kasası |  adminvault. &lt;bölge >. &lt;fqdn > |

Azure Stack Azure AD dağıtım modunu kullanarak dağıtırsanız, yalnızca önceki tabloda listelenen sertifika istemeniz gerekir. Ancak, Azure Stack AD FS dağıtım modunu kullanarak dağıtırsanız, aşağıdaki tabloda açıklanan sertifikaları da isteğinde gerekir:

|Dağıtım klasörü|Gerekli bir sertifika konusu ve konu alternatif adları (SAN)|Kapsam (bölge başına)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|ADFS|ADFS.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası)|ADFS|*&lt;bölge >. &lt;fqdn >*|
|Graf|Grafiği.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası)|Graf|*&lt;bölge >. &lt;fqdn >*|
|

> [!IMPORTANT]
> Bu bölümde listelenen tüm sertifikalar aynı parolayı olması gerekir. 

## <a name="optional-paas-certificates"></a>İsteğe bağlı PaaS sertifikaları
Planlıyorsanız, ek Azure Stack PaaS hizmetler (SQL, MySQL ve App Service) sonra Azure Stack dağıtmayı dağıtılan ve yapılandırıldı, PaaS Hizmetleri uç noktaları kapsayacak şekilde ek bir sertifika istemeniz gerekir. 

> [!IMPORTANT]
> App Service, SQL ve MySQL kaynak sağlayıcıları için kullandığınız sertifikaların genel Azure Stack uç noktaları için kullanılan aynı kök yetkilisi gerekir. 

Aşağıdaki tabloda, SQL ve MySQL bağdaştırıcısı ve App Service için gerekli sertifikalar ve uç noktaları açıklar. Bu sertifikalar Azure Stack dağıtım klasörüne kopyalamanız gerekmez. Bunun yerine, ek kaynak sağlayıcısı yüklediğinizde, bu sertifikalar sağlar. 

|Kapsam (bölge başına)|Sertifika|Gerekli bir sertifika konusu ve konu alternatif adları (SAN)|Alt etki alanı ad alanı|
|-----|-----|-----|-----|
|SQL, MySQL|SQL ve MySQL|&#42;.dbadapter.  *&lt;bölge >.&lt; FQDN >*<br>(Joker SSL sertifikası)|dbadapter.*&lt;region>.&lt;fqdn>*|
|App Service|Web trafiği varsayılan SSL sertifikası|&#42;.appservice.  *&lt;bölge >.&lt; FQDN >*<br>&#42;. scm.appservice.  *&lt;bölge >.&lt; FQDN >*<br>&#42;. sso.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(Çoklu etki alanı joker SSL sertifikası<sup>1</sup>)|appservice.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|
|App Service|API|api.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|appservice.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|
|App Service|FTP|FTP.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|appservice.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|
|App Service|SSO|SSO.appservice.  *&lt;bölge >.&lt; FQDN >*<br>(SSL sertifikası<sup>2</sup>)|appservice.  *&lt;bölge >.&lt; FQDN >*<br>SCM.appservice.  *&lt;bölge >.&lt; FQDN >*|

<sup>1</sup> birden fazla joker konu alternatif adı ile bir sertifika gerektirir. Tek bir sertifikanın birden fazla joker karakter SAN'lar tüm ortak sertifika yetkilileri tarafından desteklenmiyor olabilir 

<sup>2</sup> A &#42;.appservice. *&lt;bölge >. &lt;fqdn >* joker karakter sertifika yerine bu üç sertifika kullanılamaz (api.appservice. *&lt;bölge >. &lt;fqdn >*, ftp.appservice. *&lt;bölge >. &lt;fqdn >* ve sso.appservice. *&lt;bölge >. &lt;fqdn >*. Appservice, açıkça Bu uç noktalar için ayrı sertifikaların kullanımını gerektirir. 

## <a name="learn-more"></a>Daha fazla bilgi edinin
Bilgi edinmek için nasıl [Azure Stack dağıtımı için PKI sertifikalarını oluşturmak](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Sonraki adımlar
[Kimlik Tümleştirme](azure-stack-integrate-identity.md)

