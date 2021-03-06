---
title: Azure MFA NPS uzantısı ile Uzak Masaüstü Ağ Geçidi tümleştirme | Microsoft Docs
description: Uzak Masaüstü Ağ Geçidi altyapınızı Azure mfa'yı Microsoft Azure için ağ ilkesi sunucusu uzantısı kullanarak tümleştirin
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: e22fedda4861e68f2318aff89bc3fe5a15cb6ede
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160112"
---
# <a name="integrate-your-remote-desktop-gateway-infrastructure-using-the-network-policy-server-nps-extension-and-azure-ad"></a>Ağ İlkesi Sunucusu (NPS) uzantısı ve Azure AD kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı tümleştirin

Bu makalede ayrıntıları Uzak Masaüstü Ağ Geçidi altyapınızı Azure multi-Factor Authentication (MFA) ile tümleştirmek için Microsoft Azure için ağ ilkesi sunucusu (NPS) uzantısını kullanarak sağlar. 

Ağ İlkesi Sunucusu (NPS) uzantısı için Azure, müşterilerin korumanıza olanak tanır. uzak kimlik denetimi içeri arama kullanıcı hizmeti (RADIUS) istemci kimlik doğrulaması Azure'ı kullanarak bulut tabanlı [multi-Factor Authentication (MFA)](multi-factor-authentication.md). Bu çözüm, kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı eklemek için iki aşamalı doğrulama sağlar.

Bu makalede, Azure için NPS uzantısı kullanarak NPS altyapı ile Azure mfa'yı tümleştirmek için adım adım yönergeler sağlar. Bu, bir Uzak Masaüstü Ağ Geçidi oturum açmaya çalışan kullanıcılar için güvenli doğrulama sağlar. 

Ağ İlkesi ve erişim Hizmetleri'ni (NPS) kuruluşlar yeteneği aşağıdakileri sağlar:
* Yönetim Merkezi konumlarını ve denetim kimin bağlanabilir belirterek ağ isteklerinin gün bağlantıların ne zaman izin verilir, bağlantıları süresini ve istemcilerin bağlanın ve benzeri güvenlik düzeyini tanımlar. Bu ilkelerin her VPN ya da Uzak Masaüstü (RD) Ağ Geçidi sunucusuna belirtmek yerine, bu ilkeler, bir kez merkezi bir konumda belirtilebilir. RADIUS protokolü, merkezi kimlik doğrulaması, yetkilendirme ve hesap işlemleri (AAA) sağlar. 
* Kurmak ve cihazları Kısıtlanmamış veya kısıtlanmış ağ kaynaklarına erişim izni olup olmadığını belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkeleri uygular.
* Kimlik doğrulama ve yetkilendirme erişim için 802.1 zorlamak için sağladıkları x özellikli kablosuz erişim noktaları ve Ethernet anahtarları.    

Genellikle, ilkeleri kuruluşların kullanın ve VPN yönetimini merkezileştirin kolaylaştırmak için NPS (RADIUS). Ancak, çoğu kuruluş ayrıca NPS RD Masaüstü Bağlantısı Yetkilendirme İlkeleri (RD Cap'leri) yönetimini merkezden gerçekleştirin ve kolaylaştırmak için kullanır. 

Kuruluşlar, NPS güvenliğini ve yüksek düzeyde uyumluluk sağlamak için Azure MFA ile de tümleştirebilirsiniz. Bu kullanıcılar oturum açmak için Uzak Masaüstü Ağ geçidi için iki aşamalı doğrulama oluşturmanızı sağlar. Kullanıcıların erişim verilmesi için kendi denetimde kullanıcının sahip olduğu bilgilerle birlikte, kullanıcı adı/parola bileşimini sağlamanız gerekir. Bu bilgileri güvenilir ve kolayca yinelenen, bir cep telefonu numarası, telefona numarası, uygulamayı bir mobil cihaz ve benzeri gibi.

Azure için NPS uzantısı kullanıma açılmadan önce yapılandırın ve ayrı bir MFA sunucusu şirket içi ortamda içindeaçıklandığıgibikorumaktümleşikNPSveAzuremfa'yıortamlarıiçinikiaşamalıdoğrulamayıuygulamaayarlarınışahsenyapmasınıistedinizmüşterilervardı.[ Uzak Masaüstü Ağ geçidi ve Azure multi-Factor Authentication sunucusu RADIUS kullanan](howto-mfaserver-nps-rdg.md).

Azure için NPS uzantısı kullanılabilirliğini artık kuruluşların güvenli RADIUS istemci kimlik doğrulaması için şirket içi tabanlı MFA çözümünü veya bir bulut tabanlı MFA çözümünü dağıtmak için seçmenizi sağlar.

## <a name="authentication-flow"></a>Kimlik doğrulama akışı

Kullanıcıların Uzak Masaüstü Ağ Geçidi aracılığıyla ağ kaynaklarına erişim verilmesi için bir RD Bağlantı Yetkilendirme İlkesi (RD CAP) ve bir RD kaynak yetkilendirme ilkesi (RD RAP) belirtilen koşulları karşılaması gerekir. RD CAP kimin RD ağ geçitlerine bağlanmak için yetkili belirtin. Uzak Masaüstü veya kullanıcı RD Ağ geçidi üzerinden bağlanmasına izin verilen uzaktan uygulamaları gibi ağ kaynaklarına RD RAP belirtin. 

RD Ağ geçidi için RD Cap'leri bir merkezi ilke deposunu kullanmak üzere yapılandırılabilir. RD Ağ Geçidi üzerinde işlendikçe RD RAP bir merkezi ilke kullanamazsınız. Merkezi ilke deposu olarak hizmet veren başka bir NPS sunucusunun RADIUS istemcisi için RD Cap'leri merkezi ilke deposu kullanmak üzere yapılandırılmış bir RD Ağ Geçidi örneğidir.

Azure için NPS uzantısı NPS ve Uzak Masaüstü Ağ geçidi ile tümleştirildiğinde, başarılı kimlik doğrulaması akışı aşağıdaki gibidir:

1. Uzak Masaüstü Ağ Geçidi sunucusu, Uzak Masaüstü oturumu gibi bir kaynağa bağlanmak için bir Uzak Masaüstü kullanıcı kimlik doğrulama isteği alır. Bir RADIUS istemcisi işlevi gören, Uzak Masaüstü Ağ Geçidi sunucusu, RADIUS erişim isteğini ileti isteği dönüştürür ve NPS uzantısı, yüklü olduğu, RADIUS (NPS'yi) sunucusuna ileti gönderir. 
2. Kullanıcı adı ve parola birleşimini Active Directory'de doğrulanır ve kullanıcının kimliği doğrulanır.
3. NPS bağlantı isteği ve ağ ilkelerinde belirtilen tüm koşullar karşılanıyorsa (örneğin, zaman gün veya grubun üyelik kısıtlamaları), bir istek için ikincil kimlik doğrulaması ile Azure MFA NPS uzantısı tetikler. 
4. Azure MFA, Azure AD ile iletişim kurar, kullanıcının ayrıntılarını alır ve (SMS mesajı, mobil uygulama vb.) kullanıcı tarafından yapılandırılan yöntemi kullanarak ikincil kimlik doğrulaması gerçekleştirir. 
5. MFA testini başarılı olduktan sonra Azure MFA NPS uzantısı sonucu iletişim kurar.
6. Uzantının yüklü, NPS sunucusunun bir RADIUS Erişim Kabul iletisi RD CAP ilkesi için Uzak Masaüstü Ağ Geçidi sunucusuna gönderir.
7. Kullanıcı, RD Ağ Geçidi aracılığıyla istenen ağ kaynağına erişim izni verilir.

## <a name="prerequisites"></a>Önkoşullar
Bu bölümde, Uzak Masaüstü Ağ geçidi ile Azure mfa'yı tümleştirme önce gerekli önkoşulları açıklanmaktadır. Başlamadan önce aşağıdaki önkoşulların yerinde olması gerekir.  

* Uzak Masaüstü Hizmetleri (RDS) altyapısı
* Azure MFA lisans
* Windows Server yazılımı
* Ağ İlkesi ve erişim Hizmetleri'ni (NPS) rol
* Azure Active Directory, şirket içi Active Directory ile eşitlenmiş
* Azure Active Directory GUID kimliği

### <a name="remote-desktop-services-rds-infrastructure"></a>Uzak Masaüstü Hizmetleri (RDS) altyapısı
Çalışan bir Uzak Masaüstü Hizmetleri (RDS) altyapısının yerinde olması gerekir. Bunu yapmanız durumunda bu altyapı aşağıdaki Hızlı Başlangıç şablonu kullanarak Azure'da hızlıca oluşturabilirsiniz: [Uzak Masaüstü Oturum Koleksiyonu Oluştur dağıtım](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

El ile test etmek için hızlı bir şekilde şirket içi RDS altyapı oluşturmak istiyorsanız, bir dağıtmak için adımları izleyin. 
**Daha fazla bilgi edinin**: [RDS dağıtma ile Azure Hızlı Başlangıç](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) ve [temel bir RDS altyapı dağıtımı](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### <a name="azure-mfa-license"></a>Azure MFA lisans
Lisans bir Azure AD Premium, Enterprise Mobility ve Security (EMS) veya MFA abonelik kullanılabilir Azure MFA için gereklidir. Tüketim tabanlı lisans gibi kullanıcı başına veya kimlik doğrulaması lisans başına Azure mfa NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için [Azure multi-Factor Authentication'ı alma](concept-mfa-licensing.md). Sınama amacıyla bir deneme aboneliğini kullanabilirsiniz. 

### <a name="windows-server-software"></a>Windows Server yazılımı
NPS uzantısı, Windows Server 2008 R2 SP1 gerektirir veya üstü yüklü NPS rol hizmetine sahip. Bu bölümdeki tüm adımlar, Windows Server 2016'yı kullanarak gerçekleştirildi.

### <a name="network-policy-and-access-services-nps-role"></a>Ağ İlkesi ve erişim Hizmetleri'ni (NPS) rol
NPS rol hizmetinin işlevselliğinin yanı sıra ağ erişim ilkesi sistem sağlığı hizmeti RADIUS sunucusu ve istemci sağlar. Bu rol altyapınızda en az iki bilgisayara yüklenmesi gerekir: Uzak Masaüstü Ağ geçidi ve başka bir üye sunucu veya etki alanı denetleyicisi. Varsayılan olarak, zaten Uzak Masaüstü Ağ geçidi olarak yapılandırılmış bilgisayarda rolüdür.  Ayrıca NPS rolü üzerinde en az bir etki alanı denetleyicisi veya üye sunucu gibi başka bir bilgisayar üzerinde yüklemeniz gerekir.

NPS rolü yükleme hakkında bilgi için Windows Server 2012 veya daha eski hizmet bkz [NAP sistem durumu ilkesi sunucusu yükleme](https://technet.microsoft.com/library/dd296890.aspx). NPS, bir etki alanı denetleyicisinde NPS'yi yüklemek için öneri de dahil olmak üzere en iyi bir açıklaması için bkz. [NPS için en iyi](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory, şirket içi Active Directory ile eşitlenmiş
NPS uzantısını kullanmak için şirket içi kullanıcıları Azure AD ile eşitlenen ve MFA için etkinleştirilmiş olmalıdır. Bu bölümde, şirket içi kullanıcıların AD Connect kullanarak Azure AD ile eşitlenir varsayılır. Azure AD hakkında bilgi için bkz [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği
NPS uzantısını yüklemek için Azure AD'nin GUID bilmeniz gerekir. Azure AD GUID'i bulmak için yönergeler aşağıda verilmiştir.

## <a name="configure-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını yapılandırma 
Bu bölümde, Azure mfa'yı Uzak Masaüstü Ağ geçidi ile tümleştirmeye yönelik yönergeler sağlar. Yönetici olarak, kullanıcılar şirket içinde çok faktörlü cihazlar ya da uygulamaları kaydedebilmek için önce Azure MFA hizmetini yapılandırmanız gerekir.

Bağlantısındaki [bulutta Azure multi Factor Authentication kullanmaya başlama](howto-mfa-getstarted.md) , Azure AD kullanıcıları için mfa'yı etkinleştirmek için. 

### <a name="configure-accounts-for-two-step-verification"></a>İki aşamalı doğrulama için hesaplarını yapılandırma
Bir hesap için mfa'yı etkinleştirildikten sonra ikinci kimlik doğrulama faktörü için kullanın ve iki aşamalı doğrulama kullanarak kimlik doğrulaması güvenilir bir cihaz başarılı bir şekilde yapılandırmadığınız sürece MFA İlkesi tarafından yönetilen kaynaklara oturum açamazsınız.

Bağlantısındaki [Azure multi-Factor Authentication benim için ne demektir?](../user-help/multi-factor-authentication-end-user.md) anlamak ve cihazlarınızı MFA için kullanıcı hesabınız ile düzgün şekilde yapılandırmak için.

## <a name="install-and-configure-nps-extension"></a>Yükleme ve NPS uzantısı yapılandırma
Bu bölümde, Uzak Masaüstü Ağ Geçidi istemci kimlik doğrulaması için Azure mfa'yı kullanmak için RDS altyapı yapılandırma için yönergeler sağlar.

### <a name="acquire-azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği Al

NPS uzantısı yapılandırma işleminin bir parçası olarak, Azure AD kiracınız için yönetici kimlik bilgileri ve Azure AD Kimliğini sağlamanız gerekir. Aşağıdaki adımlar Kiracı kimliğini almak nasıl gösterir

1. Oturum [Azure portalında](https://portal.azure.com) Azure kiracısının genel Yöneticisi olarak.
2. Sol gezinti bölmesinde **Azure Active Directory** simgesi.
3. Seçin **özellikleri**.
4. Özellikler dikey penceresinde, dizin Kimliği'nin yanındaki tıklayın **kopyalama** kimliği panoya kopyalamak için aşağıda gösterildiği gibi simgesi.

 ![Özellikler](./media/howto-mfa-nps-extension-rdg/image1.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleme
NPS uzantısı, Ağ İlkesi ve erişim Hizmetleri'ni (NPS) rolü yüklü bir sunucuya yükleyin. Tasarımınızı için RADIUS sunucusu olarak işlev görür. 

>[!Important]
> Uzak Masaüstü Ağ Geçidi sunucunuzda NPS uzantısı yüklemeyin emin olun.
> 

1. İndirme [NPS uzantısı](https://aka.ms/npsmfa). 
2. Yürütülebilir kurulum dosyası (NpsExtnForAzureMfaInstaller.exe) NPS sunucusuna kopyalayın.
3. NPS sunucusunda çift **NpsExtnForAzureMfaInstaller.exe**. İstenirse, tıklayın **çalıştırma**.
4. NPS uzantısı için Azure mfa'yı Kurulum iletişim kutusunda, yazılım lisans koşullarını gözden geçirin. kontrol **lisans hüküm ve koşulları kabul ediyorum**, tıklatıp **yükleme**.
 
  ![Azure mfa'yı Kurulum](./media/howto-mfa-nps-extension-rdg/image2.png)

5. NPS uzantısı için Azure mfa'yı Kurulum iletişim kutusunda **Kapat**. 

  ![Azure mfa NPS uzantısı](./media/howto-mfa-nps-extension-rdg/image3.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Bir PowerShell betiğini kullanarak NPS uzantısı ile kullanım için sertifikaları yapılandırma
Ardından, iletişimlerin güvenliğini sağlamak ve güvencesi sağlamak için NPS uzantısı tarafından sertifikalar kullanmak için yapılandırmanız gerekir. NPS bileşenleri otomatik olarak imzalanan bir sertifika kullanmak için NPS ile yapılandıran bir Windows PowerShell Betiği içerir. 

Komut aşağıdaki eylemleri gerçekleştirir:

* Otomatik olarak imzalanan bir sertifika oluşturur
* Hizmet sorumlusu Azure AD için sertifikanın ortak anahtarını ilişkilendirir
* Sertifika yerel makine deposu
* Sertifikanın özel anahtarı ağ kullanıcı erişimi verir
* Ağ İlkesi Sunucusu hizmetini yeniden başlatır

Kendi sertifikalarını kullanmak istiyorsanız, Azure AD'de hizmet sorumlusu sertifikanıza ortak anahtarı ilişkilendirmek vb. gerekir.

Betiği kullanmak için Azure AD yönetici kimlik bilgilerinizi ve daha önce kopyaladığınız Azure AD Kiracı Kimliğinizi uzantısı sağlar. Betik, NPS uzantısı yüklü olduğu her NPS sunucusunda çalıştırın. Ardından şunları yapın:

1. Bir yönetici Windows PowerShell istemi açın.
2. PowerShell isteminde `cd ‘c:\Program Files\Microsoft\AzureMfa\Config’`basın **ENTER**.
3. Tür `.\AzureMfsNpsExtnConfigSetup.ps1`basın **ENTER**. Betik, Azure Active Directory PowerShell Modülü yüklü olup olmadığını denetler. Yüklü değilse, komut sizin için modülünü yükler.

  ![Azure AD PowerShell](./media/howto-mfa-nps-extension-rdg/image4.png)
  
4. PowerShell modülünün yükleme betiği doğruladıktan sonra Azure Active Directory PowerShell modülü iletişim kutusunu görüntüler. İletişim kutusunda, Azure AD yönetici kimlik bilgilerini ve parolayı girin ve tıklayın **oturum**.

  ![PowerShell hesabı açın](./media/howto-mfa-nps-extension-rdg/image5.png)

5. İstendiğinde, panoya daha önce kopyaladığınız Kiracı kimliği yapıştırın ve basın **ENTER**.

  ![Kiracı Kimliğini girin](./media/howto-mfa-nps-extension-rdg/image6.png)

6. Betik, otomatik olarak imzalanan bir sertifika oluşturur ve başka yapılandırma değişiklikleri gerçekleştirir. Çıktı, görüntüyü aşağıda gösterildiği gibi olması gerekir.

  ![Kendinden imzalı bir sertifika](./media/howto-mfa-nps-extension-rdg/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Uzak Masaüstü Ağ Geçidi üzerinde NPS bileşenlerini yapılandırma
Bu bölümde, Uzak Masaüstü Ağ Geçidi bağlantısı Yetkilendirme İlkeleri ve diğer RADIUS ayarlarını yapılandırın.

Kimlik doğrulama akışı, Uzak Masaüstü Ağ geçidi ile NPS sunucusunun yüklü olduğu yeri NPS sunucusunun arasında RADIUS iletileri değiştirilebilmesi gerektirir. Başka bir deyişle, Uzak Masaüstü Ağ Geçidi hem NPS uzantısı, yüklü olduğu bir NPS sunucusunun RADIUS istemci ayarları yapılandırmalısınız. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-to-use-central-store"></a>Uzak Masaüstü Ağ Geçidi bağlantısı yetkilendirme ilkeleri merkezi deposunu kullanmak üzere yapılandırma
Uzak Masaüstü Bağlantısı Yetkilendirme İlkeleri (RD Cap'leri) bir Uzak Masaüstü Ağ Geçidi sunucusuna bağlanmak için koşulları belirtin. RD CAP yerel olarak depolanabilir (varsayılan) ya da depolanabilir NPS çalıştıran merkezi bir RD CAP Deposu içinde. RDS ile Azure mfa'yı tümleştirmesini yapılandırmak için merkezi bir depo kullanımını belirtmeniz gerekir.

1. RD Ağ Geçidi sunucusunda açın **Sunucu Yöneticisi**. 
2. Menüsünde **Araçları**, işaret **Uzak Masaüstü Hizmetleri**ve ardından **Uzak Masaüstü Ağ Geçidi Yöneticisi**.

  ![Uzak Masaüstü Hizmetleri](./media/howto-mfa-nps-extension-rdg/image8.png)

3. RD Ağ Geçidi Yöneticisi ' sağ  **\[sunucu adı\] (yerel)**, tıklatıp **özellikleri**.

  ![Sunucu Adı](./media/howto-mfa-nps-extension-rdg/image9.png)

4. Özellikler iletişim kutusunda, seçmek **RD CAP Store** sekmesi.
5. RD CAP Store sekmesinde **NPS çalıştıran merkezi sunucu**. 
6. İçinde **NPS çalıştıran sunucu için bir ad veya IP adresi girin** alan, NPS uzantısını yüklediğiniz sunucunun IP adresini veya sunucu adını yazın.

  ![Adını veya IP adresini girin](./media/howto-mfa-nps-extension-rdg/image10.png)
  
7. **Ekle**'ye tıklayın.
8. İçinde **paylaşılan gizli diziyi** iletişim kutusunda, paylaşılan gizlilik girin ve ardından **Tamam**. Bu paylaşılan gizli dizinin kaydedin ve kaydı güvenli bir şekilde saklayın emin olun.

 >[!NOTE]
 >Paylaşılan gizliliğin, RADIUS sunucuları ve istemciler arasında güven oluşturmak için kullanılır. Uzun ve karmaşık bir parola oluşturun.
 >

 ![Paylaşılan parola](./media/howto-mfa-nps-extension-rdg/image11.png)

9. **Tamam**’a tıklayarak iletişim kutusunu kapatın.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Uzak Masaüstü Ağ Geçidi NPS'ye RADIUS zaman aşımı değerini yapılandırma
Kullanıcıların kimlik bilgilerini doğrulamak için zaman olduğundan emin olmak için iki aşamalı doğrulamanın, yanıtlar almasına ve yanıt RADIUS iletiler için RADIUS zaman aşımı değeri ayarlamak gereklidir.

1. RD Ağ Geçidi sunucusunda, Sunucu Yöneticisi'ni açın. Menüsünde **Araçları**ve ardından **ağ ilkesi sunucusu**. 
2. İçinde **NPS (yerel)** genişletin **RADIUS istemcileri ve sunucuları**seçip **uzak RADIUS sunucu**.

 ![Uzak RADIUS sunucusu](./media/howto-mfa-nps-extension-rdg/image12.png)

3. Ayrıntılar bölmesinde **TS Ağ Geçidi sunucusu GRUBUNU**.

 >[!NOTE]
 >NPS ilkelerinin merkezi sunucu yapılandırıldığında bu RADIUS sunucu grubu oluşturuldu. RD Ağ Geçidi grubunda birden daha fazla olması durumunda bu sunucuya veya sunucu grubu RADIUS iletileri iletir.
 >

4. İçinde **TS Ağ Geçidi sunucusu grubu özellikleri** iletişim kutusunda, RD Cap'leri depolamak ve ardından yapılandırılmış NPS sunucusu adı ve IP adresi seçin **Düzenle**. 

 ![TS Ağ Geçidi sunucusu grubu](./media/howto-mfa-nps-extension-rdg/image13.png)

5. İçinde **RADIUS sunucusu Düzenle** iletişim kutusunda **Yük Dengeleme** sekmesi.
6. İçinde **Yük Dengeleme** sekmesinde **istek kabul etmeden önce yanıt almadan saniye sayısını bırakılan** alan, varsayılan değer 3 ila 30 ila 60 saniye arasında bir değer olarak değiştirin.
7. İçinde **sunucu kullanılamaz olarak tanımlandığında istekler arasındaki saniye sayısını** alanında, önceki adımda belirtilen değerden büyük veya ona eşit bir değer 30 saniye varsayılan değeri değiştirin.

 ![RADIUS sunucusunu Düzenle](./media/howto-mfa-nps-extension-rdg/image14.png)

8.  Tıklayın **Tamam** iletişim kutularını kapatmak için iki kez.

### <a name="verify-connection-request-policies"></a>Bağlantı isteği ilkeleri doğrulayın 
RD Ağ Geçidi bağlantısı Yetkilendirme İlkeleri için bir merkezi ilke deposunu kullanmak üzere yapılandırdığınızda, varsayılan olarak, RD Ağ Geçidi UÇ istekleri NPS sunucusuna iletmek için yapılandırılır. NPS sunucusu ile Azure mfa'yı uzantısı yüklü, RADIUS erişim isteğini işler. Aşağıdaki adımlar varsayılan bağlantı isteği ilkesi nasıl gösterir. 

1. NPS (yerel) konsolunda, RD Ağ Geçidi üzerinde genişletin **ilkeleri**seçip **bağlantı isteği ilkeleri**.
2. Sağ **bağlanma isteği ilkeleri**ve çift **TS Ağ Geçidi kimlik doğrulama İlkesi**.
3. İçinde **TS Ağ Geçidi kimlik doğrulama İlkesi Özellikleri** iletişim kutusu, tıklayın **ayarları** sekmesi.
4. Üzerinde **ayarları** iletme, bağlantı isteği altında sekmesini **kimlik doğrulaması**. RADIUS istemcisi, kimlik doğrulama istekleri iletmek için yapılandırılır.

 ![Kimlik doğrulama ayarları](./media/howto-mfa-nps-extension-rdg/image15.png)
 
5. Tıklayın **iptal**. 

## <a name="configure-nps-on-the-server-where-the-nps-extension-is-installed"></a>NPS uzantısı, yüklü olduğu sunucuda NPS yapılandırma
NPS uzantısı, yüklü olduğu bir NPS sunucusu ile NPS sunucu Uzak Masaüstü Ağ Geçidi üzerinde RADIUS mesaj alışverişi gerekir. Bu ileti değişim etkinleştirmek için NPS uzantısı Hizmeti'nin yüklendiği sunucuda NPS bileşenleri yapılandırmanız gerekir. 

### <a name="register-server-in-active-directory"></a>Sunucusu Active Directory'de Kaydettir
Bu senaryoda düzgün çalışması için NPS sunucusunun Active Directory'de kayıtlı olması gerekir.

1. NPS sunucusunda açın **Sunucu Yöneticisi**.
2. Sunucu Yöneticisi'nde **Araçları**ve ardından **ağ ilkesi sunucusu**. 
3. Sağ tıklayın ağ ilkesi sunucusu konsolunda **NPS (yerel)** ve ardından **Active Directory'de kayıt sunucusu**. 
4. Tıklayın **Tamam** iki kez.

 ![Sunucu AD'ye kaydetme](./media/howto-mfa-nps-extension-rdg/image16.png)

5. Konsolunu sonraki yordam için açık bırakın.

### <a name="create-and-configure-radius-client"></a>Oluşturma ve RADIUS istemci yapılandırma 
Uzak Masaüstü Ağ Geçidi NPS sunucusunun RADIUS istemcisi olarak yapılandırılması gerekir. 

1. NPS uzantısı yüklü olduğu, buna NPS sunucusunda **NPS (yerel)** konsolunda, sağ **RADIUS istemcileri** tıklatıp **yeni**.

 ![Yeni RADIUS istemcisi](./media/howto-mfa-nps-extension-rdg/image17.png)

2. İçinde **yeni RADIUS istemcisi** iletişim kutusunda, gibi kolay bir ad verin _ağ geçidi_ve IP adresi ya da Uzak Masaüstü Ağ Geçidi sunucusunun DNS adı. 
3. İçinde **paylaşılan gizlilik** ve **paylaşılan gizliliği onayla** alanları önceden kullanmış olduğunuz aynı parolayı girin.

 ![Ad ve adres](./media/howto-mfa-nps-extension-rdg/image18.png)

4. Tıklayın **Tamam** yeni RADIUS istemcisi iletişim kutusunu kapatın.

### <a name="configure-network-policy"></a>Ağ İlkesi yapılandırma
NPS sunucusu ile Azure mfa'yı uzantısı atanmış merkezi ilke deposu bağlantı yetkilendirme ilkesi (CAP) için olduğunu hatırlayın. Bu nedenle, bir büyük harf geçerli bağlantı isteklerini yetkilendirmek için NPS sunucusuna uygulamak gerekir.  

1. NPS (yerel) konsolda **ilkeleri**, tıklatıp **ağ ilkeleri**.
2. Sağ **diğer erişim sunucularına bağlantı**, tıklatıp **yinelenen ilke**. 

 ![Yinelenen ilke](./media/howto-mfa-nps-extension-rdg/image19.png)

3. Sağ **diğer erişim sunucularına bağlantı kopyalama**, tıklatıp **özellikleri**.

 ![Ağ Özellikleri](./media/howto-mfa-nps-extension-rdg/image20.png)

4. İçinde **diğer erişim sunucularına bağlantı kopyalama** iletişim kutusundaki **ilke adı**, gibi uygun bir ad girin _RDG_CAP_. Denetleme **etkin ilke**seçip **erişim ver**. İsteğe bağlı olarak **ağ erişim sunucusu türü**seçin **Uzak Masaüstü Ağ Geçidi**, veya olarak bırakılabilir **belirtilmemiş**.

 ![Bağlantı kopyalama](./media/howto-mfa-nps-extension-rdg/image21.png)

5.  Tıklayın **kısıtlamaları** sekmesini tıklatıp denetleyin **istemcilerin kimlik doğrulama yöntemi anlaşmadan bağlanmasına izin ver**.

 ![İstemcilerin bağlanmasına izin ver](./media/howto-mfa-nps-extension-rdg/image22.png)

6. İsteğe bağlı olarak, tıklayın **koşullar** sekmesini ve bağlantının, örneğin, belirli bir Windows grubu üyeliği'da yetkilendirilmesi karşılanması gereken koşulları ekleyin.

 ![Koşullar](./media/howto-mfa-nps-extension-rdg/image23.png)

7. **Tamam**’a tıklayın. İlgili Yardım konusunu görüntülemek için sorulduğunda **Hayır**.
8. Erişim verir ve yeni ilkeniz ilkenin etkinleştirildiğini listenin başında olduğundan emin olun.

 ![Ağ ilkeleri](./media/howto-mfa-nps-extension-rdg/image24.png)

## <a name="verify-configuration"></a>Yapılandırmayı doğrulama
Yapılandırmayı doğrulamak için uygun bir RDP istemcisi ile Uzak Masaüstü Ağ geçidi için oturum açmanız gerekir. Bağlantı Yetkilendirme ilkelerinizi tarafından izin verilen ve Azure MFA için etkinleştirilmiş bir hesap kullandığınızdan emin olun. 

Aşağıdaki görüntüde gösterdiği gibi kullandığınız **Uzak Masaüstü Web erişimi** sayfası.

![Uzak Masaüstü Web erişimi](./media/howto-mfa-nps-extension-rdg/image25.png)

Kimlik bilgilerinizi birincil kimlik doğrulaması için başarıyla girildikten sonra Uzak Masaüstü Bağlantısı iletişim kutusu başlatma uzak bağlantı durumunu aşağıda gösterildiği gibi gösterir. 

Başarılı bir şekilde Azure mfa'yı daha önce yapılandırdığınız ikincil kimlik doğrulama yöntemi ile kimlik doğrulaması, kaynağa bağlıdır. Ancak, ikincil kimlik doğrulaması başarılı olmazsa, kaynağa erişimi reddedilir. 

![Uzak bağlantı](./media/howto-mfa-nps-extension-rdg/image26.png)

Aşağıdaki örnekte, kimlik doğrulayıcı uygulamasını bir Windows phone'da ikincil kimlik doğrulaması sağlamak için kullanılır.

![Hesaplar](./media/howto-mfa-nps-extension-rdg/image27.png)

İkincil kimlik doğrulama yöntemi başarıyla doğruladıktan sonra Uzak Masaüstü Ağ Geçidi normal şekilde oturum açtığınız. Güvenilen bir cihazda mobil uygulama kullanarak bir ikincil kimlik doğrulama yöntemini kullanmak için gerekli olduğundan, ancak oturum açma işleminiz, aksi takdirde duruma göre daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Başarılı oturum açma olayları için Olay Görüntüleyicisi günlüklerini görüntüle
Başarılı oturum açma olaylarını Windows Olay Görüntüleyicisi günlükleri görüntülemek için Windows Terminal Hizmetleri ve Windows güvenlik günlükleri sorgulamak için aşağıdaki Windows PowerShell komutunu verebilir.

Ağ geçidi işlem günlüklerinde başarılı oturum açma olaylarını sorgulamak için _(olay görüntüleyicisi\uygulama ve hizmet Logs\Microsoft\Windows\TerminalServices-Gateway\Operational)_, aşağıdaki PowerShell komutlarını kullanın:

* _Get-WinEvent günlükadı - Microsoft-Windows-TerminalServices-ağ geçidi/işletimsel_ | burada {$_.ID - eq '300'} | FL 
* Bu komut, kullanıcının kaynak yetkilendirme ilkesi gereksinimlerini (RD RAP) sağlandığından ve erişim izni verildi gösteren Windows olayları görüntüler.

![Olay Görüntüleyicisi'ni görüntüleme](./media/howto-mfa-nps-extension-rdg/image28.png)

* _Get-WinEvent günlükadı - Microsoft-Windows-TerminalServices-ağ geçidi/işletimsel_ | burada {$_.ID - eq '200'} | FL
* Bu komut, kullanıcı bağlantısı yetkilendirme ilkesi gereksinimleri sağlandığında gösteren olayları görüntüler.

![Bağlantı Yetkilendirme](./media/howto-mfa-nps-extension-rdg/image29.png)

Bu günlük ve filtre olay kimlikleri, 300 ve 200 üzerinde görüntüleyebilirsiniz. Başarılı oturum açma olaylarını Güvenlik Olay Görüntüleyicisi günlüklerinde sorgulamak için aşağıdaki komutu kullanın:

* _Get-WinEvent - günlükadı güvenlik_ | burada {$_.ID - eq '6272'in '} | FL 
* Bu komut, merkezi NPS veya RD Ağ Geçidi sunucusu üzerinde çalıştırılabilir. 

![Başarılı oturum açma olayları](./media/howto-mfa-nps-extension-rdg/image30.png)

Aşağıda gösterildiği gibi güvenlik günlüğü ya da Ağ İlkesi ve Erişim Hizmetleri özel görünümü da görüntüleyebilirsiniz:

![Ağ İlkesi ve Erişim Hizmetleri](./media/howto-mfa-nps-extension-rdg/image31.png)

Azure MFA için NPS uzantısı yüklendiği sunucuda Olay Görüntüleyicisi'ni uygulama günlükleri uzantısına özel bulabilirsiniz _uygulama ve Hizmetleri Logs\Microsoft\AzureMfa_. 

![Olay Görüntüleyicisi'ni uygulama günlükleri](./media/howto-mfa-nps-extension-rdg/image32.png)

## <a name="troubleshoot-guide"></a>Sorun giderme kılavuzu

Yapılandırma beklendiği gibi çalışmıyorsa, sorun giderme başlamak için ilk kullanıcının Azure mfa'yı kullanmak için yapılandırıldığını doğrulamak için yerdir. Bağlanma kullanıcının [Azure portalında](https://portal.azure.com). Kullanıcıların ikincil doğrulama için istenir ve başarıyla kimlik doğrulaması, Azure MFA'ın hatalı bir yapılandırma ortadan kaldırabilir.

Azure MFA için kullanıcı olarak çalışıyorsa, ilgili olay günlüklerini gözden geçirmelisiniz. Bunlar, güvenlik olayı, ağ geçidi işletimsel ve önceki bölümde açıklanan Azure mfa'yı günlükleri içerir. 

Aşağıda güvenlik günlüğü başarısız oturum açma olayı (olay kimliği 6273) gösteren bir örnek çıktı verilmiştir.

![Başarısız oturum açma olayı](./media/howto-mfa-nps-extension-rdg/image33.png)

İlgili bir olay AzureMFA günlüklerinden aşağıdadır:

![Azure MFA günlük](./media/howto-mfa-nps-extension-rdg/image34.png)

Gerçekleştirmek için Gelişmiş Seçenekleri sorun giderin, NPS hizmetinin yüklendiği NPS veritabanı biçimi günlük dosyalarına başvurun. Bu günlük dosyaları oluşturulur _%SystemRoot%\System32\Logs_ klasör virgülle ayrılmış metin dosyalarını olarak. 

Bunlar bir açıklaması için günlük dosyaları, bkz: [NPS veritabanı biçimi günlük dosyalarını yorumlamak](https://technet.microsoft.com/library/cc771748.aspx). Bu günlük dosyaları girişleri bir elektronik tablo veya bir veritabanı aktarmadan yorumlamak zor olabilir. Günlük dosyaları yorumlama içinde yardımcı olmak için birkaç IAS Çözümleyicileri çevrimiçi bulabilirsiniz. 

Aşağıdaki resimde bir çıktı gösterir böyle indirilebilir [paylaşılan yazılım uygulama](http://www.deepsoftware.com/iasviewer). 

![Paylaşılan yazılım uygulama](./media/howto-mfa-nps-extension-rdg/image35.png)

Son olarak, için ek sorun giderme seçenekleri, bir protokol çözümleyici kullanabileceğiniz gibi [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). 

Microsoft Message Analyzer görüntüden aşağıdaki kullanıcı adını içeren RADIUS protokolü filtrelenmiş ağ trafiğini gösterir **Contoso\AliceC**.

![Microsoft ileti Çözümleyicisi](./media/howto-mfa-nps-extension-rdg/image36.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure Multi-Factor Authentication’ı edinme](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md)
