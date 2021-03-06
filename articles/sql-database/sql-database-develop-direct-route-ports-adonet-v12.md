---
title: SQL veritabanı için 1433 dışındaki bağlantı noktaları | Microsoft Docs
description: Azure SQL veritabanı ADO.NET'ten istemci bağlantılarını proxy sunucusunu atlamak ve 1433 dışındaki bağlantı noktaları kullanarak doğrudan veritabanını etkileşim.
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: a8c9eef968465ecf9c8a29df471955b89f3585a0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645098"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları
Bu konu, ADO.NET 4.5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışını tanımlar. 

> [!IMPORTANT]
> Bağlantı mimarisi hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı bağlantısı mimarisi](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Dış vs içinde
Azure SQL veritabanı bağlantıları için biz ilk istemci programınız çalışıp çalışmayacağını başvurmalısınız *dışında* veya *içinde* Azure bulut sınır. Alt bölümlerde iki yaygın senaryolar açıklanmaktadır.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Dış:* istemcisi, masaüstü bilgisayarınızda çalıştırır
Bağlantı noktası 1433 SQL veritabanı istemci uygulamanızı barındıran masaüstü bilgisayarınızda açık olması gerekir tek bağlantı noktasıdır.

#### <a name="inside-client-runs-on-azure"></a>*İç:* istemci Azure üzerinde çalışır
İstemcinizin Azure bulut sınırının içinde çalıştığında, veririz kullandığı bir *doğrudan rota* SQL veritabanı sunucusu ile etkileşim. Daha fazla bağlantı kurulduktan sonra istemci ve veritabanı arasındaki etkileşimler Azure SQL veritabanı ağ geçidi içerir.

Sıra aşağıdaki gibidir:

1. ADO.NET 4.5 (veya üzeri) Azure bulut kısa bir etkileşim başlatır ve dinamik olarak tanımlanan bağlantı noktası numarasını alır.
   
   * Dinamik olarak tanımlanan bağlantı noktası numarasını 11000 11999 veya 14000 14999 aralıktır.
2. ADO.NET ardından SQL veritabanı sunucusuna doğrudan hiçbir Ara yazılımla arasında bağlanır.
3. Sorgular veritabanına doğrudan gönderilir ve sonuçları doğrudan istemciye döndürülür.

11000 11999 ve Azure İstemci makinenizde 14000 14999 aralıklarına SQL veritabanı ile ADO.NET 4.5 istemci etkileşimler için kullanılabilir kalan bağlantı noktasının emin olun.

* Özellikle, aralığındaki bağlantı noktalarına diğer giden blockers boş olması gerekir.
* Azure VM'nizi üzerinde **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** bağlantı noktası ayarlarını denetler.
  
  * Kullanabileceğiniz [güvenlik duvarının kullanıcı arabirimi](http://msdn.microsoft.com/library/cc646023.aspx) belirtmek için bir kural eklemek için **TCP** protokolü bir bağlantı noktası aralığı sözdizimi ile birlikte ister **11000 11999**.

## <a name="version-clarifications"></a>Sürüm açıklamalar
Bu bölümde ürün sürümleri başvurmak adlar açıklar. Ayrıca, bazı ürünler arasındaki sürümleri eşleştirmelerini listeler.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 TDS 7.3 protokolü, ancak değil 7.4 destekler.
* ADO.NET 4.5 ve sonraki TDS 7.4 protokolünü destekler.

## <a name="related-links"></a>İlgili bağlantılar
* ADO.NET 4.6 20 Temmuz 2015 tarihinde yayımlanmıştır. .NET ekibi Web günlüğü duyurudan kullanılabilir [burada](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 15 Ağustos 2012'de serbest bırakıldı. .NET ekibi Web günlüğü duyurudan kullanılabilir [burada](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * ADO.NET 4.5.1 hakkında bir blog gönderisini kullanılabilir [burada](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).
* [TDS Protokolü sürüm listesi](http://www.freetds.org/userguide/tdshistory.htm)
* [SQL veritabanı geliştirme genel bakış](sql-database-develop-overview.md)
* [Azure SQL veritabanı güvenlik duvarı](sql-database-firewall-configure.md)
* [Nasıl yapılır: SQL veritabanında güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md)

