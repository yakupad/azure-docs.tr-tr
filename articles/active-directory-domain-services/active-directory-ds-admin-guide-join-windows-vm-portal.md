---
title: 'Azure Active Directory etki alanı Hizmetleri: bir Windows Server VM yönetilen bir etki alanına katılın. | Microsoft Docs'
description: Windows Server sanal makinesini Azure AD DS'ye ekleme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: maheshu
ms.openlocfilehash: 2929f85b738171f7fb7f5b66af90e4e2ab54f5d0
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317179"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Windows Server sanal makinesini yönetilen bir etki alanına ekleme
Bu makalede Azure portalını kullanarak bir Windows Server sanal makine dağıtma gösterilmektedir. Ardından, sanal makine bir Azure Active Directory etki alanı Hizmetleri (Azure AD DS) yönetilen etki alanına katılmak nasıl gösterir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="step-1-create-a-windows-server-virtual-machine"></a>1. adım: bir Windows Server sanal makine oluşturma
Azure AD DS etkinleştirdikten sanal ağa katılmış bir Windows sanal makine oluşturmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](http://portal.azure.com)’da oturum açın.
2. Sol bölmenin en üstünde seçin **yeni**.
3. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.

    ![Windows Server 2016 Datacenter bağlantı](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. İçinde **Temelleri** bölmesinde Sihirbazı'nın sanal makine için temel ayarlarını yapılandırın.

    ![Temel kavramları bölmesi](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Sanal makinede oturum açmak için kullanılan bir yerel yönetici hesabının kullanıcı adı ve parola buraya girdiğiniz içindir. Sanal makineyi parola yanılma saldırılarına karşı korumak için güçlü bir parola seçin. Burada etki alanı kullanıcı hesabının kimlik bilgilerini girmeyin.
    >

5. Seçin bir **boyutu** sanal makine için. Daha fazla boyutları görüntülemek için seçin **tüm görüntüle** veya değiştirme **desteklenen disk türü** Filtresi.

    !["Bir boyutu seçin" bölmesi](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. İçinde **ayarları** bölmesinde, Azure AD DS tarafından yönetilen etki alanı dağıtıldığı sanal ağı seçin. Yönetilen etki alanınızı içine dağıtılan olandan farklı bir alt ağ seçin. Diğer ayarlar için varsayılanları tutun ve ardından **Tamam**.

    ![Sanal makine için sanal ağ ayarları](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Doğru sanal ağ ve alt ağ seçin.**
    >
    > Yönetilen etki alanınızı dağıtıldığı sanal ağı seçin veya sanal kullanarak bağlı olduğu bir sanal ağ ağ eşlemesi. Bağlantısız bir sanal ağ seçerseniz, sanal makineyi yönetilen bir etki alanına katılamaz.
    >
    > Ayrılmış bir alt ağ, yönetilen etki alanı dağıtma öneririz. Bu nedenle, yönetilen etki alanınızı etkinleştirdikten alt çekme değil.

7. Diğer ayarlar için varsayılanları tutun ve ardından **Tamam**.
8. Üzerinde **satın alma** sayfasında, ayarları gözden geçirin ve ardından **Tamam** sanal makine dağıtılamıyor.
9. VM dağıtımı Azure portal panosuna sabitlendi.

    ![Bitti](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Dağıtım tamamlandıktan sonra üzerinde VM hakkındaki bilgileri görüntüleyebilirsiniz **genel bakış** sayfası.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>2. adım: yerel yönetici hesabını kullanarak Windows Server sanal makineye bağlanma
Ardından, yeni oluşturulan Windows Server sanal makine etki alanına bağlayın. Sanal makineyi oluşturduğunuzda belirttiğiniz yerel yönetici kimlik bilgilerini kullanın.

Sanal makineye bağlanmak için aşağıdaki adımları gerçekleştirin:

1. İçinde **genel bakış** bölmesinde, **Bağlan**.  
    Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve indirilir.

    ![Windows sanal makineye bağlanma](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. VM'nize bağlanmak için indirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.
3. Girin, **yerel yönetici kimlik bilgilerinin**, sanal makineyi oluşturduğunuzda belirttiğiniz (örneğin, *localhost\mahesh*).
4. Oturum açma işlemi sırasında bir sertifika uyarısı görürseniz, seçin **Evet** veya **devam** bağlanmak için.

Bu noktada, yeni oluşturulan Windows sanal makine için yerel yönetici kimlik bilgilerinizle oturum açmanız. Sanal Makine etki alanına katılmak için sonraki adım olacaktır.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>3. adım: Windows Server sanal makine Azure AD DS tarafından yönetilen etki alanına katılın.
Windows Server sanal makine Azure AD DS tarafından yönetilen etki alanına katılmak için aşağıdaki adımları tamamlayın:

1. "2. adımda." gösterildiği gibi Windows Server VM Bağlan Üzerinde **Başlat** ekran, açık **Sunucu Yöneticisi'ni**.
2. Sol bölmesinde **Sunucu Yöneticisi'ni** penceresinde, seçin **yerel sunucu**.

    ![Sanal makine üzerinde Sunucu Yöneticisi penceresi](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Altında **özellikleri**seçin **çalışma grubu**.
4. İçinde **Sistem Özellikleri** penceresinde, seçin **değişiklik** etki alanına katılamaz.

    ![Sistem Özellikleri penceresi](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. İçinde **etki alanı** kutusunda DS tarafından yönetilen Azure AD etki alanınızın adını belirtin ve ardından **Tamam**.

    ![Birleştirilecek etki alanını belirtin](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. Etki alanına katılmak için kimlik bilgilerinizi girmeniz istenir. Kimlik bilgilerini kullanan bir *Azure AD DC Yöneticiler grubuna ait kullanıcı*. Yalnızca bu grubun üyeleri makinelere yönetilen etki alanına katılmak için izinleri yönetme izinleri var.

    ![Kimlik bilgileri belirtmek için Windows Güvenlik penceresinde](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. Kimlik bilgileri aşağıdaki yollardan birini belirtebilirsiniz:

   * **UPN biçimini**: Azure AD içinde yapılandırıldığı gibi kullanıcı hesabı için kullanıcı asıl adı (UPN) soneki (önerilen) belirtin. Bu örnekte, Kullanıcı UPN sonekinin *bob* olan *bob@domainservicespreview.onmicrosoft.com*.

   * **SAMAccountName biçimi**: SAMAccountName biçiminde hesap adını belirtebilirsiniz. Bu örnekte, kullanıcı *bob* girmesi *CONTOSO100\bob*.

     > [!TIP]
     > **Kimlik bilgileri belirtmek için UPN biçimini kullanmanızı öneririz.**
     >
     > Bir kullanıcının UPN önek aşırı uzun olması durumunda (örneğin, *joehasareallylongname*), otomatik olarak oluşturulan SAMAccountName olabilir. Birden çok kullanıcı aynı UPN sonekine varsa (örneğin, *bob*), Azure AD kiracınızda hizmeti tarafından otomatik olarak oluşturulan kendi SAMAccountName biçimi olabilir. Bu durumlarda, UPN biçimini güvenilir etki alanında oturum açmak için kullanılabilir.
     >

8. Başarılı bir şekilde bir etki alanına katıldıktan sonra aşağıdaki ileti etki alanına memnuniyetle karşılamaktadır.

    ![Etki alanına Hoş Geldiniz](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Etki alanına katılma tamamlamak için sanal makineyi yeniden başlatın.

## <a name="troubleshoot-joining-a-domain"></a>Bir etki alanına katılma sorunlarını giderme
### <a name="connectivity-issues"></a>Bağlantı sorunları
Sanal Makine etki alanı bulamıyor ise, aşağıdaki sorun giderme adımlarını deneyin:

* Sanal makineyi Azure AD DS etkin aynı sanal ağa bağlı olduğunu doğrulayın. Aksi halde, sanal makineye bağlanmak veya etki alanına katılmak alamıyor.

* Sırayla Azure AD DS etkin sanal ağa bağlı bir sanal ağdaki sanal makine olduğunu doğrulayın.

* Yönetilen etki alanının DNS etki alanı adını ping işlemi yapmayı deneyin (örneğin, *contoso100.com ping*). Bunu yapamıyorsanız, IP adreslerini Azure AD DS etkinleştirdiğiniz sayfada görüntülenen etki alanı için ping işlemi yapmayı deneyin (örneğin, *10.0.0.4 ping*). IP adresi, ancak etki alanı değil ping, DNS hatalı yapılandırılmış olabilir. Sanal ağ için DNS sunucuları olarak yapılandırılmış IP adresleri etki alanının olup olmadığını denetleyin.

* Sanal makinedeki DNS çözümleyicisi önbelleği temizleme deneyin (*ipconfig/flushdns*).

Etki alanına katılmak kimlik bilgilerini ister bir pencere gösterilirse, bağlantı sorunları yok.

### <a name="credentials-related-issues"></a>Kimlik bilgileri ile ilgili sorunlar
Kimlik bilgileriyle konusunda sorun yaşıyoruz ve etki alanına katılamıyor, aşağıdaki sorun giderme adımlarını deneyin:

* Kimlik bilgileri belirtmek için UPN biçimini kullanmayı deneyin. Kiracınızda aynı UPN sonekine sahip çok sayıda kullanıcı varsa veya UPN önek aşırı uzun olması durumunda, SAMAccountName, hesabınız için otomatik olarak oluşturulan olabilir. Bu durumlarda, hesabınız için SAMAccountName biçimi ne, beklediğiniz veya şirket içi etki alanınızda kullanmayı farklı olabilir.

* Ait olduğu bir kullanıcı hesabının kimlik bilgilerini kullanmayı deneyin *AAD DC Yöneticiler* grubu.

* Sahip olduğunuz onay [parola eşitleme etkin](active-directory-ds-getting-started-password-sync.md) yönetilen etki alanınız için.

* Azure AD içinde yapılandırıldığı gibi kullanıcı UPN kullandığınız denetleyin (örneğin, *bob@domainservicespreview.onmicrosoft.com*) oturum açmak için.

* Tamamlanması yeterince uzun parola eşitleme için bekleyin başlangıç kılavuzuna belirtilmiş.

## <a name="related-content"></a>İlgili içerik
* [Azure AD DS Başlarken Kılavuzu](active-directory-ds-getting-started.md)
* [Bir Azure AD DS tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
