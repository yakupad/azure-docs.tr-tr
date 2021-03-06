---
title: Azure veritabanı için MySQL verileri, çoğaltma saklı yordamlar
description: Bu makalede, verileri, çoğaltma için kullanılan tüm depolanmış yordamları sunar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: 2d62cd693d7a67faf836c645f8bd33de9afca949
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266117"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Azure veritabanı için MySQL verileri, çoğaltma saklı yordamlar

Verileri, çoğaltma, şirket içi, sanal makine ya da MySQL hizmeti Azure veritabanına diğer bulut sağlayıcıları tarafından barındırılan veritabanı Hizmetleri çalışan bir MySQL sunucusu verileri eşitlemenize olanak tanır.

Aşağıdaki saklı yordamları ayarlayın veya verileri, birincil ve çoğaltma arasında çoğaltmayı kaldırmak için kullanılır.

|**Saklı yordam adı**|**Giriş parametreleri**|**Çıktı parametreleri**|**Kullanım notu**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_primary*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL modu ile veri aktarımı için CA sertifikanın bağlamı master_ssl_ca parametresine geçirin. </br><br>SSL olmadan veri aktarmak için boş bir dize master_ssl_ca parametresine geçirin.|
|*MySQL.az_replication _sihirbaz Başlat*|Yok|Yok|Çoğaltma başlatır.|
|*MySQL.az_replication _stop*|Yok|Yok|Çoğaltma durur.|
|*MySQL.az_replication _remove_primary*|Yok|Yok|Birincil ve çoğaltma arasındaki çoğaltma ilişkisini kaldırır.|
|*MySQL.az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatası atlar.|

Verileri, birincil ve çoğaltma Azure veritabanındaki arasındaki MySQL için çoğaltma ayarlamak için başvurmak [verileri, çoğaltmayı yapılandırmak nasıl](howto-data-in-replication.md).