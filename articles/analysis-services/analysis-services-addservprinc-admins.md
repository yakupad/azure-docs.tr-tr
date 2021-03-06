---
title: Azure Analysis Services sunucu yöneticisi rolüne hizmet sorumlusu ekleme | Microsoft Docs
description: Sunucu Yöneticisi rolüne bir Otomasyon hizmet sorumlusu ekleme hakkında bilgi edinin
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 779a202fccd3ff56c174ebc1ebbf3c4adfdd8c7b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441748"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Sunucu Yöneticisi rolüne hizmet sorumlusu ekleme 

 Katılımsız PowerShell görevleri otomatikleştirmek için bir hizmet sorumlunuz olmalıdır **yöneticisine** ayrıcalıkları yönetilen Analysis Services sunucusunda. Bu makalede, bir Azure AS sunucusuna sunucu yöneticileri rolü için bir hizmet sorumlusu eklemeyi açıklar.

## <a name="before-you-begin"></a>Başlamadan önce
Bu görevi gerçekleştirmeden önce Azure Active Directory'de kayıtlı bir hizmet sorumlusu olması gerekir.

[Hizmet sorumlusu - Azure portal'ı oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Gerekli izinler
Bu görevi tamamlamak için olmalıdır [yöneticisine](analysis-services-server-admins.md) Azure AS sunucu üzerindeki izinleri. 

## <a name="add-service-principal-to-server-administrators-role"></a>Sunucu yöneticileri rolü için hizmet sorumlusu ekleme

1. SSMS'de, Azure AS sunucusuna bağlanın.
2. İçinde **sunucu özellikleri** > **güvenlik**, tıklayın **Ekle**.
3. İçinde **bir kullanıcı veya Grup Seç**, kayıtlı uygulamanız tarafından adı seçin ve ardından arama **Ekle**.

    ![Hizmet sorumlusu hesabı arayın](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Hizmet sorumlusu hesabı Kimliğini doğrulayın ve ardından **Tamam**.
    
    ![Hizmet sorumlusu hesabı arayın](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> AzureRm cmdlet'leri kullanarak sunucu işlemleri için hizmet sorumlusu çalışan Zamanlayıcı da ait olmanız gerekir **sahibi** rolü için kaynak [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS'yi indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


