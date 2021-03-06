---
title: 'Azure Active Directory etki alanı Hizmetleri: CentOS VM yönetilen bir etki alanına katılın. | Microsoft Docs'
description: Azure AD Etki Alanı Hizmetleri'ne CentOS Linux sanal makine birleştirme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: d76371935fddbfe94c6dc45e27971487e7fa4277
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333590"
---
# <a name="join-a-centos-linux-virtual-machine-to-a-managed-domain"></a>CentOS Linux sanal makinesini yönetilen bir etki alanına katma
Bu makalede CentOS Linux sanal makine Azure'da bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma kullanmayı gösterir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
3. **Azure AD etki alanı Hizmetleri** Azure AD dizini için etkinleştirilmesi gerekir. Bunu yapmadıysanız, özetlenen tüm görevleri izleyin [Getting Started guide](active-directory-ds-getting-started.md).
4. Sanal ağ için DNS sunucuları olarak yönetilen etki alanı IP adreslerini yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz: [Azure sanal ağı için DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. İçin gerekli adımları [Azure AD etki alanı Hizmetleri yönetilen etki alanınız için parola eşitleme](active-directory-ds-getting-started-password-sync.md).


## <a name="provision-a-centos-linux-virtual-machine"></a>CentOS Linux sanal makine sağlama
Aşağıdaki yöntemlerden birini kullanarak azure'da bir CentOS sanal makine sağlama:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineye dağıtmak **Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz aynı sanal ağ**.
> * Çekme bir **farklı alt** Azure AD etki alanı Hizmetleri içinde etkinleştirdiğiniz olandan.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Uzaktan yeni sağlandı Linux sanal makineye bağlanma
CentOS sanal makine Azure'da sağlandı. Sonraki uzaktan VM sağlarken oluşturulmuş yerel yönetici hesabı kullanarak sanal makineye bağlanmak için bir görevdir.

Makalesindeki yönergeleri izleyin [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makine üzerinde konak dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresi ve ana bilgisayar adını güncelleştirin.

```
sudo vi /etc/hosts
```

Hosts dosyasında şu değeri girin:

```
127.0.0.1 contoso-centos.contoso100.com contoso-centos
```
Burada, 'contoso100.com' DNS etki alanı, yönetilen etki alanı adıdır. 'contoso-centos' CentOS sanal makineyi yönetilen bir etki alanına katılma barındırıcı adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Gerekli paketler Linux sanal makineye yükleme
Ardından, sanal makinenin etki alanına katılma için gerekli paketleri yüklemek. SSH terminalinizde gereken paketleri yüklemek için aşağıdaki komutu yazın:

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen bir etki alanına katılma
Gerekli paketleri Linux sanal makinede yüklü olan, sonraki görev sanal makineyi yönetilen etki alanına belirlemektir.

1. AAD etki alanı Hizmetleri yönetilen etki bulur. SSH terminalinizde aşağıdaki komutu yazın:

    ```
    sudo realm discover CONTOSO100.COM
    ```

    > [!NOTE]
    > **Sorun giderme:** varsa *bölge Bul* yönetilen etki alanınızı bulamıyor:  
      * Etki alanı sanal makine (try ping) erişilebilir olduğundan emin olun.  
      * Sanal makine yönetilen etki alanı kullanılamıyor aynı sanal ağa gerçekten dağıtıldıktan denetleyin.
      * Sanal ağın DNS sunucusu ayarlarını yönetilen etki alanının etki alanı denetleyicilerine işaret edecek şekilde güncelleştirdiyseniz denetleyin.  
      >

2. Kerberos başlatır. SSH terminalinizde aşağıdaki komutu yazın:

    > [!TIP]
    > * 'AAD DC Yöneticiler' grubuna ait bir kullanıcı belirtin.
    > * Büyük harflerle etki alanı adı belirtin, başka kinit başarısız olur.
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. Makine etki alanına katılın. SSH terminalinizde aşağıdaki komutu yazın:

    > [!TIP]
    > Önceki adımda ('kinit') belirtilen aynı kullanıcı hesabı kullanın.
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

("Başarılı bir şekilde kaydedilen makine bölgedeki") bir ileti almalısınız zaman makine başarıyla katılırsa yönetilen etki.


## <a name="verify-domain-join"></a>Etki alanına katılma doğrulayın
Makine için yönetilen etki alanı başarıyla katıldı olup olmadığını doğrulayın. Etki alanına katılmış CentOS VM farklı bir SSH bağlantısı kullanarak bağlanın. Bir etki alanı kullanıcı hesabı kullanmanızı ve kullanıcı hesabının doğru çözülmüş olup olmadığını denetleyin.

1. Terminal, SSH SSH kullanarak CentOS sanal makine etki alanına bağlanmak için aşağıdaki komutu yazın katıldı. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, 'bob@CONTOSO100.COM' Bu durumda.)
    ```
    ssh -l bob@CONTOSO100.COM contoso-centos.contoso100.com
    ```

2. SSH terminalinizde giriş dizini doğru başlatılmadı görmek için aşağıdaki komutu yazın.
    ```
    pwd
    ```

3. SSH terminalinizde grup üyeliklerini doğru çözüldüğünü olmadığını görmek için aşağıdaki komutu yazın.
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>Sorun giderme etki alanına katılma
Başvurmak [sorun giderme etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm-portal.md#troubleshoot-joining-a-domain) makalesi.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Bir Windows Server sanal makine bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makine için oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Kerberos yükleme](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows tümleştirme Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
