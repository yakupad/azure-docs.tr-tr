---
title: 'Azure AD Connect: Hesaplar ve izinler | Microsoft Docs'
description: Bu konuda kullanılan ve oluşturulan hesaplar ve gereken izinler açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: cfb6fb512ecb7d57cf411a31b2e04726bfc4b743
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263411"
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Hesaplar ve izinler

## <a name="accounts-used-for-azure-ad-connect"></a>Azure AD Connect için kullanılan hesaplar

![](media/active-directory-aadconnect-accounts-permissions/account5.png)

Azure AD Connect, şirket içi veya Windows Server Active Directory Azure Active Directory bilgileriyle eşitlemek için 3 hesaplarını kullanır.  Bu hesaplar şunlardır:

- **AD DS bağlayıcı hesabı**: bilgi için Windows Server Active Directory okuma/yazma için kullanılır

- **Ad eşitleme hizmeti hesabı**: eşitleme hizmetini çalıştırmak ve SQL veritabanına erişmek için kullanılan

- **Azure AD Bağlayıcısı hesabı**: Azure AD'ye yazmak için kullanılır

Azure AD Connect çalıştırmak için kullanılan üç bu hesabı ek olarak, Azure AD Connect'i yüklemek için aşağıdaki ek hesaplar gerekir.  Bunlar:

- **AD DS kuruluş yöneticisi hesabı**: Azure AD Connect'i yüklemek için kullanılan
- **Azure AD genel yönetici hesabı**: Azure AD Bağlayıcısı hesabı oluşturup Azure AD'ye yapılandırmak için kullanılır.

- **(İsteğe bağlı) SQL SA hesabı**: SQL Server'ın tam sürümünü kullanırken ad eşitleme veritabanını oluşturmak için kullanılır.  Bu SQL Server yerel veya uzak Azure AD Connect yüklemesi için olabilir.  Bu hesap aynı kuruluş yöneticisi olarak hesap olabilir.  Veritabanı sağlama, artık SQL Yöneticisi tarafından bant dışında yapılabilmesi ve ardından veritabanı sahibi haklarıyla Azure AD Connect Yöneticisi tarafından yüklenir.  Hakkında bilgi için bkz bu [SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme]()

## <a name="installing-azure-ad-connect"></a>Azure AD Connect yükleniyor
Azure AD Connect Yükleme Sihirbazı'nı iki farklı yol sunar:

* Hızlı ayarlar sihirbazın daha fazla ayrıcalıkları gerektirir.  Bu, böylelikle kullanıcılar oluşturabilir veya izinleri yapılandırmak gerek kalmadan yapılandırmanızı kolayca ayarlayabilirsiniz.
* Özel ayarlar, sihirbazın daha fazla seçenek ve seçenekleri sunar. Ancak, sizin doğru izinlere sahip olmak gereken bazı durumlar vardır.



## <a name="express-settings-installation"></a>Hızlı ayarları yükleme
Express ayarlarında Yükleme Sihirbazı aşağıdaki amaçlarla ister:

  - AD DS kuruluş yöneticisi kimlik bilgileri
  - Azure AD genel yönetici kimlik bilgileri

### <a name="ad-ds-enterprise-admin-credentials"></a>AD DS kuruluş yöneticisi kimlik bilgileri
AD DS kuruluş yöneticisi hesabı, şirket içi Active Directory'nizde yapılandırmak için kullanılır. Bu kimlik bilgileri yalnızca yükleme sırasında kullanılır ve yükleme tamamlandıktan sonra kullanılmaz. Kuruluş yöneticisi etki alanı yöneticisi Active Directory izinlerini tüm alanlarında ayarlanabilir emin olmanız gerekir.

Dirsync'ten yükseltme yapıyorsanız, AD DS Enterprise Admins kimlik bilgileri DirSync tarafından kullanılan hesabın parolasını sıfırlamak için kullanılır. Ayrıca Azure AD genel yönetici kimlik bilgileri gerekir.

### <a name="azure-ad-global-admin-credentials"></a>Azure AD genel yönetici kimlik bilgileri
Bu kimlik bilgileri yalnızca yükleme sırasında kullanılır ve yükleme tamamlandıktan sonra kullanılmaz. Oluşturmak için kullanılan [Azure AD Bağlayıcısı hesabı](#azure-ad-service-account) değişiklikler Azure AD'ye eşitlemek için kullanılır. Hesabın Azure AD'de eşitleme ayrıca bir özellik sağlar.

### <a name="ad-ds-connector-account-required-permissions-for-express-settings"></a>AD DS bağlayıcı hesabı hızlı ayarları için gereken izinler
[AD DS bağlayıcı hesabı](#active-directory-account) Windows Server AD yazma ve okuma için oluşturulan ve hızlı ayarlar ile oluşturduğunuzda aşağıdaki izinlere sahiptir:

| İzin | İçin kullanılan |
| --- | --- |
| <li>Dizin Değişikliklerini Çoğaltma</li><li>Çoğaltma Directory yapılan tüm değişiklikler |Parola Karması eşitleme |
| Okuma/yazma tüm özellikleri kullanıcı |İçeri aktarma ve Exchange karma |
| Okuma/yazma tüm özellikleri iNetOrgPerson |İçeri aktarma ve Exchange karma |
| Tüm özellikleri Grup okuma/yazma |İçeri aktarma ve Exchange karma |
| Tüm özellikleri okuma/yazma başvurun |İçeri aktarma ve Exchange karma |
| Parola sıfırlama |Parola geri yazma özelliğini etkinleştirmek için hazırlama |

### <a name="express-installation-wizard-summary"></a>Hızlı Yükleme Sihirbazı Özet

![Hızlı yükleme](media/active-directory-aadconnect-get-started-express/express.png)

Express installlation sihirbaz sayfalarında toplandı, kimlik bilgilerini bir özeti aşağıda verilmiştir ve ne için kullanılır.

| Sihirbaz sayfası | Toplanan kimlik bilgileri | Gerekli izinler | İçin kullanılan |
| --- | --- | --- | --- |
| Yok |Yükleme Sihirbazı çalıştıran kullanıcı |Yerel sunucunun Yöneticisi |<li>Oluşturur [ADSync yapılandırmaya hesabı](#azure-ad-connect-sync-service-account) eşitleme hizmetini çalıştırmak için kullanılan hesap. |
| Azure AD'ye Bağlanma |Azure AD directory kimlik bilgileri |Azure AD'de genel yönetici rolü |<li>Azure AD dizini eşitleme etkinleştiriliyor.</li>  <li>Oluşturulmasını [Azure AD Bağlayıcısı hesabı](#azure-ad-service-account) Azure AD'de devam eden eşitleme işlemleri için kullanılır.</li> |
| AD DS'ye Bağlanma |Şirket içi Active Directory kimlik bilgileri |Active Directory'de Enterprise Admins (EA) grubunun üyesi |<li>Oluşturur [AD DS bağlayıcı hesabı](#active-directory-account) Active Directory ve bu izinleri verir. Bu hesap oluşturulan okumak ve dizin eşitleme sırasında yazmak için kullanılır.</li> |


## <a name="custom-installation-settings"></a>Özel yükleme ayarları

Özel ayarlar yüklemesiyle sihirbaz daha fazla seçenek ve seçenekleri sunar. 

### <a name="custom-installation-wizard-summary"></a>Özel bir Yükleme Sihirbazı Özeti

Özel installlation sihirbaz sayfalarında toplandı, kimlik bilgilerini bir özeti aşağıda verilmiştir ve ne için kullanılır.

![Hızlı yükleme](media/active-directory-aadconnect-accounts-permissions/customize.png)

| Sihirbaz sayfası | Toplanan kimlik bilgileri | Gerekli izinler | İçin kullanılan |
| --- | --- | --- | --- |
| Yok |Yükleme Sihirbazı çalıştıran kullanıcı |<li>Yerel sunucunun Yöneticisi</li><li>Tam SQL Server kullanıyorsanız, kullanıcının SQL Sistem Yöneticisi (SA) olması gerekir</li> |Varsayılan olarak kullanılan yerel hesabı oluşturan [eşitleme altyapısı hizmet hesabı](#azure-ad-connect-sync-service-account). Hesabı yalnızca belirli bir hesabın yönetici belirtmiyor oluşturulur. |
| Eşitleme Hizmetleri, hizmet hesabı seçeneğini yükleyin |AD veya yerel kullanıcı hesabı kimlik bilgileri |Yükleme Sihirbazı tarafından verilen kullanıcı izinleri |Yönetici hesabınız belirtiyorsa, bu hesap için eşitleme hizmeti hizmet hesabı olarak kullanılır. |
| Azure AD'ye Bağlanma |Azure AD directory kimlik bilgileri |Azure AD'de genel yönetici rolü |<li>Azure AD dizini eşitleme etkinleştiriliyor.</li>  <li>Oluşturulmasını [Azure AD Bağlayıcısı hesabı](#azure-ad-service-account) Azure AD'de devam eden eşitleme işlemleri için kullanılır.</li> |
| Dizinlerinizi bağlama |Şirket içi Active Directory kimlik bilgileri Azure AD'ye bağlı her bir orman için |Hangi özellikleri etkinleştirmek ve bulunabilir izinler bağlıdır [AD DS bağlayıcı hesabı oluşturma](#create-the-ad-dso-connector-account) |Bu hesap, okumak ve dizin eşitleme sırasında yazmak için kullanılır. |
| AD FS Sunucuları |Listedeki her sunucu için Sihirbazı çalıştıran kullanıcı oturum açma kimlik bilgilerini bağlanmak yetersiz olduğunda sihirbaz kimlik bilgilerini toplar. |Etki alanı yöneticisi |Yükleme ve AD FS sunucusu rolü yapılandırması. |
| Web uygulaması Ara sunucusu |Listedeki her sunucu için Sihirbazı çalıştıran kullanıcı oturum açma kimlik bilgilerini bağlanmak yetersiz olduğunda sihirbaz kimlik bilgilerini toplar. |Hedef makinede yerel yönetici |Yükleme ve WAP sunucusu rolü yapılandırması. |
| Ara sunucu güveni kimlik bilgileri |Federasyon Hizmeti'ne güvenen kimlik bilgilerini (proxy FS güven sertifikadan kaydetmek için kullandığı kimlik bilgileri |AD FS sunucusunun yerel yönetici olan etki alanı hesabı |İlk kayıt FS WAP güven sertifikası. |
| "Bir etki alanı kullanıcı hesabı seçeneğini kullan" AD FS hizmet hesabı sayfası |AD kullanıcı hesabı kimlik bilgileri |Etki alanı kullanıcısı |Kimlik bilgileri sağlanan AD kullanıcı hesabı, AD FS hizmeti oturum açma hesabı olarak kullanılır. |

### <a name="create-the-ad-ds-connector-account"></a>AD DS bağlayıcı hesabı oluşturma
Belirttiğiniz hesabın **dizinlerinizi bağlama** sayfasına yüklemeden önce Active Directory'de mevcut olmalıdır.  Azure AD Connect sürüm 1.1.524.0 ve daha sonra Azure AD Connect Sihirbazı'nı oluşturma izin verme seçeneğini **AD DS bağlayıcı hesabı** Active Directory'ye bağlanmak için kullanılır.  

Ayrıca, gerekli izinler de olmalıdır. Yükleme Sihirbazı, izinleri ve herhangi bir sorun yalnızca eşitleme sırasında bulunan doğrulamaz.

İhtiyaç duyduğunuz izinleri bağlıdır isteğe bağlı özellikler sağlar. Birden çok etki alanı varsa, ormandaki tüm etki alanları için izinleri verilmelidir. Bunlardan birine etkinleştirmezseniz özellikleri, varsayılan **etki alanı kullanıcısı** izinler yeterli değil.

| Özellik | İzinler |
| --- | --- |
| msDS-Consistencyguid'i özelliği |Belirtilmiştir msDS-Consistencyguid'i özniteliği için yazma izinleri [tasarım kavramları - msDS-Consistencyguid'i sourceAnchor olarak kullanma](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). | 
| Parola Karması eşitleme |<li>Dizin Değişikliklerini Çoğaltma</li>  <li>Çoğaltma Directory yapılan tüm değişiklikler |
| Exchange karma dağıtımı |Özniteliklere açıklandığı yazma izinleri [Exchange karma geri yazma](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) kullanıcıları, grupları ve kişileri için. |
| Exchange posta ortak klasör |İçinde belirtilen öznitelikler için Okuma izinleri [Exchange posta ortak klasör](active-directory-aadconnectsync-attributes-synchronized.md#exchange-mail-public-folder) ortak klasörleri için. | 
| Parola geri yazma |Özniteliklere açıklandığı yazma izinleri [parola yönetimine Başlarken](../authentication/howto-sspr-writeback.md) kullanıcılar için. |
| Cihaz geri yazma |Bölümünde anlatıldığı gibi bir PowerShell Betiği ile verilen izinler [cihaz geri yazmayı](active-directory-aadconnect-feature-device-writeback.md). |
| Grup geri yazma |Okuma, oluşturma, güncelleştirme ve silme grubu eşitlenmesi için nesneleri **Office 365 grupları**.  Daha fazla bilgi için [grup geri yazma](active-directory-aadconnect-feature-preview.md#group-writeback).|

## <a name="upgrade"></a>Yükseltme
Bir Azure AD Connect sürümünden yeni sürüme yükselttiğinizde, aşağıdaki izinlere ihtiyacınız vardır:

>[!IMPORTANT]
>Derleme 1.1.484 ile başlayarak, Azure AD Connect SQL veritabanını yükseltmek için sysadmin izinleri gerektiren bir regresyon hata kullanıma sunuldu.  Bu hata, yapı 1.1.647 düzeltilir.  Bu derleme için yükseltme yapıyorsanız, sysadmin izinleri gerekir.  Dbo izinleri yeterli değil.  Sysadmin izinlerine gerek kalmadan Azure AD Connect'e yükseltmenin denerseniz yükseltme başarısız olur ve Azure AD Connect artık doğru bir şekilde daha sonra çalışmaz.  Microsoft, bunun farkında ve bu sorunu gidermek için çalışmaktadır.


| Sorumlu | Gerekli izinler | İçin kullanılan |
| --- | --- | --- |
| Yükleme Sihirbazı çalıştıran kullanıcı |Yerel sunucunun Yöneticisi |İkili dosyaları güncelleştirin. |
| Yükleme Sihirbazı çalıştıran kullanıcı |ADSyncAdmins üyesi |Eşitleme kuralları ve diğer yapılandırma değişikliklerini yapın. |
| Yükleme Sihirbazı çalıştıran kullanıcı |Tam SQL server kullanıyorsanız: DBO (veya benzeri) eşitleme altyapısı veritabanının |Yeni sütunları içeren tablolar güncelleştirme gibi veritabanı düzeyi değişiklikleri yapın. |

## <a name="more-about-the-created-accounts"></a>Oluşturulan hesaplar hakkında daha fazla bilgi
### <a name="ad-ds-connector-account"></a>AD DS bağlayıcı hesabı
Hızlı ayarları kullanırsanız, eşitleme için kullanılan Active Directory'de bir hesap oluşturulur. Oluşturulan hesap orman kök etki alanı Kullanıcılar kapsayıcısı içinde bulunur ve adı ön ekine sahip **MSOL_**. Hesabın süresinin sona kadar karmaşık bir parola oluşturulur. Etki alanınızdaki bir parola ilkesi varsa, uzun olduğundan emin olun ve karmaşık parolalar bu hesap için izin verilecek.

![AD hesabı](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Özel ayarları kullanırsanız, ardından yüklemeye başlamadan önce bir hesap oluşturmak için sorumlu olursunuz.  Bkz: [AD DS bağlayıcı hesabı oluşturma](#create-the-ad-dso-connector-account).

### <a name="adsync-service-account"></a>Ad eşitleme hizmeti hesabı
Eşitleme hizmeti farklı hesaplarla çalıştırabilirsiniz. Altında çalışabilmesi için bir **sanal hizmet hesabı** (VSA) bir **Grup yönetilen hizmet hesabı** (gMSA/sMSA) veya normal bir kullanıcı hesabı. Desteklenen seçenekler 2017 ile değiştirildi. yeni bir yükleme yaparken Connect Nisan yayın. Azure AD Connect'in önceki bir sürümden yükseltirseniz, bu ek seçenekleri kullanılamaz.

| Hesap türü | Yükleme seçeneği | Açıklama |
| --- | --- | --- |
| [Sanal hizmet hesabı](#virtual-service-account) | Hızlı ve özel, Nisan 2017 ve üzeri | Bir etki alanı denetleyicisi yüklemelerinde hariç tüm express yüklemeleri için kullanılan seçenek budur. Başka bir seçeneği kullanılmadığı sürece, özel, varsayılan seçenektir. |
| [Grup yönetilen hizmet hesabı](#group-managed-service-account) | Özel, 2017 Nisan ve üzeri | Ardından bir uzak SQL server kullanıyorsanız, bir grup yönetilen hizmet hesabı kullanılması önerilir. |
| [Kullanıcı hesabı](#user-account) | Hızlı ve özel, Nisan 2017 ve üzeri | Bir kullanıcı hesabı ile AAD_ öneki yalnızca Windows Server 2008 ve bir etki alanı denetleyicisine yüklendiğinde yüklendiğinde yükleme sırasında oluşturulur. |
| [Kullanıcı hesabı](#user-account) | Hızlı ve özel, Mart 2017 ve önceki sürümleri | Yükleme sırasında yerel bir hesap ile AAD_ ön eki oluşturulur. Özel yükleme kullanırken, başka bir hesap belirtilebilir. |

Connect 2017 bir derleme ile Mart kullandığınız ya da Windows Güvenlik nedenleriyle şifreleme anahtarları yok eder. bu yana daha önce daha sonra hizmet hesabı parolasını sıfırlamalısınız değil ise. Azure AD Connect'i yeniden yüklemeden başka bir hesap için hesaba geçemezsiniz. Nisan 2017'den bir yapıya yükseltme veya daha sonra ardından desteklenen, ancak bir hizmet hesabı parolasını değiştirmek için kullanılan hesaba geçemezsiniz.

> [!Important]
> İlk yüklemede yalnızca hizmet hesabı ayarlayabilirsiniz. Yükleme tamamlandıktan sonra hizmet hesabını değiştirmek için desteklenmiyor.

Bu, eşitleme hizmeti hesabı için önerilen ve desteklenen seçenekler varsayılan bir tablodur.

Gösterge:

- **Kalın** varsayılan seçenek belirtir ve önerilen seçenek çoğu durumda.
- *İtalik* varsayılan seçenek olmadığı durumlarda, önerilen seçenek gösterir.
- 2008 - Windows Server 2008'de yüklendiğinde varsayılan seçeneği
- Non-kalın - desteklenen seçeneği
- Yerel hesap - yerel kullanıcı hesabı sunucuda
- Etki alanı hesabı - etki alanı kullanıcı hesabı
- smsa'yı - [tek başına yönetilen hizmet hesabı](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [Grup yönetilen hizmet hesabı](https://technet.microsoft.com/library/hh831782.aspx)

| | Yerel veritabanı</br>Express | LocalDB/LocalSQL</br>Özel | Uzak SQL</br>Özel |
| --- | --- | --- | --- |
| **tek başına/çalışma grubu makinesine** | Desteklenmiyor | **VSA**</br>Yerel hesap (2008)</br>Yerel hesap |  Desteklenmiyor |
| **Makine etki alanına katılmış** | **VSA**</br>Yerel hesap (2008) | **VSA**</br>Yerel hesap (2008)</br>Yerel hesap</br>Etki alanı hesabı</br>smsa'yı, gMSA | **gMSA**</br>Etki alanı hesabı |
| **Etki alanı denetleyicisi** | **Etki alanı hesabı** | *gMSA*</br>**Etki alanı hesabı**</br>smsa'yı| *gMSA*</br>**Etki alanı hesabı**|

#### <a name="virtual-service-account"></a>Sanal hizmet hesabı
Bir sanal hizmet hesabı, parola yok ve Windows tarafından yönetilen hesap özel türüdür.

![VSA](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

VSA, eşitleme altyapısı ve SQL aynı sunucu üzerinde olduğu senaryolar ile kullanılmak üzere tasarlanmıştır. Uzak SQL kullanın ve ardından kullanmanızı öneririz, bir [Grup yönetilen hizmet hesabı](#managed-service-account) yerine.

Bu özellik, Windows Server 2008 R2 gerektirir veya üzeri. Windows Server 2008'de Azure AD Connect'i yükleme sonra yüklemeyi yeniden kullanarak döner bir [kullanıcı hesabı](#user-account) yerine.

#### <a name="group-managed-service-account"></a>Grup yönetilen hizmet hesabı
Uzak bir SQL server'ı kullanın ve ardından için kullanmanızı öneririz, bir **Grup yönetilen hizmet hesabı**. Active Directory Grup yönetilen hizmet hesabı için hazırlama hakkında daha fazla bilgi için bkz. [Grup yönetilen hizmet hesaplarına genel bakış](https://technet.microsoft.com/library/hh831782.aspx).

Bu seçeneği kullanmak üzere [gerekli bileşenleri yükleme](active-directory-aadconnect-get-started-custom.md#install-required-components) sayfasında **mevcut bir hizmet hesabını kullanma**seçip **yönetilen hizmet hesabı**.  
![VSA](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Kullanmak için ayrıca desteklenen bir [tek başına yönetilen hizmet hesabı](https://technet.microsoft.com/library/dd548356.aspx). Ancak, bunlar yerel makinede kullanılabilir ve varsayılan sanal hizmet hesabı üzerinde kullanılacakları bir yararı yoktur.

Bu özellik, Windows Server 2012 veya sonraki sürümünü gerektirir. Daha eski bir işletim sistemi ve uzak SQL kullanmak ihtiyacınız varsa kullanmalısınız bir [kullanıcı hesabı](#user-account).

#### <a name="user-account"></a>Kullanıcı hesabı
(Özel ayarlarında kullanılacak hesabı belirtmediğiniz sürece), yerel hizmet hesabı Yükleme Sihirbazı tarafından oluşturulur. Hesap önekli **AAD_** ve gerçek eşitleme hizmeti olarak çalıştırmak için kullanılır. Azure AD Connect, etki alanı denetleyicisine yüklerseniz, hesap etki alanında oluşturulur. **AAD_** hizmet hesabı, etki alanında bulunmalıdır:
   - SQL server çalıştıran uzak bir sunucu kullanın
   - kimlik doğrulaması gerektiren bir proxy kullanın

![Eşitleme hizmeti hesabı](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Hesabın süresinin sona kadar karmaşık bir parola oluşturulur.

Bu hesap, diğer hesaplar için parolaları güvenli bir şekilde depolamak için kullanılır. Bu hesapları parolaların veritabanında şifrelenmiş olarak depolanır. Şifreleme anahtarları için özel anahtarlar, Şifreleme Hizmetleri gizli anahtar şifreleme Windows Data Protection API (DPAPI) kullanılarak korunur.

Tam SQL Server kullanıyorsanız hizmet hesabının eşitleme altyapısı için oluşturulan veritabanı DBO'su olduğundan. Hizmet, diğer herhangi bir izinle beklendiği gibi çalışmayacaktır. Bir SQL oturum açma da oluşturulur.

Hesap, ayrıca dosyaları, kayıt defteri anahtarlarını ve diğer nesneleri eşitleme altyapısında ilgili izinler verilir.

### <a name="azure-ad-connector-account"></a>Azure AD Bağlayıcısı hesabı
Hesabınız Azure AD'de eşitleme hizmetin kullanımı için oluşturulur. Bu hesap, görünen ada göre tanımlanabilir.

![AD hesabı](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount2.png)

Hesap kullanılır sunucunun adını, kullanıcı adı ikinci kısmında tanımlanabilir. Resimde, sunucu adı DC1 ' dir. Sunucuları hazırlama varsa, her sunucuyla kendi hesabı vardır.

Hesabın süresinin sona kadar karmaşık bir parola oluşturulur. Özel bir rol verilir **dizin eşitlemesi hesapları** dizin eşitleme görevleri gerçekleştirmek için yalnızca izinlere sahiptir. Bu özel yerleşik rol dışında Azure AD Connect Sihirbazı verilemez. Azure portalında bu rolü hesapla gösterir **kullanıcı**.

Azure AD'de eşitleme hizmeti hesabı 20 bir sınırlama yoktur. Azure AD'niz içinde var olan Azure AD hizmet hesabını listesini almak için aşağıdaki Azure AD PowerShell cmdlet'ini çalıştırın: `Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

Kullanılmayan Azure AD'ye kaldırmak için hizmet hesapları, aşağıdaki Azure AD PowerShell cmdlet'ini çalıştırın: `Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="related-documentation"></a>İlgili belgeler
Varsa, ilgili belgeleri okuyun değil [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory-aadconnect.md), aşağıdaki tabloda ilgili konulara bağlantılar sağlar.

|Konu |Bağlantı|  
| --- | --- |
|Azure AD Connect'i indirme | [Azure AD Connect’i indirme](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Hızlı ayarları kullanarak yükleme | [Azure AD Connect’i hızlı yükleme](./active-directory-aadconnect-get-started-express.md)|
|Özelleştirilmiş ayarları kullanarak yükleme | [Azure AD Connect özel yüklemesi](./active-directory-aadconnect-get-started-custom.md)|
|DirSync'ten yükseltme | [Azure AD eşitleme aracından (DirSync) yükseltme](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Yükleme işleminden sonra | [Yüklemeyi doğrulama ve lisansları atama](active-directory-aadconnect-whats-next.md)|

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
