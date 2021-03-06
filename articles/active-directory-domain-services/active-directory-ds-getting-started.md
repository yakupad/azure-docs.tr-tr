---
title: 'Azure Active Directory etki alanı Hizmetleri: Başlarken | Microsoft Docs'
description: Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: 340193f191bbdbe658769f9265f9e63844481c32
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265278"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri etkinleştir
Bu makalede, Azure Active Directory etki alanı Azure portalını kullanarak hizmetlerin (Azure AD DS) nasıl etkinleştirileceği gösterilmektedir.


## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri tamamlamak için gerekir:

* Geçerli bir **Azure aboneliği**.
* Bir **Azure AD dizini** -ya da bir şirket içi dizin veya bir yalnızca bulut dizini ile eşitlenir.
* **Azure aboneliğine Azure AD dizini ile ilişkili olmalıdır**.
* Gereksinim duyduğunuz **genel yönetici** Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek için Azure AD dizininizi ayrıcalıklar.


## <a name="enable-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri'ni etkinleştirme

Başlatmak için **etkinleştirmek Azure AD etki alanı Hizmetleri** Sihirbazı, aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com) gidin.
2. Sol bölmede **Kaynak oluştur**’a tıklayın.
3. İçinde **yeni** sayfasında, **etki alanı Hizmetleri** arama çubuğunu içine.

    ![Etki Alanı Hizmetleri için arama](./media/getting-started/search-domain-services.png)

4. Seçmek için tıklatın **Azure AD etki alanı Hizmetleri** arama önerileri listesinden. Üzerinde **Azure AD etki alanı Hizmetleri** sayfasında, **oluşturma** düğmesi.

    ![Etki Alanı Hizmetleri görüntüle](./media/getting-started/domain-services-blade.png)

5. **Etkinleştirmek Azure AD etki alanı Hizmetleri** Sihirbazı başlatıldığında.


## <a name="task-1-configure-basic-settings"></a>Görev 1: temel ayarlarını yapılandırma
İçinde **Temelleri** Sayfa Sihirbazı'nın yönetilen etki alanı için DNS etki alanı adı belirtin. Ayrıca, yönetilen etki alanı dağıtılmalıdır Azure konumu ve kaynak grubu seçebilirsiniz.

![Temel yapılandırma](./media/getting-started/domain-services-blade-basics.png)

1. Seçin **DNS etki alanı adı** yönetilen etki alanınız için.

   > [!NOTE]
   > **Bir DNS etki alanı adı seçmek için yönergeler**
   > * **Yerleşik etki alanı adı:** varsayılan olarak, sihirbaz varsayılan/dahili-içeren etki alanı adını dizini belirtir (ile bir **. onmicrosoft.com** soneki) sizin için. Yönetilen etki alanına güvenli LDAP erişim Internet üzerinden etkinleştirmeyi seçerseniz, ortak bir DNS kaydı oluşturma veya bu etki alanı adı için bir ortak CA güvenli LDAP sertifika alma sorunları bekler. Microsoft sahibi *. onmicrosoft.com* etki alanı ve CA'ları bu etki alanı için doğrulanmasından sertifikaları cihaza değil.
   * **Özel etki alanı adları:** bir özel etki alanı adı da yazabilirsiniz. Bu örnekte özel etki alanı adı *contoso100.com* şeklindedir.
   * **Yönlendirilebilir olmayan etki alanı sonekleri:** genellikle yönlendirilebilir olmayan etki alanı adı soneki önleme öneririz. Örneğin, bir etki alanı DNS etki alanı adı 'contoso.local' ile oluşturmamak iyidir. '.Local' DNS soneki yönlendirilebilir ve DNS çözümlemesi ile sorunlara neden olabilir.
   * **Etki alanı ön eki sınırlamaları:** belirtilen etki alanı adınızı önek (örneğin, *contoso100* içinde *contoso100.com* etki alanı adı) 15 veya daha az karakter içermelidir. 15 karakterden daha uzun bir önek ile yönetilen bir etki alanı oluşturamazsınız.
   * **Ağ adı çakışmalarını:** yönetilen etki alanı için seçtiğiniz DNS etki alanı adı sanal ağda zaten mevcut olduğundan emin olun. Özellikle, denetleme olup olmadığını:
       * Sanal ağda aynı DNS etki alanı adı içeren bir Active Directory etki alanı zaten var.
       * Yönetilen etki alanını etkinleştirmek planladığınız sanal ağ ile şirket içi ağınıza bir VPN bağlantısı vardır. Bu senaryoda, şirket içi ağınızda aynı DNS etki alanı adına sahip bir etki alanı olmadığından emin olun.
       * Sanal ağda bu ada sahip bir bulut hizmetiniz zaten varsa.
    >

2. Azure seçin **abonelik** , yönetilen etki alanı oluşturmak istediğinizi içinde.

3. Seçin **kaynak grubu** için yönetilen etki alanına ait olması gerekir. Ya da seçin **Yeni Oluştur** veya **var olanı kullan** seçenekleri kaynak grubunu seçin.

4. Azure seçin **konumu** , yönetilen etki alanında oluşturulmalıdır. Üzerinde **ağ** sayfasında sihirbazın, seçtiğiniz konuma ait yalnızca sanal ağlar görürsünüz.

5. Tıklatın **Tamam** üzerinde taşımayı **ağ** Sihirbazı sayfası.


## <a name="next-step"></a>Sonraki adım
[2. Görev: Ağ ayarlarını yapılandırma](active-directory-ds-getting-started-network.md)
