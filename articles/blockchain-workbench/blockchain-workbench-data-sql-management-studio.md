---
title: Azure Blockchain Workbench Verilerini SQL Server Management Studio ile Kullanma
description: SQL Server Management Studio’nun içinden Azure Blockchain Workbench'in SQL Veritabanı’na bağlanmayı öğrenin.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 5c5a2e2b42cd9c19810e6579159b228369ee8f7f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969308"
---
# <a name="using-azure-blockchain-workbench-data-with-sql-server-management-studio"></a>Azure Blockchain Workbench verilerini SQL Server Management Studio ile kullanma

Microsoft SQL Server Management Studio, Azure Blockhain Workbench'in SQL Veritabanı’na yönelik olarak hızla sorgu yazma ve test etme olanağı sağlar. Bu bölüm, SQL Server Management Studio’nun içinden Azure Blockchain Workbench'in SQL Veritabanı’na nasıl bağlanılacağını gösteren adım adım bir kılavuz içerir.

## <a name="prerequisites"></a>Önkoşullar

* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)’yu indirin.

## <a name="connecting-sql-server-management-studio-to-data-in-azure-blockchain-workbench"></a>SQL Server Management Studio’yu Azure Blockchain Workbench’teki verilere bağlama

1. SQL Server Management Studio’yu açıp **Bağlan**’ı seçin.
2. **Veritabanı Altyapısı**’nı seçin.

    ![Veritabanı altyapısı](media/blockchain-workbench-data-sql-management-studio/database-engine.png)

3. **Sunucuya Bağlan** iletişim kutusuna sunucu adını ve veritabanınızın kimlik bilgilerini girin.

    Azure Blockchain Workbench dağıtım işlemi tarafından oluşturulan kimlik bilgilerini kullanıyorsanız kullanıcı adı **dbadmin**, parola ise dağıtım sırasında belirttiğiniz parola olur.

    ![SQL kimlik bilgilerini girin](media/blockchain-workbench-data-sql-management-studio/sql-creds.png)

 4. SQL Server Management Studio, Azure Blockchain Workbench veritabanındaki veritabanlarının, veritabanı görünümlerinin ve saklı yordamların listesini görüntüler.

    ![Veritabanı listesi](media/blockchain-workbench-data-sql-management-studio/db-list.png)

5. Veritabanı görünümlerinin herhangi biri ile ilişkili verileri görüntülemek için aşağıdaki adımlarla otomatik olarak bir select deyimi oluşturabilirsiniz.
6. Nesne Gezgini'nde veritabanı görünümlerinden herhangi birine sağ tıklayın.
7. **Betiği Farklı Görüntüle** seçeneğini belirleyin.
8. **Şuna kadar SEÇ:** seçeneğini belirleyin.
9. **Yeni Sorgu Düzenleyicisi Penceresi**‘ni seçin.
10. **Yeni Sorgu** seçilerek yeni bir sorgu oluşturulabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](blockchain-workbench-database-views.md)