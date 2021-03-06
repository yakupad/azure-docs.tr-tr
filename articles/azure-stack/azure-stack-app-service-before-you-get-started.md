---
title: Azure Stack üzerinde App Service'te dağıtmadan önce | Microsoft Docs
description: Azure Stack üzerinde App Service'te dağıtmadan önce tamamlanması gereken adımları
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: anwestg
ms.openlocfilehash: 22901374988f6654bc1fb282315db81bb17c815f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857874"
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Azure Stack üzerinde App Service ile çalışmaya başlamadan önce

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te Azure App Service'ı dağıtmadan önce bu makalede bölümündeki önkoşul adımlarını tamamlamanız gerekir.

> [!IMPORTANT]
> 1804 güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya Azure App Service 1.2 dağıtmadan önce en son Azure Stack geliştirme Seti'ni (ASDK) dağıtın.

## <a name="download-the-installer-and-helper-scripts"></a>Yükleyici ve yardımcı betikleri indirin

1. İndirme [dağıtım yardımcı betikleri Azure Stack üzerinde App Service'te](https://aka.ms/appsvconmashelpers).
2. İndirme [yükleyici Azure Stack üzerinde App Service'te](https://aka.ms/appsvconmasinstaller).
3. Yardımcı betikleri .zip dosyasından dosyaları ayıklayın. Aşağıdaki dosya ve klasörleri ayıklanır:

   - Common.ps1
   - Oluşturma AADIdentityApp.ps1
   - Oluşturma ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Modülleri klasöründe
     - GraphAPI.psm1

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure Stack 1802 güncelleştirme, hata etki alanları için destek eklendi. Azure Stack'te Azure App Service'in yeni dağıtımlar hata etki alanlarına dağıtılmış ve hataya dayanıklılık sağlar.

1802 güncelleştirmeden önce dağıtılan var olan dağıtımlar Azure Stack'te Azure App Service'in için bkz. [bir App Service kaynak sağlayıcısı, hata etki alanları arasında yeniden dengelemeniz](azure-stack-app-service-fault-domain-update.md) makalesi.

Ayrıca, gerekli bir dosya sunucusu ve SQL Server örneklerini yüksek oranda kullanılabilir bir yapılandırmada dağıtın.

## <a name="get-certificates"></a>Sertifikaları Al

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure Stack için Azure Resource Manager kök sertifika

Ayrıcalıklı uç noktada Azure Stack tümleşik sistemi veya Azure Stack Geliştirme Seti konak ulaşabileceği bir bilgisayarda yükseltilmiş bir PowerShell oturumu açın.

Çalıştırma *Get-AzureStackRootCert.ps1* yardımcı betikleri ayıkladığınız klasöre betikten. Betik, App Service sertifikaları oluşturmak için gereken betik ile aynı klasörde bir kök sertifika oluşturur.

Aşağıdaki PowerShell komutunu çalıştırdığınızda AzureStack\CloudAdmin için ayrıcalıklı uç noktasını ve kimlik bilgilerini sağlamanız gerekir.

```PowerShell
    Get-AzureStackRootCert.ps1
```

#### <a name="get-azurestackrootcertps1-script-parameters"></a>Get-AzureStackRootCert.ps1 betik parametreleri

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| PrivilegedEndpoint | Gerekli | AzS-ERCS01 | Ayrıcalıklı uç noktası |
| CloudAdminCredential | Gerekli | AzureStack\CloudAdmin | Azure Stack bulut yöneticileri etki alanı hesabı kimlik bilgileri |

### <a name="certificates-required-for-asdk-deployment-of-azure-app-service"></a>Azure App Service'in ASDK dağıtım için gerekli sertifikaları

*Oluştur AppServiceCerts.ps1* betik, App Service gerektiren dört sertifikaları oluşturmak için Azure Stack sertifika yetkilisi ile çalışır.

| Dosya adı | Kullanım |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | App Service varsayılan SSL sertifikası |
| api.appservice.local.azurestack.external.pfx | App Service API SSL sertifikası |
| ftp.appservice.local.azurestack.external.pfx | App Service yayımcı SSL sertifikası |
| sso.appservice.local.azurestack.external.pfx | App Service uygulama kimliği sertifikası |

Sertifikaları oluşturmak için aşağıdaki adımları izleyin:

1. Azure Stack geliştirme Seti'ni konağa AzureStack\AzureStackAdmin hesabını kullanarak oturum açın.
2. Yükseltilmiş bir PowerShell oturumu açın.
3. Çalıştırma *Oluştur AppServiceCerts.ps1* yardımcı betikleri ayıkladığınız klasöre betikten. Bu betik, App Service sertifikaları oluşturmak için gereken betik ile aynı klasörde dört sertifikaları oluşturur.
4. .Pfx dosyaları güvenli hale getirmek için bir parola girin ve bir not edin. Yükleyici Azure Stack üzerinde App Service'te girmek zorunda kalırsınız.

#### <a name="create-appservicecertsps1-script-parameters"></a>Oluşturma AppServiceCerts.ps1 betik parametreleri

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| pfxPassword | Gerekli | Null | Parola sertifika özel anahtarını korunmasına yardımcı olur |
| DomainName | Gerekli | Local.azurestack.external | Azure Stack bölge ve etki alanı soneki |

### <a name="certificates-required-for-azure-stack-production-deployment-of-azure-app-service"></a>Azure App Service'in Azure Stack Üretim dağıtımı için gerekli sertifikaları

Kaynak sağlayıcısı üretim ortamında çalıştırmak için aşağıdaki sertifikalar sağlamanız gerekir:

- Varsayılan etki alanı sertifikası
- API sertifikası
- Yayımlama sertifikası
- Kimlik sertifikası

#### <a name="default-domain-certificate"></a>Varsayılan etki alanı sertifikası

Varsayılan etki alanı sertifikası ön uç rolüne yerleştirilir. Azure App Service'e joker veya varsayılan etki alanı isteği için kullanıcı uygulamaları bu sertifikayı kullanın. Sertifika Ayrıca kaynak denetimi işlemleri (Kudu) için kullanılır.

Sertifika .pfx biçiminde olmalıdır ve üç konulu bir joker sertifika olmalıdır. Bu gereksinim, hem varsayılan etki alanı hem de kaynak denetimi işlemleri için SCM uç noktasının kapsayan bir sertifika sağlar.

| Biçimlendir | Örnek |
| --- | --- |
| \*.appservice. \<bölge\>.\< DomainName\>.\< Uzantı\> | \*.appservice.redmond.azurestack.external |
| \*. scm.appservice. <region>. <DomainName>.<extension> | \*.scm.appservice.redmond.azurestack.external |
| \*. sso.appservice. <region>. <DomainName>.<extension> | \*.sso.appservice.redmond.azurestack.external |

#### <a name="api-certificate"></a>API sertifikası

API sertifikasını Yönetim rolüne yerleştirilir. Kaynak sağlayıcısı güvenli API çağrıları yardımcı olmak için kullanır. Yayımlama sertifikası API DNS girişi ile eşleşen bir konu içermelidir.

| Biçimlendir | Örnek |
| --- | --- |
| api.appservice. \<bölge\>.\< DomainName\>.\< Uzantı\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Yayımlama sertifikası

Bunlar içeriği karşıya yüklediğinizde yayımcı rolünün sertifikası için uygulama sahipleri FTPS trafiğinin güvenliğini sağlar. Yayımlama sertifikası FTPS DNS girişi ile eşleşen bir konu içermelidir.

| Biçimlendir | Örnek |
| --- | --- |
| FTP.appservice. \<bölge\>.\< DomainName\>.\< Uzantı\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Kimlik sertifikası

Identity application sertifikası sağlar:

- Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) dizin, Azure Stack ve App Service işlem kaynak sağlayıcısı ile destek tümleştirmesine arasında tümleştirme.
- Çoklu oturum açma senaryoları için Azure Stack'te Azure App Service içinde Gelişmiş geliştirici araçları.

Sertifika kimliği şu biçimde eşleşen bir konu içermelidir.

| Biçimlendir | Örnek |
| --- | --- |
| SSO.appservice. \<bölge\>.\< DomainName\>.\< Uzantı\> | sso.appservice.redmond.azurestack.external |

## <a name="virtual-network"></a>Sanal ağ

Azure Stack'te Azure App Service kaynak sağlayıcısı için mevcut bir sanal ağı dağıtmanıza olanak tanır veya dağıtımın bir parçası bir sanal ağ oluşturmanıza olanak sağlar. Mevcut bir sanal ağ kullanarak dosya sunucusu ve Azure Stack'te Azure App Service için gerekli SQL Server'a bağlanmak için iç IP'ler kullanımını etkinleştirir. Sanal ağ aşağıdaki adres aralığını ve alt ağlar ile Azure Stack'te Azure App Service yüklemeden önce yapılandırılmalıdır:

Sanal ağ - /16

Alt ağlar

- ControllersSubnet /24
- ManagementServersSubnet /24
- FrontEndsSubnet /24
- PublishersSubnet /24
- WorkersSubnet /21

## <a name="prepare-the-file-server"></a>Dosya sunucusunu hazırlama

Azure App Service, bir dosya sunucusu kullanılmasını gerektirir. Üretim dağıtımları için dosya sunucusu yüksek oranda kullanılabilir ve hataları işleme yeteneğine sahip olacak şekilde yapılandırılması gerekir.

Yalnızca Azure Stack geliştirme Seti'ni dağıtımlar için kullandığınız [örnek Azure Resource Manager dağıtım şablonu](https://aka.ms/appsvconmasdkfstemplate) yapılandırılmış tek düğümlü dosya sunucusu dağıtmak için. Tek düğümlü dosya sunucusu bir çalışma grubunda olacaktır.

>[!IMPORTANT]
> App Service'ta da mevcut bir sanal ağ dağıtmayı tercih ederseniz dosya sunucusu App Service'ten ayrı bir alt ağa dağıtılması gerekir.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Gruplar ve Active Directory hesaplarını sağlama

1. Aşağıdaki Active Directory genel güvenlik gruplarını oluşturun:

   - FileShareOwners
   - FileShareUsers

2. Hizmet hesapları olarak aşağıdaki Active Directory hesaplarını oluşturun:

   - FileShareOwner
   - FileShareUser

   Güvenlik en iyi uygulama, kullanıcıların bu hesapların (ve tüm web rolleri için) benzersiz ve güçlü kullanıcı adları ve parolalar olması gerekir. Parolaları aşağıdaki koşullarla ayarlayın:

   - Etkinleştirme **parola her zaman geçerli olsun**.
   - Etkinleştirme **kullanıcı parolayı değiştiremez**.
   - Devre dışı **kullanıcının sonraki oturum açışında parolasını değiştirmesi**.

3. Hesapları grup üyeliklerine aşağıdaki şekilde ekleyin:

   - Ekleme **FileShareOwner** için **FileShareOwners** grubu.
   - Ekleme **FileShareUser** için **FileShareUsers** grubu.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Gruplar ve hesaplar bir çalışma grubunda sağlama

>[!NOTE]
> Ne zaman yapılandırmakta olduğunuz tüm aşağıdaki komutları çalıştırın, bir dosya sunucusu bir **yönetici komut istemi**. <br>***PowerShell kullanmayın.***

Azure Resource Manager şablonunu kullandığınızda, kullanıcılar zaten oluşturulmuştur.

1. FileShareOwner ve FileShareUser hesaplarını oluşturmak için aşağıdaki komutları çalıştırın. Değiştirin `<password>` kendi değerlerinizle.

   ``` DOS
   net user FileShareOwner <password> /add /expires:never /passwordchg:no
   net user FileShareUser <password> /add /expires:never /passwordchg:no
   ```

2. Aşağıdaki WMIC komutları çalıştırarak süresiz olarak hesaplar için parolaları ayarlayın:

   ``` DOS
   WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
   WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
   ```

3. FileShareUsers ve FileShareOwners yerel gruplarını oluşturun ve bunları Birinci adımdaki hesapları ekleyin:

   ``` DOS
   net localgroup FileShareUsers /add
   net localgroup FileShareUsers FileShareUser /add
   net localgroup FileShareOwners /add
   net localgroup FileShareOwners FileShareOwner /add
   ```

### <a name="provision-the-content-share"></a>İçerik paylaşımı sağlama

İçerik paylaşımı Kiracı Web sitesi içeriğini içerir. Tek dosya sunucusunda içerik paylaşımı sağlama yordamı, Active Directory ve çalışma grubu ortamları için aynıdır. Ancak, Active Directory'deki bir yük devretme kümesi için farklıdır.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>Tek bir dosya sunucusunda (Active Directory veya çalışma grubu) içerik paylaşımı sağlama

Tek dosya sunucusunda, yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın. Değeri Değiştir `C:\WebSites` , ortamınızdaki ilgili yollarla.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="add-the-fileshareowners-group-to-the-local-administrators-group"></a>FileShareOwners grubunu yerel Administrators grubuna ekleyin.

Windows Uzaktan düzgün çalışması Yönetim için yerel Administrators grubuna FileShareOwners grubunu eklemeniz gerekir.

#### <a name="active-directory"></a>Active Directory

Aşağıdaki komutları yükseltilmiş komut isteminde, dosya sunucusunda veya yük devretme kümesi düğümü davranan her dosya sunucusunda çalıştırın. Değeri Değiştir `<DOMAIN>` kullanmak istediğiniz etki alanı adına sahip.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Çalışma grubu

Aşağıdaki komutu yükseltilmiş bir komut isteminde dosya sunucusunda çalıştırın:

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Paylaşımlara erişim denetimini yapılandırın

Dosya sunucusunda veya geçerli küme kaynak sahibi yük devretme kümesi düğümünde, aşağıdaki komutları yükseltilmiş komut isteminde çalıştırın. İtalik değerleri ortamınıza özgü değerlerle değiştirin.

#### <a name="active-directory"></a>Active Directory

```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Çalışma grubu

```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server-instance"></a>SQL Server örneği hazırlamak

Azure Stack barındırma ve ölçüm veritabanları üzerinde Azure App Service için App Service veritabanlarını tutmak için bir SQL Server örneği hazırlamanız gerekir.

Azure Stack geliştirme Seti'ni dağıtımları için SQL Server Express 2014 SP2 kullanabilirsiniz veya üzeri.

Üretim ve yüksek kullanılabilirlik amaçları için SQL Server 2014 SP2 tam bir sürümünü kullanın veya daha sonra karma mod kimlik doğrulamasını etkinleştirmek ve gerekir dağıtın bir [yüksek oranda kullanılabilir yapılandırma](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

Azure Stack'te Azure App Service için SQL Server örneğinin tüm App Service rollerinden erişilebilmelidir. SQL Server Azure Stack'te varsayılan sağlayıcı aboneliği içinde dağıtabilirsiniz. Ya da kuruluşunuz içinde var olan altyapının (var olduğu sürece Azure Stack bağlanabilirliği) kullanın. Azure Market görüntüsü kullanıyorsanız, güvenlik duvarı uygun şekilde yapılandırmayı unutmayın.

>[!NOTE]
> SQL Iaas sanal makine görüntüleri birçok Market yönetimi özelliği yoluyla kullanılabilir. Bir Market öğesi kullanarak VM dağıtmadan önce her zaman SQL Iaas uzantısı en son sürümünü indirin emin olun. SQL görüntülerinin Azure'da kullanıma sunulan SQL VM'ler ile aynıdır. SQL Iaas uzantısı bu görüntülerden oluşturulan ve portal geliştirmeleri karşılık gelen VM'ler için otomatik düzeltme eki uygulama ve yedekleme özellikleri gibi özellikler sağlar.
>
Herhangi bir SQL sunucu rolleri için varsayılan bir örnek veya adlandırılmış bir örnek kullanabilirsiniz. Adlandırılmış bir örnek kullanırsanız, el ile SQL Server Browser hizmetini başlatma ve bağlantı noktası 1434'ü açın emin olun.

>[!IMPORTANT]
> App Service'ta da mevcut bir sanal ağ dağıtmayı tercih ederseniz SQL Server App Service ve dosya sunucusu ayrı bir alt ağa dağıtılması gerekir.
>

## <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

Aşağıdaki işlemleri desteklemek için bir Azure AD hizmet sorumlusu yapılandırın:

- Sanal makine ölçek üzerinde çalışan katmanları tümleştirme ayarlayın.
- SSO için Azure işlevleri portal ve Gelişmiş geliştirici araçları.

Bu adımlar yalnızca Azure AD tarafından güvenliği sağlanan Azure Stack ortamları için geçerlidir.

Yöneticiler, SSO için yapılandırmanız gerekir:

- App Service (Kudu) içinde Gelişmiş Geliştirici Araçları'nı etkinleştirin.
- Azure işlevleri portal deneyimi kullanımını etkinleştirin.

Şu adımları uygulayın:

1. Bir PowerShell örneği azurestack\AzureStackAdmin açın.
2. Yüklediğiniz ve açtığınız içinde betikleri konumunu Git [önkoşul adım](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
4. Çalıştırma **Oluştur AADIdentityApp.ps1** betiği. İstendiğinde, Azure Stack dağıtımınız için kullandığınız Azure AD Kiracı Kimliğinizi girin. Örneğin, **myazurestack.onmicrosoft.com**.
5. İçinde **kimlik bilgisi** penceresinde, Azure AD Hizmet Yöneticisi hesabını ve parolayı girin. **Tamam**’ı seçin.
6. Sertifika dosyası yolu ve sertifika parolasını girin [daha önce oluşturduğunuz sertifika](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Varsayılan olarak bu adım için oluşturulan sertifika **sso.appservice.local.azurestack.external.pfx**.
7. Betik Kiracı Azure AD örneğinde yeni bir uygulama oluşturur. PowerShell çıkışında döndürülen uygulama Kimliğini not edin. Bu bilgiler yükleme sırasında ihtiyacınız var.
8. Yeni bir tarayıcı penceresi açın ve oturum [Azure portalında](https://portal.azure.com) Azure Active Directory Hizmet Yöneticisi olarak
9. Azure AD kaynak Sağlayıcısı'nı açın.
10. Seçin **uygulama kayıtları**.
11. Adım 7 bir parçası olarak döndürülen uygulama Kimliğini arayın. Bir App Service uygulama listelenir.
12. Seçin **uygulama** listesinde.
13. Seçin **ayarları**.
14. Seçin **gerekli izinler** > **izinleri verin** > **Evet**.

```PowerShell
    Create-AADIdentityApp.ps1
```

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| DirectoryTenantName | Gerekli | Null | Azure AD Kiracı kimliği GUID veya dize sağlayın. Myazureaaddirectory.onmicrosoft.com buna bir örnektir. |
| AdminArmEndpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Adminmanagement.local.azurestack.external buna bir örnektir. |
| TenantARMEndpoint | Gerekli | Null | Kiracı Azure Resource Manager uç noktası. Management.local.azurestack.external buna bir örnektir. |
| AzureStackAdminCredential | Gerekli | Null | Azure AD Hizmet Yöneticisi kimlik bilgisi. |
| CertificateFilePath | Gerekli | Null | **Tam yol** daha önce oluşturulan Identity application sertifika dosyası için. |
| CertificatePassword | Gerekli | Null | Parola, sertifika özel anahtarını korunmasına yardımcı olur. |

## <a name="create-an-active-directory-federation-services-application"></a>Active Directory Federasyon Hizmetleri uygulama oluşturma

AD FS tarafından güvenliği sağlanan Azure Stack ortamlarında aşağıdaki işlemleri desteklemek için bir AD FS hizmet sorumlusu yapılandırmanız gerekir:

- Sanal makine ölçek üzerinde çalışan katmanları tümleştirme ayarlayın.
- SSO için Azure işlevleri portal ve Gelişmiş geliştirici araçları.

Yöneticiler, SSO için yapılandırmanız gerekir:

- Sanal makine ölçek kümesi tümleştirme için hizmet sorumlusu üzerinde çalışan katmanları yapılandırın.
- App Service (Kudu) içinde Gelişmiş Geliştirici Araçları'nı etkinleştirin.
- Azure işlevleri portal deneyimi kullanımını etkinleştirin.

Şu adımları uygulayın:

1. Bir PowerShell örneği azurestack\AzureStackAdmin açın.
2. Yüklediğiniz ve açtığınız içinde betikleri konumunu Git [önkoşul adım](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).
4. Çalıştırma **Oluştur ADFSIdentityApp.ps1** betiği.
5. İçinde **kimlik bilgisi** penceresinde, AD FS bulut yönetici hesabı ve parola girin. **Tamam**’ı seçin.
6. Sertifika dosyası yolu ve sertifika parolasını sağlayın [daha önce oluşturduğunuz sertifika](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Varsayılan olarak bu adım için oluşturulan sertifika **sso.appservice.local.azurestack.external.pfx**.

```PowerShell
    Create-ADFSIdentityApp.ps1
```

| Parametre | Gerekli veya isteğe bağlı | Varsayılan değer | Açıklama |
| --- | --- | --- | --- |
| AdminArmEndpoint | Gerekli | Null | Yönetici Azure Resource Manager uç noktası. Adminmanagement.local.azurestack.external buna bir örnektir. |
| PrivilegedEndpoint | Gerekli | Null | Ayrıcalıklı uç noktası. AzS-ERCS01 buna bir örnektir. |
| CloudAdminCredential | Gerekli | Null | Azure Stack bulut yöneticileri için etki alanı hesabı kimlik bilgisi. Azurestack\CloudAdmin buna bir örnektir. |
| CertificateFilePath | Gerekli | Null | **Tam yol** kimlik uygulamanın Sertifika PFX dosyası. |
| CertificatePassword | Gerekli | Null | Parola, sertifika özel anahtarını korunmasına yardımcı olur. |

## <a name="next-steps"></a>Sonraki adımlar

[App Service kaynak Sağlayıcısı'nı yükleme](azure-stack-app-service-deploy.md)
