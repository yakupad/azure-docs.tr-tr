---
title: Azure noktadan siteye VPN bağlantıları ile ilgili | Microsoft Docs
description: Bu makalede, noktadan siteye bağlantılar anlamanıza yardımcı olur ve kullanmak için P2S VPN ağ geçidi kimlik doğrulama türünü karar vermenize yardımcı olur.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/06/2018
ms.author: cherylmc
ms.openlocfilehash: 8cdc80e8e4f8d3feb36ca82740d5610e60716ec6
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003368"
---
# <a name="about-point-to-site-vpn"></a>Noktadan siteye VPN hakkında

Noktadan Siteye (P2S) VPN ağ geçidi bağlantısı, ayrı bir istemci bilgisayardan sanal ağınıza güvenli bir bağlantı oluşturmanıza olanak sağlar. P2S bağlantısı, istemci bilgisayardan başlatılarak oluşturulur. Bu çözüm, Azure sanal ağlarına uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak isteyen uzaktan çalışan kişiler için kullanışlıdır. P2S VPN ayrıca, bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda S2S VPN yerine kullanabileceğiniz yararlı bir çözümüdür. Bu tablo Resource Manager dağıtım modelleri için geçerlidir.

## <a name="protocol"></a>Hangi protokolün P2S kullanıyor mu?

Noktadan siteye VPN aşağıdaki protokollerden birini kullanarak şunları yapabilirsiniz:

* Yuva Tünel Protokolü (SSTP), bir özel SSL tabanlı VPN Protokolü güvenliğini sağlayın. Bir SSL VPN çözümü, güvenlik duvarları, SSL kullanan TCP bağlantı noktası 443 de çoğu güvenlik duvarı açık olduğundan duvarlarından geçebildiği. SSTP yalnızca Windows cihazlarda desteklenir. Azure, SSTP (Windows 7 ve üzeri) yüklü Windows'ın tüm sürümlerini destekler.

* IKEv2 VPN, standart tabanlı bir IPsec VPN çözümüdür. IKEv2 VPN, Mac cihazlardan (OSX sürüm 10.11 ve üzeri) bağlantı kurmak için kullanılabilir.

Windows ve Mac cihazlarından oluşan bir karma istemci ortamlarını varsa, hem SSTP ve Ikev2 yapılandırın.

>[!NOTE]
>Resource Manager dağıtım modeli için yalnızca Ikev2 P2S için kullanılabilir. Klasik dağıtım modeli için kullanılabilir değil.
>

## <a name="authentication"></a>P2S VPN istemcileri nasıl doğrulanır?

Azure, P2S VPN bağlantısı kabul eder önce kullanıcının önce doğrulanması gerekir. Bağlanan kullanıcının kimliğini doğrulamak için Azure'un sunduğu iki mekanizma vardır.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Yerel Azure sertifika doğrulaması kullanarak kimlik doğrulaması

Yerel Azure sertifika kimlik doğrulaması kullanırken, cihazda mevcut olan bir istemci sertifikası bağlanan kullanıcının kimliğini doğrulamak için kullanılır. İstemci sertifikaları Güvenilen kök sertifikadan oluşturulmuş ve ardından her istemci bilgisayara yüklenir. Kurumsal bir çözüm kullanılarak oluşturulmuş bir kök sertifika kullanabilir veya otomatik olarak imzalanan bir sertifika oluşturabilirsiniz.

İstemci sertifikası doğrulama, VPN ağ geçidi tarafından gerçekleştirilir ve P2S VPN bağlantısı kurulması sırasında gerçekleşir. Kök sertifika doğrulama için gereklidir ve Azure'a karşıya yüklenmelidir.

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Active Directory (AD) etki alanı sunucusu kullanarak kimlik doğrulaması

AD etki alanı kimlik doğrulaması, kuruluş etki alanı kimlik bilgilerini kullanarak kullanıcıların Azure'a bağlamanıza olanak tanır. Bu AD server ile tümleşen bir RADIUS sunucusu gerektirir. Kuruluşlar, mevcut bir RADIUS dağıtımı da yararlanabilirsiniz.   
  RADIUS sunucusu şirket içinde dağıtılması olabilir ya da Azure vnet'inizde. Kimlik doğrulaması sırasında Azure VPN Gateway, geçiş ve ileten kimlik doğrulama iletilerinde RADIUS sunucusu ve bağlanan cihaz arasında sürekli olarak görev yapar. Bu nedenle RADIUS sunucusu ağ geçidi ulaşılabilirlik önemlidir. Daha sonra RADIUS sunucusunun mevcut şirket içi ise, azure'dan şirket içi siteye bir VPN S2S bağlantısı için erişilebilirlik gereklidir.  
  RADIUS sunucusu, AD Sertifika Hizmetleri ile de tümleştirebilirsiniz. Bu RADIUS sunucusu ve kurumsal sertifika dağıtımınızı alternatif olarak, Azure sertifika doğrulaması P2S sertifika kimlik doğrulaması için kullanmanıza olanak sağlar. Kök sertifikaları ve iptal edilen sertifikaları yüklemek gerekmez avantajlarındandır.

Bir RADIUS sunucusu, ayrıca diğer dış kimlik sistemleri ile tümleştirebilirsiniz. Bu, çok faktörlü seçenekleri dahil olmak üzere, P2S VPN için kimlik doğrulama seçenekleri bolca yukarı açar.

![Noktadan siteye](./media/point-to-site-about/p2s.png "noktadan siteye")

## <a name="what-are-the-client-configuration-requirements"></a>İstemci yapılandırma gereksinimleri nelerdir?

>[!NOTE]
>Windows istemcileri için istemci cihazı Azure VPN bağlantısını başlatmak için istemci cihaza yönetici hakları olmalıdır.
>

Kullanıcılar yerel VPN istemcileri, Windows ve Mac cihazları üzerinde P2S için kullanın. Azure, bir VPN istemcisi yapılandırma zip sağlar yerel bu istemciler tarafından Azure'a bağlanmak için gereken ayarları içeren dosya.

* Windows cihazları için VPN istemci yapılandırması kullanıcılar cihazlarına yüklemek bir yükleyici paketi oluşur.
* Mac cihazlar için kullanıcıların cihazlarında yükleme mobileconfig dosya oluşur.

Zip dosyası ayrıca, bu cihazlar için kendi profili oluşturmak için kullanabileceğiniz Azure tarafında bazı önemli ayarlar değerlerini sağlar. Bazı değerler, VPN ağ geçidi adresi, yapılandırılmış tünel türünü, yollar ve ağ geçidi doğrulaması için kök sertifikasını içerir.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="gwsku"></a>Hangi ağ geçidi SKU'ları destek P2S VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. İnternet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle garantili bir verimlilik değildir.
* Fiyatlandırma bilgileri Fiyatlandırma sayfasında bulunabilir 
* SLA (hizmet düzeyi sözleşmesi) bilgilerine SLA sayfasında bulunabilir.

>[!NOTE]
>Temel SKU, IKEv2 veya RADIUS kimlik doğrulamasını desteklemez.
>

## <a name="configure"></a>P2S bağlantısı nasıl yapılandırabilirim?

P2S yapılandırması oldukça birkaç belirli adım gerektirir. Aşağıdaki makaleler, P2S yapılandırması ve VPN istemci cihazları yapılandırmak için bağlantıları yürütmek için adımları içerir:

* [P2S bağlantısı - RADIUS kimlik doğrulamasını yapılandırma](point-to-site-how-to-radius-ps.md)

* [-Yerel Azure sertifika doğrulaması P2S bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="faqcert"></a>Yerel Azure sertifika doğrulaması hakkında SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>RADIUS kimlik doğrulaması hakkında SSS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar

* [P2S bağlantısı - RADIUS kimlik doğrulamasını yapılandırma](point-to-site-how-to-radius-ps.md)

* [-Yerel Azure sertifika doğrulaması P2S bağlantısı yapılandırma](vpn-gateway-howto-point-to-site-rm-ps.md)
