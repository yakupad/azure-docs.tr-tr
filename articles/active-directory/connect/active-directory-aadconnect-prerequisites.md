---
title: 'Azure AD Connect: Önkoşulları ve donanım | Microsoft Docs'
description: Bu konu ön koşullar ve Azure AD Connect için donanım gereksinimlerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/09/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 22751d7ab38717fefdebe107e7a7d6fc10dda4c4
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39326199"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Azure AD Connect önkoşulları
Bu konu ön koşullar ve Azure AD Connect için donanım gereksinimlerini açıklar.

## <a name="before-you-install-azure-ad-connect"></a>Azure AD Connect'i yüklemeden önce
Azure AD Connect'i yüklemeden önce gereken birkaç şey vardır.

### <a name="azure-ad"></a>Azure AD
* Bir Azure aboneliği veya bir [Azure deneme aboneliğine](https://azure.microsoft.com/pricing/free-trial/). Bu aboneliğin yalnızca olduğu için Azure AD Connect kullanarak değil ve Azure portalına erişim için gereklidir. Daha sonra PowerShell veya Office 365 kullanıyorsanız, Azure AD Connect'i kullanmak için bir Azure aboneliği gerekmez. Office 365 lisansı varsa, Office 365 portalını da kullanabilirsiniz. Ücretli bir Office 365 lisansı ile Azure portalında Office 365 portalından alabilirsiniz.
  * Ayrıca [Azure portalında](https://portal.azure.com). Bu portalı bir Azure AD lisansı gerektirmez.
* [Ekleme ve etki alanı doğrulama](../active-directory-domains-add-azure-portal.md) Azure AD'de kullanmayı planlayın. Örneğin, contoso.com kullanıcılarınız için kullanın. ardından emin planlıyorsanız, bu etki alanı doğrulandı ve contoso.onmicrosoft.com varsayılan etki alanı yalnızca kullanmıyorsanız.
* Azure AD kiracısı tarafından varsayılan 50 k nesneleri sağlar. Etki alanınızı doğrulayın, sınır 300 k nesnelere artar. Azure AD'de daha da fazla nesneleri ihtiyacınız varsa daha da artırılmış sınırda olacak şekilde bir destek talebi açmanız gerekir. 500'den fazla k nesneleri gerekiyorsa, Office 365, Azure AD temel, Azure AD Premium veya Enterprise Mobility ve güvenlik gibi bir lisans gerekir.
* ADSyncPrep Azure AD Connect için Active Directory ortamınızı hazırlamak için kullanılan işlevleri sağlayan bir PowerShell Betiği modüldür.  ADSyncPrep gerektirir [Azure AD Microsoft çevrimiçi v1.1 PowerShell Modülü](https://docs.microsoft.com/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0).  Sürüm 2 işe yaramaz.  Modülünü kullanarak yükleyebilirsiniz `Install-Module` cmdlet'i.  Daha fazla bilgi için sağlanan bağlantıya bakın.

### <a name="prepare-your-on-premises-data"></a>Şirket içi verilerinizi hazırlayın
* Kullanım [Idfix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) Azure AD'ye eşitlemeden önce yinelemeleri ve dizininizdeki biçimlendirme sorunları gibi hataları belirlemek için ve Office 365.
* Gözden geçirme [isteğe bağlı eşitleme özellikleri Azure AD'de etkinleştirebilirsiniz](active-directory-aadconnectsyncservice-features.md) ve hangi özelliklerin etkinleştirmelisiniz değerlendirin.

### <a name="on-premises-active-directory"></a>Şirket içi Active Directory
* AD şema sürümü ve orman işlev düzeyi Windows Server 2003 veya üstü olmalıdır. Etki alanı denetleyicileri, şema ve orman düzeyinde gereksinimlerin karşılanıp karşılanmadığından sürece herhangi bir sürümü çalıştırabilirsiniz.
* Özelliğini kullanmayı planlıyorsanız **parola geri yazma**, etki alanı denetleyicileri (en son SP ile) Windows Server 2008 veya üzeri olmalıdır. 2008'de (pre-R2), DC'leri olan sonra uygulamanız gerekir [düzeltme KB2386717](http://support.microsoft.com/kb/2386717).
* Azure AD tarafından kullanılan etki alanı denetleyicisi yazılabilir olmalıdır. Bu **desteklenmiyor** RODC (salt okunur etki alanı denetleyicisi) ve Azure AD Connect tüm yazma yeniden yönlendirmeleri izlemez.
* Bu **desteklenmiyor** şirket içi ormanlar / "noktalı" kullanarak etki alanı kullanmak için (adında nokta ".") NetBIOS adları.
* Önerilir [Active Directory geri dönüşüm kutusunu etkinleştirme](active-directory-aadconnectsync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Azure AD Connect sunucusu
* Azure AD Connect, Windows Server Essentials ve Small Business Server açıp yüklenemez. Sunucu Windows Server standard veya daha iyi kullanıyor olmanız gerekir.
* Azure AD Connect sunucusu tam GUI yüklü olması gerekir. Bu **desteklenmiyor** sunucu Çekirdeğinde yüklemek için.
* Azure AD Connect, Windows Server 2008 veya üzeri yüklü olmalıdır. Hızlı ayarları kullanarak, bu sunucu bir etki alanı denetleyicisi veya üye sunucu olabilir. Özel ayarları kullanırsanız, sunucunun tek başına olabilir ve bir etki alanına katılması gerekmez.
* Windows Server 2008 veya Windows Server 2008 R2'de Azure AD Connect'i yüklemek, Windows Update'ten en son düzeltmeler uyguladığınızdan emin olun. Yükleme yüklenmemiş bir sunucuyla başlatmanız mümkün değil.
* Özelliğini kullanmayı planlıyorsanız **parola eşitleme**, Azure AD Connect sunucusu, Windows Server 2008 R2 SP1 veya üstü olmalıdır.
* Kullanmayı planlıyorsanız bir **Grup yönetilen hizmet hesabı**, Azure AD Connect sunucusu, Windows Server 2012 veya üzeri olmalıdır.
* Azure AD Connect sunucusu olmalıdır [.NET Framework 4.5.1](#component-prerequisites) veya üzeri ve [Microsoft PowerShell 3.0](#component-prerequisites) veya sonraki bir sürümü yüklü.
* Azure AD Connect sunucusu PowerShell Transkripsiyonu Grup İlkesi etkin olmaması gerekir.
* Active Directory Federasyon Hizmetleri dağıtılıyor, Windows Server 2012 R2 AD FS veya Web uygulaması Ara sunucusu yüklendiği sunucuları olmalıdır veya üzeri. [Windows Uzaktan Yönetimi](#windows-remote-management) bu sunucularda uzak yükleme için etkinleştirilmesi gerekir.
* Active Directory Federasyon Hizmetleri dağıtılıyor gerekirse [SSL sertifikaları](#ssl-certificate-requirements).
* Active Directory Federasyon Hizmetleri dağıtıldığı sonra yapılandırmanız gereken [ad çözümlemesi](#name-resolution-for-federation-servers).
* Genel Yöneticiler varsa MFA etkin, ardından URL'yi **https://secure.aadcdn.microsoftonline-p.com** Güvenilen siteler listesinde olmalıdır. Bir MFA testini için istenir ve bu önce eklemiştir değil Bu siteyi Güvenilen siteler listesine eklemeniz istenir. Güvenilen siteler listenize eklemek Internet Explorer'ı kullanabilirsiniz.

### <a name="sql-server-used-by-azure-ad-connect"></a>Azure AD Connect tarafından kullanılan SQL Server
* Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Varsayılan olarak, bir SQL Server 2012 Express LocalDB'nı (SQL Server Express'in hafif bir sürüm) yüklenir. SQL Server Express yaklaşık 100.000 nesneye yönetmenize olanak sağlayan bir 10 GB boyut sınırı vardır. Dizin nesneleri daha yüksek hacimde yönetmeniz gerekiyorsa, Yükleme Sihirbazı'nı farklı bir SQL Server yüklemesine işaret edecek şekilde gerekir.
* Ayrı bir SQL Server kullanıyorsanız, ardından bu gereksinimleri geçerlidir:
  * Azure AD Connect, SQL Server 2008 (en son hizmet paketiyle) SQL Server 2016 SP1, Microsoft SQL Server'ın tüm sürümleri destekler. Microsoft Azure SQL veritabanı **desteklenmiyor** veritabanı olarak.
  * Büyük küçük harf duyarsız bir SQL harmanlaması kullanmanız gerekir. Bu harmanlamaları ile tanımlanan bir \_CI_ adında. Bu **desteklenmiyor** tarafından tanımlanan bir büyük/küçük harfe harmanlamasını kullanacak şekilde \_CS_ adında.
  * Yalnızca bir eşitleme Altyapısı SQL örneği başına olabilir. Bu **desteklenmiyor** FIM/MIM eşitleme, DirSync veya Azure AD Sync ile bir SQL örneği paylaşmak için.

### <a name="accounts"></a>Hesaplar
* Azure AD kiracısı için Azure AD genel yönetici hesabı ile tümleştirmek istediğiniz. Bu hesap olmalıdır bir **okul veya kuruluş hesabı** ve bir **Microsoft hesabı**.
* Hızlı ayarları kullan veya Dirsync'ten yükseltme, yerel Active Directory'nizde için Kurumsal Yönetici hesabı olmalıdır.
* [Active Directory'de hesapları](active-directory-aadconnect-accounts-permissions.md) özel ayarlarını yükleme yolunu kullanır.

### <a name="connectivity"></a>Bağlantı
* Azure AD Connect sunucusu, hem intranet hem de internet DNS çözümlemesi gerekir. DNS sunucusu adları hem şirket içi Active Directory'niz ve Azure AD uç noktaları çözümleyebilmesi gerekir.
* Güvenlik duvarları, Intranet üzerinde sahip olduğunuz ve Azure AD Connect sunucuları ve etki alanı denetleyicilerinizin arasında bağlantı noktalarını açın ve ardından görmek gerekiyorsa [Azure AD Connect bağlantı noktaları](active-directory-aadconnect-ports.md) daha fazla bilgi için.
* Ara sunucunuzda veya güvenlik duvarı sınırlamak hangi URL'leri erişilebilir sonra URL'leri belirtilmiştir [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) açılması gerekir.
  * Microsoft Cloud Almanya'da veya Microsoft Azure kamu Bulutu kullanıyorsanız, daha sonra bkz [Azure AD Connect eşitleme hizmeti örnekleri konuları](active-directory-aadconnect-instances.md) URL'ler için.
* Azure AD Connect'i (sürüm 1.1.614.0 ve sonrası) varsayılan olarak TLS 1.2 eşitleme altyapısı ve Azure AD arasındaki iletişimi şifrelemek için kullanır. TLS 1.2 temel alınan işletim sisteminde kullanılabilir durumda değilse, Azure AD Connect artımlı olarak geri eski protokollere (TLS 1.1 ve TLS 1.0) döner. Örneğin, Windows Server 2008, TLS 1.1 veya TLS 1.2 desteklemediği için Azure AD Connect'in, Windows Server 2008'de TLS 1.0 kullanır.
* Sürüm 1.1.614.0 önce Azure AD Connect varsayılan olarak TLS 1.0 eşitleme altyapısı ve Azure AD arasındaki iletişimi şifrelemek için kullanır. TLS 1.2 değiştirmek için adımları izleyin. [Azure AD Connect için "etkinleştir" TLS 1.2](#enable-tls-12-for-azure-ad-connect).
* İnternet'e, aşağıdaki ayarı bağlamak için bir giden bağlantı proxy'si kullanıyorsanız **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** Yükleme Sihirbazı'nı ve Azure AD için dosya eklenmelidir İnternet'e ve Azure AD connect için eşitleme bağlanın. Bu metin, dosyanın sonunda girilmelidir. Bu kodda, &lt;PROXYADRESS&gt; gerçek Ara sunucu IP adresi veya ana bilgisayar adını temsil eder.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Ara sunucunuz kimlik doğrulaması gerektiriyorsa, ardından [hizmet hesabı](active-directory-aadconnect-accounts-permissions.md#adsync-service-account) etki alanında bulunan ve belirtmek için özelleştirilmiş ayarları yükleme yolu kullanmalısınız bir [özel hizmet hesabı](active-directory-aadconnect-get-started-custom.md#install-required-components). Ayrıca farklı bir machine.config değişiklik gerekir. Machine.Config'deki bu değişiklik, Yükleme Sihirbazı'nı ve eşitleme altyapısı, proxy sunucusundan kimlik doğrulama isteklerini yanıt. Hariç, tüm Yükleme Sihirbazı sayfalarındaki **yapılandırma** sayfası, imzalı kullanıcının kimlik bilgileri kullanılır. Üzerinde **yapılandırma** Yükleme Sihirbazı, komutun sonundaki sayfa çalıştırmaz [hizmet hesabı](active-directory-aadconnect-accounts-permissions.md#adsync-service-account) sizin tarafınızdan oluşturulmuş. Machine.config bölümünde aşağıdaki gibi görünmelidir.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Azure AD, Azure AD Connect dizin eşitleme işleminin bir parçası olarak Azure AD ile bir web isteği gönderdiğinde, yanıt 5 dakika kadar sürebilir. Boşta kalma zaman aşımı yapılandırma bağlantı sağlamak proxy sunucuları için yaygın bir durumdur. Lütfen, yapılandırmayı, en az 6 dakika veya daha fazlası ayarlandığından emin olun.

Daha fazla bilgi için MSDN görecekleri [proxy öğesi varsayılan](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Bağlantı sorunlarınız olduğunda, daha fazla bilgi için bkz. [bağlantı sorunlarını giderme](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Diğer
* İsteğe bağlı: eşitleme doğrulamak için bir test kullanıcı hesabı.

## <a name="component-prerequisites"></a>Bileşen önkoşulları
### <a name="powershell-and-net-framework"></a>PowerShell ve .net Framework
Azure AD Connect, Microsoft PowerShell ve .NET Framework 4.5.1 bağlıdır. Bu sürümü veya sonraki bir sürümü sunucuda yüklü ihtiyacınız var. Windows Server sürümüne bağlı olarak, aşağıdakileri yapın:

* Windows Server 2012R2
  * Microsoft PowerShell varsayılan olarak yüklenir. Eylem gerekmiyor.
  * .NET framework 4.5.1 ve sonraki sürümleri, Windows Update aracılığıyla sunulur. Denetim Masası'nda Windows Server için en son güncelleştirmeleri yüklediğinizden emin olun.
* Windows Server 2008R2 ve Windows Server 2012
  * Microsoft PowerShell'in en son sürümünü kullanılabilir **Windows Management Framework 4.0**üzerinden [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 ve sonraki sürümlerde kullanılabilir [Microsoft Download Center](http://www.microsoft.com/downloads).
* Windows Server 2008
  * PowerShell'in desteklenen son sürümü kullanılabilir **Windows Management Framework 3.0**üzerinden [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 ve sonraki sürümlerde kullanılabilir [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Azure AD Connect için TLS 1.2 etkinleştir
Sürüm 1.1.614.0 önce Azure AD Connect varsayılan olarak TLS 1.0 eşitleme altyapısı sunucusu ve Azure AD arasındaki iletişimi şifrelemek için kullanır. .Net uygulamalarını sunucu üzerinde varsayılan olarak TLS 1.2 kullanmak için yapılandırarak bunu değiştirebilirsiniz. TLS 1.2 hakkında daha fazla bilgi bulunabilir [Microsoft Güvenlik Danışma 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. Windows Server 2008'de TLS 1.2 etkinleştirilemez. Windows Server 2008R2 gerekir veya üzeri. İşletim sisteminiz için .net 4.5.1 düzeltme Bkz emin [Microsoft Güvenlik Danışma 2960358](https://technet.microsoft.com/security/advisory/2960358). Bu düzeltme veya sonraki bir sürümü sunucuda zaten yüklü olabilir.
2. Windows Server 2008R2 kullanırsanız, TLS 1.2 etkin olduğundan emin olun. Sunucu Windows Server 2012 ve sonraki sürümlerinde, TLS 1.2 zaten etkinleştirilmiş olmalıdır.
   ```
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   ```
3. Tüm işletim sistemleri için bu kayıt defteri anahtarını ayarlayın ve sunucuyu yeniden başlatın.
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
   "SchUseStrongCrypto"=dword:00000001
   ```
4. Ayrıca eşitleme altyapısı sunucusu ve uzak bir SQL Server arasında TLS 1.2 etkinleştirmek istediğiniz sonra emin gerekli sürümler için yüklü olan [Microsoft SQL Server için TLS 1.2 desteği](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Federasyon yükleme ve yapılandırma için Önkoşullar
### <a name="windows-remote-management"></a>Windows Uzaktan Yönetimi
Active Directory Federasyon Hizmetleri veya Web uygulama proxy'si dağıtmak için Azure AD Connect kullanarak, bu gereksinimleri kontrol edin:

* Hedef sunucunun etki alanına katılmış ise, ardından Windows Uzaktan yönetilen etkin olduğundan emin olun
  * Yükseltilmiş bir PSH komut penceresinde komutunu kullanın. `Enable-PSRemoting –force`
* Hedef sunucu ise WAP makine bir etki alanına katılmış, sonra birkaç ek gereksinimleri vardır.
  * Hedef makinede (WAP makine):
    * Winrm emin olun (Windows Uzaktan Yönetimi / WS-Management) hizmeti Hizmetler ek bileşenini çalışıyor
    * Yükseltilmiş bir PSH komut penceresinde komutunu kullanın. `Enable-PSRemoting –force`
  * (Hedef makine etki alanına katılmış veya güvenilmeyen etki alanı ise) Sihirbazı çalıştığı makinede:
    * Yükseltilmiş bir PSH komut penceresinde komut kullanın `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * Sunucu Yöneticisi'nde:
      * DMZ WAP ana makine havuzuna ekleyin (Sunucu Yöneticisi -> Yönet -> sunucuları Ekle...DNS sekmesini kullanın)
      * Sunucu Yöneticisi'ni tüm sunucuları sekmesi: WAP sunucuya sağ tıklayın ve Yönet as..., seçin, WAP makine için yerel (etki alanı değil) kimlik bilgilerini girin
      * Sunucu Yöneticisi tüm sunucuları sekmesinde uzak PSH bağlantıyı doğrulamak için: WAP sunucusuna sağ tıklayın ve Windows PowerShell'ı seçin. Uzak PowerShell oturumları kurulabilir emin olmak için Uzak PSH oturum açmanız gerekir.

### <a name="ssl-certificate-requirements"></a>SSL sertifikası gereksinimleri
* AD FS grubunuzun tüm düğümleri ve tüm Web uygulaması Ara sunucusu arasında aynı SSL sertifikası kullanmak üzere kesinlikle önerilir.
* Sertifika x X509 olmalıdır sertifika.
* Bir test laboratuvar ortamında Federasyon sunucularında otomatik olarak imzalanan bir sertifika kullanabilirsiniz. Ancak, bir üretim ortamı için genel bir CA'dan sertifika edinmenizi öneririz.
  * Genel olarak güvenilir olmayan bir sertifika kullanıyorsanız, her Web uygulaması Ara sunucusuna yüklenen sertifikanın yerel sunucuda ve tüm Federasyon sunucularında güvenilir olduğundan emin olun
* Sertifika kimlik Federasyon Hizmeti adı (örneğin, sts.contoso.com) eşleşmelidir.
  * Kimlik ya da bir konu alternatif adı (SAN) türü dNSName uzantısıdır veya SAN girdi yoksa, ortak ad olarak belirtilen konu adı.  
  * Bunlardan biri Federasyon hizmet adı ile eşleşen sağlanan sertifikanın, birden çok SAN girişi bulunabilir.
  * Çalışma alanına katılma kullanmayı planlıyorsanız, ek bir SAN değeriyle gereklidir **enterpriseregistration.** Kuruluşunuz, örneğin, kullanıcı asıl adı (UPN) sonekini tarafından izlenen **enterpriseregistration.contoso.com**.
* CryptoAPI İleri nesil (CNG) anahtarları ve anahtar depolama sağlayıcıları temel alarak sertifikaları desteklenmez. Başka bir deyişle, (şifreleme hizmeti sağlayıcısı) bir CSP ve KSP değil (anahtar depolama sağlayıcısı) dayalı bir sertifika kullanmanız gerekir.
* Joker kart sertifikaları desteklenir.

### <a name="name-resolution-for-federation-servers"></a>Federasyon sunucuları için ad çözümlemesi
* Hem (iç DNS sunucunuzun) intranet ve extranet (Genel DNS, etki alanı kayıt şirketi aracılığıyla) için AD FS Federasyon Hizmeti adının (örneğin, sts.contoso.com) için DNS kayıtlarını ayarlayın. İntranet DNS kaydı için A kullandığınızdan emin olun ve CNAME kayıtlarını değil. Bu, etki alanına katılmış makinenizde düzgün çalışması, windows kimlik doğrulaması için gereklidir.
* Birden fazla AD FS sunucusu veya Web uygulaması Ara sunucusu dağıtıyorsanız, load balancer'ınız yapılandırılmış ve AD FS Federasyon Hizmeti adının (örneğin, sts.contoso.com) için DNS kayıtlarını yük dengeleyiciye gösterdiğinden emin olun.
* Internet Explorer kullanarak, intranet tarayıcı uygulamaları için iş windows tümleşik kimlik doğrulaması için AD FS Federasyon Hizmeti adının (örneğin, sts.contoso.com) intranet bölgesine IE'de eklendiğinden emin olun. Bu Grup İlkesi aracılığıyla denetlenebilir ve tüm etki alanına katılmış bilgisayarlara dağıtılır.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect'i bileşenleri destekleme
Azure AD Connect, Azure AD Connect'in yüklü olduğu sunucuya yüklenen bileşenlerin bir listesi verilmiştir. Bu liste için temel bir Express yüklemesidir. Eşitleme hizmetlerini yükleme sayfasında farklı bir SQL Server'ı kullanmayı seçerseniz, SQL Express LocalDB yerel olarak yüklenmedi.

* Azure AD Connect Health
* Microsoft Online Services oturum açma Yardımcısı (ancak hiçbir bağımlılığı yüklü) BT uzmanları için
* Microsoft SQL Server 2012 komut satırı yardımcı programları
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 yerel istemcisi
* Microsoft Visual C++ 2013 yeniden dağıtımı paketi

## <a name="hardware-requirements-for-azure-ad-connect"></a>Azure AD Connect için donanım gereksinimleri
Azure AD Connect eşitleme bilgisayarın en düşük gereksinimleri aşağıdaki tabloda gösterilmiştir.

| Active Directory içindeki nesneleri sayısı | CPU | Bellek | Sabit sürücü boyutu |
| --- | --- | --- | --- |
| 10. 000'den az |1,6 GHz |4 GB |70 GB |
| 10,000–50,000 |1,6 GHz |4 GB |70 GB |
| 50,000–100,000 |1,6 GHz |16 GB |100 GB |
| SQL Server'ın tam sürümünü 100.000 veya daha fazla nesne için gereklidir | | | |
| 100,000–300,000 |1,6 GHz |32 GB |300 GB |
| 300,000–600,000 |1,6 GHz |32 GB |450 GB |
| Birden fazla 600,000 |1,6 GHz |32 GB |500 GB |

AD FS ve Web uygulama sunucuları çalıştıran bilgisayarlar için en düşük gereksinimler aşağıda verilmiştir:

* CPU: Çekirdek çift 1,6 GHz veya üzeri
* Bellek: 2 GB veya üzeri
* Azure VM: A2 yapılandırması veya üzeri

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
