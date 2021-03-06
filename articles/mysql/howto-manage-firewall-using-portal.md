---
title: Oluşturma ve MySQL güvenlik duvarı kurallarında Azure veritabanı için MySQL yönetme
description: Oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure Portalı'nı kullanarak yönetme
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: f7d2d97049d73387f44f55bbd2fb90a6174a9df2
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265869"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Azure veritabanı için MySQL güvenlik duvarı kurallarını yönetme
Belirtilen IP adresi veya bir IP adresi aralığı MySQL sunucusu için bir Azure veritabanına erişmek yöneticiler sunucu düzeyinde güvenlik duvarı kuralları etkinleştirin. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. MySQL sunucusu sayfasında, ayarları altında başlığını tıklatın **bağlantı güvenliği** bağlantı güvenlik sayfası Azure veritabanı için MySQL için açın.

   ![Azure portal - bağlantı güvenliği](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Tıklatın **eklemek IP** araç çubuğunda. Bu otomatik olarak bir güvenlik duvarı kuralı, bilgisayarınızın ortak IP adresiyle Azure sistem tarafından algılanan olarak oluşturur.

   ![Azure portal - My IP Ekle'yi tıklatın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portal tarafından gözlenen IP adresi internet ve Azure sunucuları erişirken kullanılan IP adresinden farklıdır. Bu nedenle, başlangıç IP ve bitiş IP beklendiği gibi kural işlevi yapmak için değiştirmeniz gerekebilir.

   Kendi IP adresini denetlemek için bir arama motoru veya diğer çevrimiçi aracı kullanın. Örneğin, "IP adresimi nedir" arayın. 

   ![My IP nedir için Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıklarını ekleyin. MySQL için Azure veritabanı için güvenlik duvarı kurallarında tek bir IP adresi veya adres aralığı belirtebilirsiniz. Kural tek bir IP adresi için sınırlamak istiyorsanız, IP başlangıç ve bitiş IP alanları aynı adresini yazın. Güvenlik Duvarı'nı açarak, Yöneticiler, kullanıcılar ve geçerli kimlik bilgilerine sahip oldukları MySQL server üzerinde herhangi bir veritabanına erişmek için uygulama sağlar.

   ![Azure portal - güvenlik duvarı kuralları ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Tıklatın **kaydetmek** bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda. Güvenlik duvarı kurallarının güncelleştirmenin başarılı olduğunu onaylanmasını bekleyin.

   ![Azure portal - Kaydet](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
MySQL sunucusu için Azure veritabanınıza bağlanmak üzere Azure uygulamalardan izin vermek için Azure bağlantıları etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure VM içinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi'nden bağlanma. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** için seçenek **ON** Portalı'nda **bağlantı güvenliği** bölmesinde ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, Azure veritabanı MySQL sunucusu için istek ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Azure portalını kullanarak var olan sunucu düzeyinde güvenlik duvarı kurallarını yönet
Güvenlik duvarı kurallarını yönetmek için gereken adımları yineleyin.
* Geçerli bilgisayar eklemek için tıklatın **+ IP eklemek**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Ek IP adresleri eklemek için yazın **kural adı**, **başlangıç IP**, ve **bitiş IP**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuralı alanlarında birini tıklatın ve ardından değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için üç nokta [...]'ı tıklatın ve ardından **silmek**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure CLI kullanarak yönetme](howto-manage-firewall-using-cli.md).
- MySQL sunucusu için bir Azure veritabanına bağlanmada daha fazla yardım için bkz: [Azure veritabanı için MySQL için bağlantı kitaplıkları](./concepts-connection-libraries.md)
