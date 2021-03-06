---
title: RBAC ve REST API - Azure kullanarak erişimini yönetme | Microsoft Docs
description: Kullanıcılar, gruplar ve rol tabanlı erişim denetimi (RBAC) ve REST API kullanarak uygulamalar için erişimi yönetmeyi öğrenin. Buna erişimi listeleme, erişim verme ve erişimi kaldırma işlemleri dahildir.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 859a410a4ff9204e8e52fbd2cc3b38823f4bb830
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435227"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>RBAC ve REST API kullanarak erişimini yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Bu makalede, kullanıcıları, grupları ve RBAC ve REST API kullanarak uygulamalar için erişimi nasıl yönetileceği açıklanmaktadır.

## <a name="list-access"></a>Erişimi listeleme

Liste erişim, RBAC, rol atamalarını listeleyin. Rol atamalarını listesinde, aşağıdakilerden birini kullanmak için [rol atamaları: liste](/rest/api/authorization/roleassignments/list) REST API'leri. Sonuçlarınızı daraltmak için bir kapsam ve isteğe bağlı bir filtre belirtin. API'yi çağırmak için erişiminiz olması gerekir `Microsoft.Authorization/roleAssignments/read` belirtilen kapsamda işlemi. Birkaç [yerleşik roller](built-in-roles.md) bu işlem için erişim izni verilir.

1. Aşağıdaki isteği başlatın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde değiştirin *{kapsamı}* rol atamalarını listelemek istediğiniz kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{filter}* rol ataması listeyi filtrelemek için uygulamak istediğiniz koşulu.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=atScope()` | Rol atamaları subscopes olarak dahil değil yalnızca belirtilen kapsam için rol atamalarını listeleyin. |
    | `$filter=principalId%20eq%20'{objectId}'` | Belirtilen kullanıcı, Grup veya hizmet sorumlusu için rol atamalarını listeleyin. |
    | `$filter=assignedTo('{objectId}')` | Olanları gruplardan devralınan dahil olmak üzere belirli bir kullanıcı için rol atamalarını listeleyin. |

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir. Bir rol ataması oluşturmak için kullanın [rol atamaları - oluşturma](/rest/api/authorization/roleassignments/create) REST API ve güvenlik sorumlusu, rol tanımı ve kapsam belirtin. Bu API'yi çağırmak için erişiminiz olması gerekir `Microsoft.Authorization/roleAssignments/write` işlemi. Yerleşik rollerin, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim izni verilir.

1. Kullanım [rol tanımları: liste](/rest/api/authorization/roledefinitions/list) REST API veya bkz [yerleşik roller](built-in-roles.md) atamak istediğiniz rol tanımı tanımlayıcısı alınamıyor.

1. Rol ataması tanımlayıcısı için kullanılacak bir benzersiz tanımlayıcısını oluşturmak için bir GUID aracını kullanın. Tanımlayıcı şu biçimdedir: `00000000-0000-0000-0000-000000000000`

1. Aşağıdaki isteği ve gövde ile başlayın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. URI içinde değiştirin *{kapsamı}* rol ataması kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{roleAssignmentName}* ile rol atamasını GUID tanımlayıcısı.

1. İstek gövdesi içinde değiştirin *{Subscriptionıd}* , abonelik tanımlayıcısı ile.

1. Değiştirin *{Roledefinitionıd}* , rol tanımı tanımlayıcısı.

1. Değiştirin *{Principalıd}* kullanıcı, Grup veya rol atanmış bir hizmet sorumlusu nesne tanımlayıcısına sahip.

## <a name="remove-access"></a>Erişimi kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir. Bir rol atamasını kaldırmak için [Rol Atamaları - Sil](/rest/api/authorization/roleassignments/delete) REST API. Bu API'yi çağırmak için erişiminiz olması gerekir `Microsoft.Authorization/roleAssignments/delete` işlemi. Yerleşik rollerin, yalnızca [sahibi](built-in-roles.md#owner) ve [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) bu işlem için erişim izni verilir.

1. Rol ataması tanıtıcısı (GUID) alın. Bu tanımlayıcı, ilk rol ataması oluşturma veya rol atamaları listeleyerek elde edeceğinizi döndürülür.

1. Aşağıdaki isteği başlatın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. URI içinde değiştirin *{kapsamı}* rol atamasını kaldırmak için kapsama sahip.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. Değiştirin *{roleAssignmentName}* ile rol atamasını GUID tanımlayıcısı.

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
- [REST API kullanarak özel roller oluşturma](custom-roles-rest.md)