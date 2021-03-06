---
title: Farklı Azure Stack geliştirme Seti'ni ortamlarındaki iki sanal ağ arasında siteden siteye VPN bağlantısı oluşturma | Microsoft Docs
description: İki tek düğümlü Azure Stack geliştirme Seti'ni ortamlar arasında siteden siteye VPN bağlantısı oluşturmak için bir bulut yöneticisinin kullandığı adım adım yordam.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/10/2017
ms.author: brenduns
ms.reviewer: scottnap
ROBOTS: NOINDEX
ms.openlocfilehash: 6225a12b50ebb7bf0a0cb9244153800ba734d93a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006912"
---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-development-kit-environments"></a>Farklı Azure Stack geliştirme Seti'ni ortamlarındaki iki sanal ağ arasında siteden siteye VPN bağlantısı oluşturma
## <a name="overview"></a>Genel Bakış
Bu makalede iki ayrı Azure Stack geliştirme Seti'ni ortamlarındaki iki sanal ağ arasında siteden siteye VPN bağlantısı oluşturma işlemini gösterir. Bağlantıları yapılandırırken Azure Stack hizmetinde VPN ağ geçitlerinin nasıl çalıştığını öğrenin.

### <a name="connection-diagram"></a>Bağlantı diyagramı
Aşağıdaki diyagram, işiniz bittiğinde bağlantı yapılandırması gibi görünmelidir gösterir.

![Siteden siteye VPN bağlantısı yapılandırma](media/azure-stack-create-vpn-connection-one-node-tp2/OneNodeS2SVPN.png)

### <a name="before-you-begin"></a>Başlamadan önce
Bağlantı yapılandırmasını tamamlamak için başlamadan önce aşağıdaki öğelerin bulunduğundan emin olun:

* İki sunucu ve açıklandığı gibi Azure Stack geliştirme Seti'ni donanım gereksinimlerini karşılayan diğer önkoşulları [hızlı başlangıç: Azure Stack geliştirme Seti'ni değerlendirmek](azure-stack-deploy-overview.md). 
* [Azure Stack geliştirme Seti'ni](https://azure.microsoft.com/overview/azure-stack/try/) dağıtım paketi.

## <a name="deploy-the-azure-stack-development-kit-environments"></a>Azure Stack geliştirme Seti'ni ortamlarını dağıtma
Bağlantı yapılandırmasını tamamlamak için iki Azure Stack geliştirme Seti'ni ortamı dağıtmanız gerekir.
> [!NOTE] 
> Her Azure Stack geliştirme dağıttığınız Seti, izleyin [dağıtım yönergeleri](azure-stack-run-powershell-script.md). Bu makalede, Azure Stack geliştirme Seti'ni ortamları adlandırılır *POC1* ve *POC2*.


## <a name="prepare-an-offer-on-poc1-and-poc2"></a>POC1 ve POC2 bir teklif hazırlama
Böylece kullanıcı teklife abone olma ve sanal makineleri dağıtmak hem POC1 hem de POC2 teklif hazırlayın. Teklif oluşturma hakkında daha fazla bilgi için bkz: [kullanılabilir hale getirir sanal makineler, Azure Stack kullanıcılarını](azure-stack-tutorial-tenant-vm.md).

## <a name="review-and-complete-the-network-configuration-table"></a>Gözden geçirin ve ağ yapılandırması tablosu tamamlayın
Her iki Azure Stack geliştirme Seti'ni ortamları için ağ yapılandırması aşağıdaki tabloda özetlenmiştir. Ağınızdaki belirli dış BGPNAT adresi eklemek için tablodan sonra görüntülenen yordamı kullanın.

**Ağ yapılandırma tablosu**
|   |POC1|POC2|
|---------|---------|---------|
|Sanal ağ adı     |VNET-01|VNET-02 |
|Sanal ağ adres alanı |10.0.10.0/23|10.0.20.0/23|
|Alt ağ adı     |Alt ağ-01|Alt ağ-02|
|Alt ağ adres aralığı|10.0.10.0/24 |10.0.20.0/24 |
|Ağ geçidi alt ağı     |10.0.11.0/24|10.0.21.0/24|
|Dış BGPNAT adresi     |         |         |

> [!NOTE]
> Örnek ortamda dış BGPNAT IP adresleri, POC1 için 10.16.167.195 ve 10.16.169.131 POC2 için ' dir. Azure Stack geliştirme Seti'ni konaklarınız için BGPNAT IP adreslerini belirlemek için aşağıdaki yordamı kullanın ve ardından bunları önceki ağ yapılandırma tabloya ekleyin.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>NAT VM dış bağdaştırıcısının IP adresini alma
1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. Yönetici parolanızı değiştirmek için aşağıdaki Powershell kod düzenleme ve kod POC ana bilgisayarında çalıştırın:

   ```powershell
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "AzS-bgpnat01" `
    -Password $Password
   ```
3. Önceki bölümde görünür ağ yapılandırma tablosu IP adresini ekleyin.

4. POC2 bu yordamı yineleyin.

## <a name="create-the-network-resources-in-poc1"></a>POC1'de ağ kaynakları oluşturma
Şimdi, ağ geçitleri ayarlarsınız ayarlamanız gerekir POC1 ağ kaynaklarını oluşturun. Aşağıdaki yönergeler kullanıcı portalını kullanarak kaynak oluşturma işlemini göstermektedir. Kaynakları oluşturmak için PowerShell kod da kullanabilirsiniz.

![Kaynakları oluşturmak için kullanılan iş akışı](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="sign-in-as-a-tenant"></a>Kiracı olarak oturum açın
Hizmet Yöneticisi bir kiracı planlar, teklifler ve kiracılarının kullanabileceği abonelikleri test etmek için oturum açabilir. Zaten yoksa, [Kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md) oturum açmadan önce.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma
1. Kullanıcı portalında oturum açmak için bir kiracı hesabı kullanın.
2. Kullanıcı Portalı'nda seçin **yeni**.

    ![Yeni sanal ağ oluşturma](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Git **Market**ve ardından **ağ**.
4. Seçin **sanal ağ**.
5. İçin **adı**, **adres alanı**, **alt ağ adı**, ve **alt ağ adres aralığı**, ağda görünen değerleri kullanın Yapılandırma tablo.
6. İçinde **abonelik**, daha önce oluşturduğunuz aboneliği görüntülenir.
7. İçin **kaynak grubu**, bir kaynak grubu oluşturabilir veya zaten bir varsa seçin **var olanı kullan**.
8. Varsayılan konumu doğrulayın.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ geçidi alt ağını oluşturma
1. Panoda, daha önce oluşturduğunuz VNET-01 sanal ağ kaynağı açın.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Sanal ağa bir ağ geçidi alt ağı eklemek için seçin **ağ geçidi alt ağı**.
   
    ![Ağ geçidi alt ağı ekleme](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Varsayılan olarak, alt ağ adı kümesine **GatewaySubnet**.
   Ağ geçidi alt ağları özeldir. Düzgün çalışması için bunlar kullanmalısınız *GatewaySubnet* adı.
5. İçinde **adres aralığı**, adresi olduğunu doğrulayın **10.0.11.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure portalında **yeni**. 
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden **sanal ağ geçidi**.
4. İçinde **adı**, girin **GW1**.
5. Seçin **sanal ağ** öğesini bir sanal ağ seçin.
   Seçin **VNET-01** listeden.
6. Seçin **genel IP adresi** menü öğesi. Zaman **genel IP adresi seçin** dikey penceresi açılır ve select **Yeni Oluştur**.
7. İçinde **adı**, girin **GW1-PiP**ve ardından **Tamam**.
8.  Varsayılan olarak, için **VPN türü**, **rota tabanlı** seçilir.
    Tutun **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway"></a>Yerel ağ geçidini oluşturma
Bu Azure Stack değerlendirme dağıtımındaki *yerel ağ geçidi* uygulaması, gerçek bir Azure dağıtımındaki uygulamadan biraz farklıdır.

Bir Azure dağıtımında yerel ağ geçidi, azure'daki bir sanal ağ geçidine bağlanmak için kullandığınız bir şirket içi (Kiracı) fiziksel cihazı, temsil eder. Bu Azure Stack değerlendirme dağıtımında, bağlantının her iki ucunda sanal ağ geçitleri demektir!

Bu konuda daha genel düşünmek için bir yerel ağ geçidi kaynağı her zaman bağlantının diğer ucundaki uzak ağ geçidi belirten yoludur. Azure Stack geliştirme Seti'ni tasarlanma şekli nedeniyle, ağ adresi çevirisi (NAT) diğer Azure Stack geliştirme Seti'ni genel IP adresi, VM üzerindeki dış ağ bağdaştırıcısının IP adresi sağlamak yerel ağ geçidi gerekir. Daha sonra her iki ucun da düzgün bağlandığından emin olmak için NAT VM üzerinde NAT eşlemeleri oluşturursunuz.


### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma
1. POC1 için Azure Stack fiziksel makinesinde oturum açın.
2. Kullanıcı Portalı'nda seçin **yeni**.
3. Git **Market**ve ardından **ağ**.
4. Kaynak listesinden **yerel ağ geçidi**.
5. İçinde **adı**, girin **POC2-GW**.
6. İçinde **IP adresi**, POC2 için dış BGPNAT adresini girin. Bu adres, daha önce ağ yapılandırma tablosunda görüntülenir.
7. İçinde **adres alanı**, daha sonra oluşturduğunuz POC2 VNET adres alanı için girin **10.0.20.0/23**.
8. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğundan ve ardından **Oluştur**.

### <a name="create-the-connection"></a>Bağlantı oluşturma
1. Kullanıcı Portalı'nda seçin **yeni**.
2. Git **Market**ve ardından **ağ**.
3. Kaynak listesinden **bağlantı**.
4. Üzerinde **Temelleri** ayarları dikey penceresinde için **bağlantı türü**seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** dikey penceresinde **sanal ağ geçidi**ve ardından **GW1**.
7. Seçin **yerel ağ geçidi**ve ardından **POC2-GW**.
8. İçinde **bağlantı adı**, girin **POC1-POC2**.
9. İçinde **paylaşılan anahtar (PSK)**, girin **12345**ve ardından **Tamam**.
10. Üzerinde **özeti** dikey penceresinde **Tamam**.

### <a name="create-a-vm"></a>VM oluşturma
VPN bağlantısından geçen verileri doğrulamak için her Azure Stack geliştirme Seti'ni veri alıp göndermek için sanal makinelerin gerekir. POC1'de, hemen bir sanal makine oluşturun ve ardından, VM alt ağında yerleştirin sanal ağınızda bulunan.

1. Azure portalında **yeni**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntüleri listesinde seçin **Windows Server 2016 Datacenter değerlendirme** görüntü.
4. Üzerinde **Temelleri** dikey penceresindeki **adı**, girin **VM01**.
5. Geçerli kullanıcı adı ve parolayı girin. Oluşturulduktan sonra VM'de oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** dikey penceresinde bu örnek için bir sanal makine boyutu seçin ve ardından **seçin**.
8. Üzerinde **ayarları** dikey penceresinde, Varsayılanları kabul edin. Emin **VNET-01** sanal ağın seçili. Alt ağ değerine ayarlandığını doğrulayın **10.0.10.0/24**. Sonra **Tamam**’ı seçin.
9. Üzerinde **özeti** dikey penceresinde, ayarları gözden geçirin ve ardından **Tamam**.



## <a name="create-the-network-resources-in-poc2"></a>POC2'de ağ kaynakları oluşturma

Sonraki adım, ağ kaynakları için POC2 oluşturmaktır. Aşağıdaki yönergeler kullanıcı portalını kullanarak kaynak oluşturma işlemini göstermektedir.

### <a name="sign-in-as-a-tenant"></a>Kiracı olarak oturum açın
Hizmet Yöneticisi bir kiracı planlar, teklifler ve kiracılarının kullanabileceği abonelikleri test etmek için oturum açabilir. Zaten yoksa, [Kiracı hesabı oluşturun](azure-stack-add-new-user-aad.md) oturum açmadan önce.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Sanal ağ ve VM alt ağı oluşturma

1. Bir kiracı hesabı kullanarak oturum açın.
2. Kullanıcı Portalı'nda seçin **yeni**.
3. Git **Market**ve ardından **ağ**.
4. Seçin **sanal ağ**.
5. POC2 için değerleri belirlemek için ağ yapılandırma tabloda daha önce görünen bilgileri kullanın **adı**, **adres alanı**, **alt ağ adı**ve **Alt ağ adres aralığı**.
6. İçinde **abonelik**, daha önce oluşturduğunuz aboneliği görüntülenir.
7. İçin **kaynak grubu**, yeni bir kaynak grubu oluşturun veya zaten bir hesabınız varsa seçin **var olanı kullan**.
8. Varsayılan doğrulama **konumu**.
9. **Panoya sabitle**’yi seçin.
10. **Oluştur**’u seçin.

### <a name="create-the-gateway-subnet"></a>Ağ Geçidi Alt Ağı oluşturma
1. Oluşturduğunuz sanal ağ kaynağı açın (**VNET-02**) panodan.
2. **Ayarlar** dikey penceresinde **Alt ağlar**’ı seçin.
3. Seçin **ağ geçidi alt ağı** sanal ağa bir ağ geçidi alt ağı eklemek için.
4. Alt ağın adı varsayılan olarak **GatewaySubnet** şeklinde ayarlanır.
   Ağ geçidi alt ağları özeldir ve düzgün şekilde çalışabilmesi için bu ada sahip olmalıdır.
5. İçinde **adres aralığı** adresi olduğunu doğrulayın, alan **10.0.21.0/24**.
6. Seçin **Tamam** ağ geçidi alt ağı oluşturmak için.

### <a name="create-the-virtual-network-gateway"></a>Sanal ağ geçidini oluşturma
1. Azure portalında **yeni**.  
2. Git **Market**ve ardından **ağ**.
3. Ağ kaynakları listesinden **sanal ağ geçidi**.
4. İçinde **adı**, girin **GW2**.
5. Bir sanal ağ seçmek için Seç **sanal ağ**. Ardından **VNET-02** listeden.
6. Seçin **genel IP adresi**. Zaman **genel IP adresi seçin** dikey penceresi açılır ve select **Yeni Oluştur**.
7. İçinde **adı**, girin **GW2-PiP**ve ardından **Tamam**.
8. Varsayılan olarak, için **VPN türü**, **rota tabanlı** seçilir.
    Tutun **rota tabanlı** VPN türü.
9. **Abonelik** ve **Konum** seçeneklerinin doğruluğunu onaylayın. Kaynağı panoya sabitleyebilirsiniz. **Oluştur**’u seçin.

### <a name="create-the-local-network-gateway-resource"></a>Yerel ağ geçidi kaynağı oluşturma

1. POC2 Kullanıcı Portalı'nda seçin **yeni**. 
4. Git **Market**ve ardından **ağ**.
5. Kaynak listesinden **yerel ağ geçidi**.
6. İçinde **adı**, girin **POC1-GW**.
7. İçinde **IP adresi**, POC1'de daha önce ağ yapılandırmasını tabloda listelenen dış BGPNAT adresini girin.
8. İçinde **adres alanı**, POC1 ' girin **10.0.10.0/23** ait adres alanı **VNET-01**.
9. Doğrulayın, **abonelik**, **kaynak grubu**, ve **konumu** doğru olduğundan ve ardından **Oluştur**.

## <a name="create-the-connection"></a>Bağlantı oluşturma
1. Kullanıcı Portalı'nda seçin **yeni**. 
2. Git **Market**ve ardından **ağ**.
3. Kaynak listesinden **bağlantı**.
4. Üzerinde **temel** ayarları dikey penceresinde için **bağlantı türü**, seçin **siteden siteye (IPSec)**.
5. Seçin **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
6. Üzerinde **ayarları** dikey penceresinde **sanal ağ geçidi**ve ardından **GW2**.
7. Seçin **yerel ağ geçidi**ve ardından **POC1-GW**.
8. İçinde **bağlantı adı**, girin **POC2-POC1**.
9. İçinde **paylaşılan anahtar (PSK)**, girin **12345**. Farklı bir değer seçerseniz, BT'nin unutmayın *gerekir* POC1 üzerinde oluşturduğunuz paylaşılan anahtar değeriyle eşleşmesi. **Tamam**’ı seçin.
10. Gözden geçirme **özeti** dikey penceresine tıklayın ve ardından **Tamam**.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
POC2'de, hemen bir sanal makine oluşturun ve sanal ağınızdaki VM alt yerleştirin.

1. Azure portalında **yeni**.
2. Git **Market**ve ardından **işlem**.
3. Sanal makine görüntüleri listesinde seçin **Windows Server 2016 Datacenter değerlendirme** görüntü.
4. Üzerinde **Temelleri** dikey penceresinde için **adı**, girin **VM02**.
5. Geçerli kullanıcı adı ve parolayı girin. Oluşturulduktan sonra sanal makineye oturum açmak için bu hesabı kullanın.
6. Sağlayan bir **abonelik**, **kaynak grubu**, ve **konumu**ve ardından **Tamam**.
7. Üzerinde **boyutu** bir sanal makine boyutu bu örneği için ve ardından dikey penceresinde **seçin**.
8. Üzerinde **ayarları** dikey penceresinde varsayılan değerleri kabul edebilirsiniz. Emin **VNET-02** sanal ağ, seçili olduğundan ve alt değerine ayarlandığını doğrulayın **10.0.20.0/24**. **Tamam**’ı seçin.
9. Ayarları gözden **özeti** dikey penceresine tıklayın ve ardından **Tamam**.

## <a name="configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal"></a>Her Azure Stack geliştirme Seti'ni ağ geçidi geçişi için NAT VM yapılandırma
Azure Stack geliştirme Seti'ni bağımsızdır ve fiziksel ana bilgisayar dağıtıldığı, ağdan yalıtılmış olduğu için *dış* ağ geçitlerinin bağlı VIP ağı gerçekten dış değildir. Bunun yerine, VIP ağının ağ adresi çevirisi gerçekleştiren bir yönlendiricinin arkasında gizlenmiştir. 

Adlı bir Windows Server sanal makinesi yönlendiricisidir *AzS-bgpnat01*, Azure Stack geliştirme Seti'ni altyapısında Yönlendirme ve Uzaktan Erişim Hizmetleri (RRAS) rolünü çalıştıran. Her iki End'i bağlanmak siteden siteye VPN bağlantısına izin vermek için AzS-bgpnat01 sanal makine, NAT yapılandırmanız gerekir. 

VPN bağlantısı yapılandırmak için BGPNAT VM üzerindeki dış arabirimi Edge ağ geçidi havuzunun VIP eşlemeleri bir statik NAT eşlemesi yolu oluşturmanız gerekir. Her bağlantı noktası bir VPN bağlantısı için bir statik NAT eşlemesi yol gereklidir.

> [!NOTE]
> Bu yapılandırma, yalnızca Azure Stack geliştirme Seti'ni ortamları için gereklidir.
> 
> 

### <a name="configure-the-nat"></a>NAT yapılandırma
> [!IMPORTANT]
> İçin bu yordamı tamamlamak zorundasınız *hem* Azure Stack geliştirme Seti'ni ortamları.

1. Belirlemek **iç IP adresi** aşağıdaki PowerShell Betiği kullanmak için. Sanal ağ geçidi (GW1 ve GW2) açın ve ardından **genel bakış** dikey penceresinde değerini kaydetme **genel IP adresi** daha sonra kullanmak için.
![İç IP adresi](media/azure-stack-create-vpn-connection-one-node-tp2/InternalIP.PNG)
2. POC1 için Azure Stack fiziksel makinesinde oturum açın.
3. Kopyalayın ve aşağıdaki PowerShell betiğini düzenleyin. Her Azure Stack geliştirme Seti'ni üzerinde NAT'ı yapılandırmak için yükseltilmiş bir Windows PowerShell ISE'de betik çalıştırın. Betikte değerlere Ekle *dış BGPNAT adresi* ve *iç IP adresi* yer tutucular:

   ```powershell
   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

4. POC2 bu yordamı yineleyin.

## <a name="test-the-connection"></a>Bağlantıyı sınama
Siteden siteye bağlantı kurulduktan sonra trafiği alabildiğinizi alabilirsiniz doğrulamalıdır. Doğrulamak için bir ya da Azure Stack geliştirme Seti'ni ortamda oluşturduğunuz sanal makine için oturum açın. Daha sonra diğer ortamda oluşturduğunuz sanal makineye ping atın. 

Siteden siteye bağlantı üzerinden trafiği gönderdiğinizden emin olmak için VIP uzak alt ağda bulunan sanal makinenin doğrudan IP (DIP) adresine ping gönderdiğinizden emin olun. Bunu yapmak için bağlantının diğer ucundaki DIP adresi bulun. Daha sonra kullanmak için adresini kaydedin.

### <a name="sign-in-to-the-tenant-vm-in-poc1"></a>POC1'deki Kiracı VM'de oturum açın
1. POC1 için Azure Stack fiziksel makinesinde oturum açın ve sonra kullanıcı portalında oturum açmak için bir kiracı hesabı kullanın.
2. Sol gezinti çubuğunda **işlem**.
3. VM listesinde, bulma **VM01** daha önce oluşturduğunuz ve ardından bu seçeneği belirleyin.
4. Sanal makine için dikey penceresinde tıklayın **Connect**ve ardından VM01.rdp dosyasını açın.
   
     ![Bağlanma düğmesi](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Sanal makine oluştururken yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş açın **Windows PowerShell** penceresi.
7. Girin **ipconfig/all**.
8. Çıktıda Bul **IPv4 adresi**ve daha sonra kullanmak için adresini kaydedin. POC2'den ping gönderilecek adres budur. Örnek ortamda adres **10.0.10.4** şeklindedir, ancak sizin ortamınızda farklı olabilir. İçinde içerilmesi gerekir **10.0.10.0/24** daha önce oluşturduğunuz alt ağ.
9. Sanal makine ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-poc2"></a>POC2'deki Kiracı VM'de oturum açın
1. POC2 için Azure Stack fiziksel makinesinde oturum açın ve sonra kullanıcı portalında oturum açmak için bir kiracı hesabı kullanın.
2. Sol gezinti çubuğunda **işlem**.
3. Sanal makineler listesinden Bul **VM02** daha önce oluşturduğunuz ve ardından bu seçeneği belirleyin.
4. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın.
5. Sanal makine oluştururken yapılandırdığınız hesabıyla oturum açın.
6. Yükseltilmiş açın **Windows PowerShell** penceresi.
7. Girin **ipconfig/all**.
8. İçinde denk gelen bir IPv4 adresi görmeniz gerekir **10.0.20.0/24**. Örnek ortamda adres olduğu **10.0.20.4**, ancak adresinizi farklı olabilir.
9. Sanal makine ping komutuna yanıt veren bir güvenlik duvarı kuralı oluşturmak için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. POC2 sanal makineden, POC1'de, sanal makinede tünel aracılığıyla ping atın. Bunu yapmak için VM01'den kaydettiğiniz DIP'ye ping gönderin.
   Örnek ortamda budur **10.0.10.4**, ancak laboratuvarınızda not aldığınız adrese ping gönderdiğinizden emin olun. Aşağıdakine benzer bir sonuç görmeniz gerekir:
   
    ![PING başarılı](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Uzak sanal makineden bir yanıt, testin başarılı olduğunu gösteriyor! Sanal makine pencereyi kapatabilirsiniz. Bağlantınızı test etmek için veri aktarımları dosya kopyası gibi diğer tür deneyebilirsiniz.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Ağ geçidi bağlantısı üzerinden veri aktarımı istatistiklerini görüntüleme
Ne kadar veri, siteden siteye bağlantı geçirir bilmek istiyorsanız, bu bilgi kullanılabilir **bağlantı** dikey penceresi. Bu test ayrıca yeni gönderdiğiniz ping gerçekten VPN bağlantısı üzerinden geçip geçmediğini doğrulamanın başka bir yoludur.

1. POC2'deki Kiracı sanal makineye oturum açmadıysanız, ancak kullanıcı portalında oturum açmak için Kiracı hesabınızı kullanın.
2. Git **tüm kaynakları**ve ardından **POC2-POC1** bağlantı. **Bağlantıları** görünür.
4. Üzerinde **bağlantı** dikey penceresinde istatistikleri **verilerinde** ve **verileri** görünür. Aşağıdaki ekran görüntüsünde, çok sayıda ek dosya aktarımı atanmıştır. Sıfır olmayan bazı değerler görürsünüz.
   
    ![Giren ve çıkan veriler](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)
