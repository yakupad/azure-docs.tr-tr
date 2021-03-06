---
title: Azure Machine Learning modeli yönetimi için Docker görüntü oluşturma | Microsoft Docs
description: Bu makalede, web hizmetiniz için bir Docker görüntüsü oluşturma adımları açıklanmaktadır.
services: machine-learning
author: chhavib
ms.author: chhavib
manager: hjerez
editor: jasonwhowell
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 233ae50246619c3e503e42081c3b4de88090f411
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34835041"
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Azure Machine Learning modeli yönetim hesabı API'si başvurusu

Dağıtım ortamı ayarlama hakkında daha fazla bilgi için bkz: [Model yönetim hesabı kurulumu](deployment-setup-configuration.md).

Azure Machine Learning modeli yönetim hesabı API'si aşağıdaki işlemleri gerçekleştirir:

- Modeli kaydı
- Bildirim oluşturma
- Docker görüntüsü oluşturma
- Web hizmeti oluşturma

Bu görüntü, yerel veya uzak bir Azure kapsayıcı hizmeti kümesi ya da tercih ettiğiniz başka bir Docker desteklenen ortamını üzerinde bir web hizmeti oluşturmak için kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Yükleme adımlarını gitti olduğundan emin olun [yükleyin ve hızlı başlangıç oluşturmak](../service/quickstart-installation.md) belge.

Devam etmeden önce aşağıdakiler gereklidir:
1. Yönetim hesabı sağlama modeli
2. Dağıtma ve yönetme modelleri ortamı oluşturma
3. Bir Machine Learning modeli

### <a name="azure-ad-token"></a>Azure AD simgesi
Azure CLI kullanırken kullanarak oturum açmasına `az login`. CLI .azure dosyasından, Azure Active Directory (Azure AD) belirteci kullanır. API'ları kullanmak istiyorsanız, aşağıdaki seçeneklere sahipsiniz.

##### <a name="acquire-the-azure-ad-token-manually"></a>Azure AD belirteci el ile Al
Kullanabileceğiniz `az login` ve en son, giriş dizininize .azure dosyasından belirteci alma.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Program aracılığıyla Azure AD belirteci alma
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Hizmet sorumlusu oluşturduktan sonra çıkış kaydedin. Şimdi, Azure AD'den bir belirteç almak üzere kullanabilirsiniz:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Bir yetkilendirme üstbilgisinde API çağrıları için belirteç alın.


## <a name="register-a-model"></a>Bir model Kaydet

Model kayıt adımı, Machine Learning modelini oluşturduğunuz Azure Model yönetim hesabı ile kaydeder. Bu kayıt modelleri ve kayıt aynı anda atanır sürümlerine izleme sağlar. Kullanıcı modeli adı sağlar. Yeni sürüm ve kimliği aynı adla modellerin sonraki kayıt oluşturur

### <a name="request"></a>İstek

| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Açıklama
Bir model kaydeder.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| model | body | Bir model kaydetmek için kullanılan yükü. | Evet | [modeli](#model) |


### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Tamam. Modeli kaydı başarılı oldu. | [modeli](#model) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Sorgu bir hesap modellerinde listesi
### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models 
 |
### <a name="description"></a>Açıklama
Bir hesap modellerinde listesini sorgular. Sonuç listesi etiketi ve adına göre filtre uygulayabilirsiniz. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm modelleri listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| ad | sorgu | Nesne adı. | Hayır | dize |
| etiket | sorgu | Model etiketi. | Hayır | dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedModelList](#paginatedmodellist) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-model-details"></a>Model ayrıntıları alma
### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{id}  
 |

### <a name="description"></a>Açıklama
Kimliğe göre bir modeli alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [modeli](#model) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Bir bildirim kayıtlı modeli ve tüm bağımlılıkları ile kaydetme

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Açıklama
Bir bildirim kayıtlı modeli ve tüm bağımlılıklarını kaydeder.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| manifestRequest | body | Bir bildirim kaydetmek için kullanılan yükü. | Evet | [Bildirimi](#manifest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Bildirim kayıt başarılı oldu. | [Bildirimi](#manifest) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Sorgu bildirimleri bir hesap listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests | 

### <a name="description"></a>Açıklama
Bir hesap bildirimleri listesini sorgular. Model kimliği sonuç listeyi filtrelemek ve yayılma adı. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm bildirimler listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| modelId | sorgu | Model kimliği | Hayır | dize |
| ManifestName | sorgu | Bildirim adı. | Hayır | dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedManifestList](#paginatedmanifestlist) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-manifest-details"></a>Bildirim ayrıntıları

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bildirimini alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Bildirimi](#manifest) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="create-an-image"></a>Görüntü oluştur

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Açıklama
Bir görüntü, Azure kapsayıcı kayıt defterinde Docker görüntü olarak oluşturur.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| imageRequest | body | Bir görüntü oluşturmak için kullanılan yükü. | Evet | [imageRequest](#imagerequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. GET çağrı görüntü oluşturma görevinin durumunu gösterir. | İşlemi konumu |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Sorgu bir hesap da görüntü listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images | 

### <a name="description"></a>Açıklama
Bir hesap görüntülerinde listesini sorgular. Sonuç listesi bildirim kimliği ve adına göre filtre uygulayabilirsiniz. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm görüntüleri listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| Bildirim kimliği | sorgu | Bildirim kimliği. | Hayır | dize |
| ManifestName | sorgu | Bildirim adı. | Hayır | dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedImageList](#paginatedimagelist) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-image-details"></a>Görüntü ayrıntıları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bir görüntü alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Görüntü Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Görüntü](#image) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |


## <a name="create-a-service"></a>Hizmet oluşturma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Açıklama
Bir hizmeti bir görüntüden oluşturur.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| Servicerequest'te | body | Bir hizmet oluşturmak için kullanılan yükü. | Evet | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. GET çağrı hizmet oluşturma görevinin durumunu gösterir. | İşlemi konumu |
| 409 | Belirtilen ada sahip bir hizmet zaten var. |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Sorgu bir hesap hizmetlerin listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services | 

### <a name="description"></a>Açıklama
Bir hesap hizmetlerin listesini sorgular. Model adı/kimliği, bildirim ad/kimliği, görüntü kimliği, hizmet adı veya Machine Learning işlem kaynak kimliği sonuç listesini filtreleyebilirsiniz Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm hizmetleri listeler. Döndürülen liste anlatır ve her sayfadaki öğelerin sayısını isteğe bağlı bir parametredir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| ServiceName | sorgu | Hizmet adı. | Hayır | dize |
| modelId | sorgu | Model adı. | Hayır | dize |
| modelName | sorgu | Model kimliği | Hayır | dize |
| Bildirim kimliği | sorgu | Bildirim kimliği. | Hayır | dize |
| ManifestName | sorgu | Bildirim adı. | Hayır | dize |
| imageId | sorgu | Görüntü Kimliği | Hayır | dize |
| computeResourceId | sorgu | Machine Learning işlem kaynak kimliği | Hayır | dize |
| sayı | sorgu | Bir sayfa almak için öğe sayısı. | Hayır | dize |
| $skipToken | sorgu | Sonraki sayfaya almak için devamlılık belirteci. | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [PaginatedServiceList](#paginatedservicelist) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse) |

## <a name="get-service-details"></a>Hizmet Ayrıntıları

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Açıklama
Kimliğe göre bir hizmetini alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [ServiceResponse](#serviceresponse) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="update-a-service"></a>Bir hizmeti güncelleştir

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| PUT |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Açıklama
Var olan bir hizmeti güncelleştirir.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| serviceUpdateRequest | body | Var olan bir hizmeti güncelleştirmek için kullanılan yükü. | Evet |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Üst bilgiler | Şema |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Zaman uyumsuz işlemi konum URL'si. GET çağrı güncelleştirme hizmeti görevin durumunu gösterir. | İşlemi konumu |
| 404 | Belirtilen Kimliğe sahip hizmet yok. |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="delete-a-service"></a>Bir hizmeti silin

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| DELETE |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id} | 

### <a name="description"></a>Açıklama
Bir hizmet siler.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Nesne Kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. |  |
| 204 | Belirtilen Kimliğe sahip hizmet yok. |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-service-keys"></a>Hizmet anahtarı alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{id}/keys | 

### <a name="description"></a>Açıklama
Hizmet anahtarları alır.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Hizmet kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [AuthKeys](#authkeys)
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="regenerate-service-keys"></a>Hizmet anahtarlarını yeniden oluştur

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| POST |  /api/subscriptions / {Subscriptionıd} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} /services/ {id} / regenerateKeys | 

### <a name="description"></a>Açıklama
Hizmet anahtarlarını yeniden oluşturur ve bunları döndürür.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Hizmet kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| regenerateKeyRequest | body | Var olan bir hizmeti güncelleştirmek için kullanılan yükü. | Evet | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [AuthKeys](#authkeys)
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Sorgu bir hesapta dağıtımlarını listesi

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments | 

### <a name="description"></a>Açıklama
Bir hesapta dağıtımlarını listesini sorgular. Sonuç listesi belirli bir hizmet için oluşturulan dağıtımlarda döndürülecek hizmeti Kimliğine göre filtreleyebilirsiniz. Hiçbir filtre aktarılırsa, sorgu hesaptaki tüm dağıtımlar listeler.

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |
| serviceId | sorgu | Hizmet kimliği | Hayır | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [DeploymentList](#deploymentlist) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-deployment-details"></a>Dağıtım ayrıntıları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/deployments/{id} | 

### <a name="description"></a>Açıklama
Kimliği dağıtılmasının alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | Dağıtım kimliği | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [Dağıtım](#deployment) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)

## <a name="get-operation-details"></a>İşlem ayrıntıları alma

### <a name="request"></a>İstek
| Yöntem | İstek URI'si |
|------------|------------|
| GET |  /api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/operations/{id} | 

### <a name="description"></a>Açıklama
İşlemi kimliğe göre zaman uyumsuz işlem durumunu alır

### <a name="parameters"></a>Parametreler
| Ad | Konum: | Açıklama | Gerekli | Şema
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | yol | Azure abonelik kimliği | Evet | dize |
| resourceGroupName | yol | Model yönetim hesabının bulunduğu kaynak grubunun adı. | Evet | dize |
| accountName | yol | Model yönetim hesabının adı. | Evet | dize |
| id | yol | İşlem kimliği. | Evet | dize |
| API sürümü | sorgu | Microsoft.Machine.Learning kaynak Sağlayıcısı'nı kullanmak için API sürümü. | Evet | dize |
| Yetkilendirme | üst bilgi | Yetkilendirme belirteci. "Bearer XXXXXX." şöyle olmalıdır | Evet | dize |

### <a name="responses"></a>Yanıtlar
| Kod | Açıklama | Şema |
|--------------------|--------------------|--------------------|
| 200 | Başarılı. | [OperationStatus](#asyncoperationstatus) |
| default | İşlemi neden başarısız olduğunu belirten hata yanıtı. | [ErrorResponse](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Tanımlar

<a name="asset"></a>
### <a name="asset"></a>Varlık
Docker görüntü oluşturma sırasında gerekli varlık nesnesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kimliği**  <br>*İsteğe bağlı*|Varlık Kimliği|dize|
|**mime türü**  <br>*İsteğe bağlı*|Model içeriğin MIME türü. MIME türü hakkında daha fazla bilgi için bkz: [IANA medya türleri listesini](https://www.iana.org/assignments/media-types/media-types.xhtml).|dize|
|**paketini açma**  <br>*İsteğe bağlı*|Burada Docker görüntü oluşturma sırasında içerik paketinden ihtiyacımız gösterir.|boole|
|**URL**  <br>*İsteğe bağlı*|Varlık konum URL'si.|dize|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Zaman uyumsuz işlem durumu.

*Tür*: enum (NotStarted, çalışan, iptal edildi, başarılı, başarısız)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
İşlem durumu.


|Ad|Açıklama|Şema|
|---|---|---|
|**createdTime**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Zaman uyumsuz işlemi oluşturma saati (UTC).|dize (tarih)|
|**endTime**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Zaman uyumsuz işlemi bitiş zamanı (UTC).|dize (tarih)|
|**Hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|
|**Kimliği**  <br>*İsteğe bağlı*|Zaman uyumsuz işlem kimliği|dize|
|**OperationType**  <br>*İsteğe bağlı*|Zaman uyumsuz işlem türü.|Enum (görüntüsü, hizmet)|
|**resourceLocation**  <br>*İsteğe bağlı*|Kaynak veya zaman uyumsuz işlemin güncelleştirilemedi.|dize|
|**Durumu**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Bir hizmet için kimlik doğrulama anahtarları.


|Ad|Açıklama|Şema|
|---|---|---|
|**primaryKey**  <br>*İsteğe bağlı*|Birincil anahtar.|dize|
|**secondaryKey**  <br>*İsteğe bağlı*|İkincil anahtar.|dize|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Autoscaler yönelik ayarlar.


|Ad|Açıklama|Şema|
|---|---|---|
|**autoscaleEnabled**  <br>*İsteğe bağlı*|Etkinleştirmek veya devre dışı autoscaler.|boole|
|**maxReplicas**  <br>*İsteğe bağlı*|En fazla ölçeklendirmek için pod çoğaltmaları sayısının üst sınırı.  <br>**En düşük değer**: `1`|integer|
|**minReplicas**  <br>*İsteğe bağlı*|Aşağı ölçeklendirmek için pod çoğaltmaları minimum sayısı.  <br>**En düşük değer**: `0`|integer|
|**refreshPeriodInSeconds**  <br>*İsteğe bağlı*|Otomatik ölçeklendirmeyi tetikleyici süredir yenileyin.  <br>**En düşük değer**: `1`|integer|
|**targetUtilization**  <br>*İsteğe bağlı*|Otomatik ölçeklendirmeyi tetikler kullanım yüzdesi.  <br>**En düşük değer**: `0`  <br>**En büyük değer**: `100`|integer|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Machine Learning işlem kaynağı.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kimliği**  <br>*İsteğe bağlı*|Kaynak Kimliği|dize|
|**type**  <br>*İsteğe bağlı*|Kaynak türü.|Enum (küme)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Kaynaklar küme içindeki bir kapsayıcı için ayrılacak yapılandırması.


|Ad|Açıklama|Şema|
|---|---|---|
|**CPU**  <br>*İsteğe bağlı*|CPU ayırma belirtir. Kubernetes biçimi: bkz [anlamı, CPU](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|dize|
|**Bellek**  <br>*İsteğe bağlı*|Bellek ayırma belirtir. Kubernetes biçimi: bkz [bellek anlamını](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|dize|


<a name="deployment"></a>
### <a name="deployment"></a>Dağıtım
Bir Azure Machine Learning dağıtımının bir örneği.


|Ad|Açıklama|Şema|
|---|---|---|
|**CreatedAt**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Dağıtım oluşturma zamanı (UTC).|dize (tarih)|
|**expiredAt**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Dağıtım süresi saat (UTC).|dize (tarih)|
|**Kimliği**  <br>*İsteğe bağlı*|Dağıtım kimliği|dize|
|**imageId**  <br>*İsteğe bağlı*|Bu dağıtım ile ilişkili resim kimliği.|dize|
|**ServiceName**  <br>*İsteğe bağlı*|Hizmet adı.|dize|
|**Durumu**  <br>*İsteğe bağlı*|Geçerli dağıtım durumu.|dize|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Dağıtım nesnelerinin bir dizisi.

*Tür*: <[dağıtım](#deployment)> dizisi


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Yönetim Hizmeti hata ayrıntısı model.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kod**  <br>*Gerekli*|Hata kodu.|dize|
|**İleti**  <br>*Gerekli*|Hata iletisi.|dize|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>ErrorResponse
Model yönetim hizmeti hata nesnesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Kod**  <br>*Gerekli*|Hata kodu.|dize|
|**Ayrıntıları**  <br>*İsteğe bağlı*|Hata ayrıntı nesneler dizisi.|<[ErrorDetail](#errordetail)> dizisi|
|**İleti**  <br>*Gerekli*|Hata iletisi.|dize|
|**statusCode**  <br>*İsteğe bağlı*|HTTP durum kodu.|integer|


<a name="image"></a>
### <a name="image"></a>Görüntü
Azure Machine Learning resim.


|Ad|Açıklama|Şema|
|---|---|---|
|**computeResourceId**  <br>*İsteğe bağlı*|Machine Learning işlem kaynak oluşturulan ortam kimliği.|dize|
|**createdTime**  <br>*İsteğe bağlı*|Görüntü oluşturma zamanı (UTC).|dize (tarih)|
|**creationState**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|
|**Açıklama**  <br>*İsteğe bağlı*|Görüntü açıklama metni.|dize|
|**Hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|
|**Kimliği**  <br>*İsteğe bağlı*|Görüntü Kimliği|dize|
|**imageBuildLogUri**  <br>*İsteğe bağlı*|Karşıya yüklediğiniz günlükler görüntü yapıdan URI'si.|dize|
|**imageLocation**  <br>*İsteğe bağlı*|Oluşturulan görüntüyü Azure kapsayıcı kayıt defteri konumu dizesi.|dize|
|**ImageType**  <br>*İsteğe bağlı*||[ImageType](#imagetype)|
|**Bildirimi**  <br>*İsteğe bağlı*||[Bildirimi](#manifest)|
|**Adı**  <br>*İsteğe bağlı*|Görüntü adı.|dize|
|**Sürüm**  <br>*İsteğe bağlı*|Model yönetim hizmeti tarafından kümesi görüntü sürümü.|integer|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Azure Machine Learning görüntü oluşturma isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**computeResourceId**  <br>*Gerekli*|Machine Learning işlem kaynak oluşturulan ortam kimliği.|dize|
|**Açıklama**  <br>*İsteğe bağlı*|Görüntü açıklama metni.|dize|
|**ImageType**  <br>*Gerekli*||[ImageType](#imagetype)|
|**manifestId**  <br>*Gerekli*|Görüntü oluşturulacak bildirimi kimliği.|dize|
|**Adı**  <br>*Gerekli*|Görüntü adı.|dize|


<a name="imagetype"></a>
### <a name="imagetype"></a>Resim Türü
Resim türünü belirtir.

*Tür*: enum (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Bildirim
Azure Machine Learning bildirimi.


|Ad|Açıklama|Şema|
|---|---|---|
|**Varlıklar**  <br>*Gerekli*|Varlıklar listesi.|<[Varlık](#asset)> dizisi|
|**createdTime**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Bildirim oluşturma zamanı (UTC).|dize (tarih)|
|**Açıklama**  <br>*İsteğe bağlı*|Açıklama metin bildirimi.|dize|
|**driverProgram**  <br>*Gerekli*|Sürücü program bildiriminin.|dize|
|**Kimliği**  <br>*İsteğe bağlı*|Bildirim kimliği.|dize|
|**modelIds**  <br>*İsteğe bağlı*|Model kimliklerini kayıtlı modellerin listesi. Dahil edilen modellerinden herhangi biri kaydettirilmedi isteği başarısız olur.|<string> Dizi|
|**modelType**  <br>*İsteğe bağlı*|Modeller, Model yönetim hizmetiyle zaten kayıtlı belirtir.|Enum (kayıtlı)|
|**Adı**  <br>*Gerekli*|Bildirim adı.|dize|
|**TargetRuntime**  <br>*Gerekli*||[TargetRuntime](#targetruntime)|
|**Sürüm**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Model yönetim hizmeti tarafından atanan bildirim sürüm.|integer|
|**webserviceType**  <br>*İsteğe bağlı*|Bildirimden oluşturulacak web hizmeti istenen türünü belirtir.|Enum (gerçek zamanlı)|


<a name="model"></a>
### <a name="model"></a>Model
Bir Azure Machine Learning model örneği.


|Ad|Açıklama|Şema|
|---|---|---|
|**CreatedAt**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Model oluşturma zamanı (UTC).|dize (tarih)|
|**Açıklama**  <br>*İsteğe bağlı*|Açıklama metnini model.|dize|
|**Kimliği**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Model kimliği|dize|
|**mime türü**  <br>*Gerekli*|Model içeriğin MIME türü. MIME türü hakkında daha fazla bilgi için bkz: [IANA medya türleri listesini](https://www.iana.org/assignments/media-types/media-types.xhtml).|dize|
|**Adı**  <br>*Gerekli*|Model adı.|dize|
|**etiketler**  <br>*İsteğe bağlı*|Model etiketi listesi.|<string> Dizi|
|**paketini açma**  <br>*İsteğe bağlı*|Docker görüntü oluşturma sırasında model paketten ihtiyacımız olup olmadığını gösterir.|boole|
|**URL**  <br>*Gerekli*|Model URL'si. Genellikle biz bir paylaşılan erişim imzası URL'SİNİN buraya yerleştirin.|dize|
|**Sürüm**  <br>*İsteğe bağlı*  <br>*Salt okunur*|Model yönetim hizmeti tarafından atanan model sürüm.|integer|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Model veri toplama bilgileri.


|Ad|Açıklama|Şema|
|---|---|---|
|**eventHubEnabled**  <br>*İsteğe bağlı*|Bir hizmet için bir olay hub'ı etkinleştirin.|boole|
|**storageEnabled**  <br>*İsteğe bağlı*|Bir hizmet için depolama etkinleştirin.|boole|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Numaralı listesini görüntüler.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Model nesneleri dizisi.|<[Görüntü](#image)> dizisi|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Bildirimleri numaralı bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Bildirim nesneler dizisi.|<[Bildirim](#manifest)> dizisi|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Modelleri numaralı bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Model nesneleri dizisi.|<[Model](#model)> dizisi|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Hizmetleri numaralı bir listesi.


|Ad|Açıklama|Şema|
|---|---|---|
|**nextLink**  <br>*İsteğe bağlı*|Sonuç listesindeki bir sonraki sayfaya devamlılık bağlantısı (mutlak URI).|dize|
|**value**  <br>*İsteğe bağlı*|Hizmet nesneler dizisi.|<[ServiceResponse](#serviceresponse)> dizisi|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Bir hizmet oluşturma isteği.


|Ad|Açıklama|Şema|
|---|---|---|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights etkinleştirin.|boole|
|**AutoScaler**  <br>*İsteğe bağlı*||[AutoScaler](#autoscaler)|
|**ComputeResource**  <br>*Gerekli*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**DataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Gerekli*|Bir hizmet oluşturmak için resim.|dize|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**: `1`|integer|
|**Adı**  <br>*Gerekli*|Hizmet adı.|dize|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalıştıran pod çoğaltmaların sayısı. Autoscaler etkin olup olmadığını belirtilemez.  <br>**En düşük değer**: `0`|integer|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Bir hizmet için bir anahtarı yeniden oluşturmak için bir istek.


|Ad|Açıklama|Şema|
|---|---|---|
|**keyType**  <br>*İsteğe bağlı*|Yeniden oluşturmak için hangi anahtarını belirtir.|Enum (birincil, ikincil)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Hizmet ayrıntılı durumu.


|Ad|Açıklama|Şema|
|---|---|---|
|**CreatedAt**  <br>*İsteğe bağlı*|Hizmet oluşturma zamanı (UTC).|dize (tarih)|
|**ID**  <br>*İsteğe bağlı*|Hizmet kimliği|dize|
|**Görüntü**  <br>*İsteğe bağlı*||[Görüntü](#image)|
|**Bildirimi**  <br>*İsteğe bağlı*||[Bildirimi](#manifest)|
|**Modelleri**  <br>*İsteğe bağlı*|Model listesi.|<[Model](#model)> dizisi|
|**Adı**  <br>*İsteğe bağlı*|Hizmet adı.|dize|
|**scoringUri**  <br>*İsteğe bağlı*|Hizmet Puanlama için URI.|dize|
|**Durumu**  <br>*İsteğe bağlı*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*İsteğe bağlı*|Son saat (UTC) güncelleştirin.|dize (tarih)|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights etkinleştirin.|boole|
|**AutoScaler**  <br>*İsteğe bağlı*||[AutoScaler](#autoscaler)|
|**ComputeResource**  <br>*Gerekli*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**DataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**: `1`|integer|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalıştıran pod çoğaltmaların sayısı. Autoscaler etkin olup olmadığını belirtilemez.  <br>**En düşük değer**: `0`|integer|
|**Hata**  <br>*İsteğe bağlı*||[ErrorResponse](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>ServiceUpdateRequest
Bir hizmeti güncelleştirmek için bir istek.


|Ad|Açıklama|Şema|
|---|---|---|
|**appInsightsEnabled**  <br>*İsteğe bağlı*|Bir hizmet için Application ınsights etkinleştirin.|boole|
|**AutoScaler**  <br>*İsteğe bağlı*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*İsteğe bağlı*||[ContainerResourceReservation](#containerresourcereservation)|
|**DataCollection**  <br>*İsteğe bağlı*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*İsteğe bağlı*|Bir hizmet oluşturmak için resim.|dize|
|**maxConcurrentRequestsPerContainer**  <br>*İsteğe bağlı*|Maksimum eşzamanlı istek sayısı.  <br>**En düşük değer**: `1`|integer|
|**numReplicas**  <br>*İsteğe bağlı*|Herhangi bir zamanda çalıştıran pod çoğaltmaların sayısı. Autoscaler etkin olup olmadığını belirtilemez.  <br>**En düşük değer**: `0`|integer|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Hedef çalışma zamanı türü.


|Ad|Açıklama|Şema|
|---|---|---|
|**özellikleri**  <br>*Gerekli*||< dize, dize > eşleme|
|**runtimeType**  <br>*Gerekli*|Çalışma zamanı belirtir.|Enum (SparkPython, Python)|

