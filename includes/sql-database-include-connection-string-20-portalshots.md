---
title: Azure portaldan bağlantı dizesini alın
description: Azure portaldan bağlantı dizesini alın
keywords: SQL bağlantı dizesi
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: dab7623c86bea4e562313e618f238b9b33c0fdc5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117299"
---
### <a name="obtain-the-connection-string-from-the-azure-portal"></a>Azure portaldan bağlantı dizesini alın
Kullanım [Azure portalında](https://portal.azure.com/) Azure SQL veritabanı ile etkileşim kurmak istemci programınız için gerekli olan bağlantı dizesini edinme.

1. Seçin **tüm hizmetleri** > **SQL veritabanları**.

2. Veritabanınızı yakın sol üst kısmındaki filtre metin kutusuna girin **SQL veritabanları** dikey penceresi.

3. Veritabanınız için satırı seçin.

4. Veritabanınızın görsel kolaylık belirli dikey göründükten sonra **simge durumuna küçült** göz atma ve veritabanı filtreleme için kullanılan dikey pencereleri daraltmak için düğmeler.

5. Veritabanı dikey penceresinde seçin **veritabanı bağlantı dizelerini Göster**.

6. Uygun bağlantı dizesini kopyalayın. ADO.NET bağlantı kitaplığını kullanmak istiyorsanız, uygun dize ile yani kopyalama **ADO.NET** sekmesi.

    ![Veritabanınız için ADO bağlantı dizesini kopyalayın][20-CopyAdoConnectionString]

7. Bağlantı dizesi, gerektiği gibi düzenleyin. yani parolanızı bağlantı dizesi Ekle veya Kaldır "@&lt;servername&gt;" kullanıcı adı veya sunucu adı çok uzun olması durumunda username kullanıcı adından.

8. Bir biçim veya başka bağlantı dizesi bilgilerini istemci program kodunuza yapıştırın.

Daha fazla bilgi için [bağlantı dizeleri ve yapılandırma dosyalarını](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->



[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
