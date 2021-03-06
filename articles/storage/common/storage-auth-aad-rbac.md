---
title: Azure depolama kapsayıcıları ve Kuyruklar'ı (Önizleme) için erişim haklarını yönetmek için RBAC kullanma | Microsoft Docs
description: Azure depolama veri kullanıcıları, grupları, uygulama hizmet sorumlularını veya yönetilen hizmet kimlikleri için erişim için rolleri atamak için rol tabanlı erişim denetimi (RBA) kullanın. Azure Storage kapsayıcıları ve Kuyruklar erişim hakları için yerleşik ve özel rollerin destekler.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: tamram
ms.openlocfilehash: cee319c4fb158e95b4a6d996f846038f0654dd32
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969162"
---
# <a name="manage-access-rights-to-azure-storage-data-with-rbac-preview"></a>RBAC (Önizleme) ile Azure depolama verilere erişim haklarını yönetme

Azure Active Directory (Azure AD) ile güvenli kaynaklara erişim hakları yetkilendirir [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure depolama genel kapsayıcılar veya sıralara erişmek için kullanılan izin kümelerini kapsayacak yerleşik RBAC rolleri kümesi tanımlar. Ne zaman bir RBAC rolü atanmış bir Azure AD kimlik için kimlik bu kaynaklara erişimi verilir göre belirtilen kapsam. Abonelik, kaynak grubu, depolama hesabı veya bir kapsayıcının veya kuyruk düzeyi için erişimi sınırlayabilirsiniz. Azure portalı, Azure komut satırı araçları ve Azure Management API'leri kullanarak Azure Storage kaynakları için erişim hakları atayabilirsiniz. 

Bir Azure AD kimlik, bir kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir veya olabilir bir *yönetilen hizmet kimliği*. Bir güvenlik sorumlusu, kullanıcı, Grup veya uygulama hizmet sorumlusu olabilir. A [yönetilen hizmet kimliği](../../active-directory/managed-service-identity/overview.md) olan Azure sanal makineleri, işlev uygulamaları, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan kimliğini doğrulamak için kullanılan otomatik olarak yönetilen bir kimlik. Azure AD'de kimlik genel bakış için bkz. [anlamak Azure kimlik çözümleri](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions).

## <a name="rbac-roles-for-azure-storage"></a>RBAC rolleri için Azure depolama

Azure depolama, hem yerleşik hem de özel RBAC rollerini destekler. Azure depolama, Azure AD ile kullanmak için bu yerleşik RBAC rolleri sunar:

- [Depolama Blob verileri katkıda bulunan (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Depolama Blob verileri Okuyucu (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Depolama kuyruk verileri katkıda bulunan (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Depolama kuyruk verileri Okuyucu (Önizleme)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Hakkında daha fazla bilgi için Azure depolama için yerleşik roller tanımlanır, bkz: [rol tanımlarını anlamak](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

Kapsayıcılar ve Kuyruklar ile kullanmak için özel roller de tanımlayabilirsiniz. Daha fazla bilgi için [Azure rol tabanlı erişim denetimi için özel roller oluşturma](https://docs.microsoft.com/azure/role-based-access-control/custom-roles.md). 

> [!IMPORTANT]
> Bu önizleme, yalnızca üretim dışı kullanması için tasarlanmıştır. Azure AD tümleştirmesi için Azure depolama genel kullanıma sunulan bildirildiği kadar üretim hizmet düzeyi sözleşmeleri (SLA'lar) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyor, uygulamalarınızda paylaşılan anahtar yetkilendirme veya SAS belirteçlerini kullanmaya devam. Önizleme hakkında ek bilgi için bkz: [erişim Azure Active Directory (Önizleme) kullanarak Azure depolama için kimlik doğrulaması](storage-auth-aad.md).
>
> Önizleme sırasında RBAC rolü atamalarını yayılması için beş dakika sürebilir.

## <a name="assign-a-role-to-a-security-principal"></a>Bir güvenlik sorumlusu için rol atama

Kapsayıcıları ve kuyruk depolama hesabınızda izinler vermek için Azure kimlik bir RBAC rolü atayın. Depolama hesabı veya belirli bir kapsayıcı veya kuyruğa rol ataması kapsamını belirleyebilirsiniz. Kapsam bağlı olarak, yerleşik rolleri tarafından verilen erişim hakları aşağıdaki tabloda özetlenmiştir: 

|                                 |     BLOB verileri katkıda bulunan                                                 |     BLOB veri okuyucusu                                                |     Kuyruk verileri katkıda bulunan                                  |     Kuyruk verileri okuyucu                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    Abonelik kapsamı       |    Tüm kapsayıcılar ve bloblar aboneliği için okuma/yazma erişimi       |    Tüm kapsayıcılar ve bloblar aboneliği okuma erişimi       |    Abonelikteki tüm kuyruklar için okuma/yazma erişimi       |    Abonelikteki tüm kuyrukları okuma erişimi         |
|    Kaynak grubu kapsamında     |    Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma/yazma erişimi     |    Tüm kapsayıcılar ve bloblar kaynak grubundaki okuma erişimi     |    Kaynak grubundaki tüm kuyruklar için okuma/yazma erişimi     |    Kaynak grubundaki tüm kuyrukları okuma erişimi     |
|    Depolama hesabı için kapsamlı    |    Tüm kapsayıcıları ve blobları depolama hesabındaki okuma/yazma erişimi    |    Tüm kapsayıcıları ve blobları depolama hesabındaki okuma erişimi    |    Depolama hesabındaki tüm kuyruklar için okuma/yazma erişimi    |    Depolama hesabındaki tüm kuyrukları okuma erişimi    |
|    Kapsayıcı/sıra kapsamı    |    Belirtilen kapsayıcıya ve bloblarına okuma/yazma erişimi              |    Belirtilen kapsayıcıya ve bloblarına okuma erişimi              |    Belirtilen sıra okuma/yazma erişimi                  |    Belirtilen sıra okuma erişimi                    |

> [!NOTE]
> Bir Azure depolama hesabınızın sahibi olarak, otomatik olarak veri erişim izni atanmaz. Siz açıkça kendiniz bir RBAC rolü için Azure depolama atamanız gerekir. Aboneliğiniz, kaynak grubu, depolama hesabı veya bir kapsayıcı veya kuyruk düzeyinde atayabilirsiniz.

Azure depolama işlemleri çağırmak için gereken izinler hakkında daha fazla bilgi için bkz: [REST işlemlerini çağırmak için izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

Aşağıdaki bölümlerde, depolama hesabına kapsamlandırılan veya tek bir kapsayıcıya kapsamlı bir rol atamak gösterilmektedir.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Azure portalında depolama hesabı için kapsamlı bir rol atayın

Tüm kapsayıcıları veya Azure portalında depolama hesabındaki sıralara erişim verme yerleşik bir rol atamak için:

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin.
2. Depolama hesabınızı seçin ve ardından **erişim denetimi (IAM)** hesabı için erişim denetimi ayarlarını görüntülemek için. Tıklayın **Ekle** düğmesini yeni bir rolü ekleyin.

    ![Depolama erişim denetimi ayarları gösteren ekran görüntüsü](media/storage-auth-aad-rbac/portal-access-control.png)

3. İçinde **izinleri eklemek** penceresinde bir Azure AD kimliğine nasıl atamak için rolü seçin. Ardından bu role atamak istediğiniz kimliğini bulmak için arama yapın. Örneğin, aşağıdaki gösterir görüntüde **depolama Blob verileri Okuyucu (Önizleme)** bir kullanıcıya atanmış rol.

    ![Bir RBAC rolünün nasıl atanacağını gösteren ekran görüntüsü](media/storage-auth-aad-rbac/add-rbac-role.png)

4. **Kaydet**’e tıklayın. Rol atanmış kimliği bu rolü altında listelenen görünür. Örneğin, aşağıdaki görüntüde eklenen kullanıcıları artık depolama hesabındaki tüm blob verilerine Okuma izinlerine sahip olduğunu gösterir.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Bir kapsayıcı veya Azure portalında sıra kapsamı rol atama

Bir kapsayıcı veya bir sıra kapsamı belirlenmiş, yerleşik bir rol atamak için adımları da buradakilere benzer. Burada gösterilen yordam, bir kapsayıcı için kapsamlı bir rolü atar, ancak bir kuyruk için kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz: 

1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin ve görüntüleme **genel bakış** hesabı.
2. BLOB hizmeti altında seçin **Blob'lara göz at**. 
3. Rol atamak istediğiniz kapsayıcıyı bulun ve kapsayıcının ayarları görüntüleyin. 
4. Seçin **erişim denetimi (IAM)** kapsayıcısı için erişim denetimi ayarlarını görüntülemek için.
5. İçinde **izinleri eklemek** penceresinde bir Azure AD kimliğine nasıl atamak istediğiniz rolü seçin. Ardından bu role atamak istediğiniz kimliğini bulmak için arama yapın.
6. **Kaydet**’e tıklayın. Rol atanmış kimliği bu rolü altında listelenen görünür. Örneğin, aşağıdaki görüntüde eklenen kullanıcı artık adlı kapsayıcıyı verilere Okuma izinleri olmasına gösterilmektedir *örnek kapsayıcı*.

    ![Ekran görüntüsü gösteren bir role atanmış kullanıcıların listesi](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Sonraki Adımlar

- RBAC hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi ile çalışmaya başlama](../../role-based-access-control/overview.md).
- Atama ve Azure PowerShell, Azure CLI veya REST API ile RBAC rolü atamalarını yönetme konusunda bilgi almak için şu makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-powershell.md)
    - [Rol tabanlı erişim denetimi (RBAC), Azure CLI ile yönetme](../../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimi (RBAC) yönetme](../../role-based-access-control/role-assignments-rest.md)
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [Azure depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure kapsayıcılar ve sıralar için Azure AD tümleştirmesi hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
- 