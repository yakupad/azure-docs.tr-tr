---
title: Zip Azure işlevleri için anında iletme dağıtımı | Microsoft Docs
description: Azure işlevleri yayımlamak için kullanılan .zip dosyası dağıtım olanakları Kudu dağıtım hizmetinin kullanın.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/29/2018
ms.author: glenga
ms.openlocfilehash: 91c16ad5a6bf8babffc0b83d801626932688631e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699963"
---
# <a name="zip-push-deployment-for-azure-functions"></a>Zip itme dağıtımı için Azure işlevleri 
Bu makalede, Azure için bir .zip (sıkıştırılmış) dosyasından işlevi uygulama projesi dosyaları dağıtmayı açıklar. Azure CLI kullanarak hem REST API'lerini kullanarak anında iletme dağıtım yapmak öğrenin. 

Azure işlevleri, Azure App Service tarafından sağlanan sürekli dağıtım ve tümleştirme seçeneklerini tam aralığını sahiptir. Daha fazla bilgi için bkz: [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md). 

Geliştirme sırasında daha hızlı yineleme için işlevi uygulama projesi dosyalarınızdan doğrudan sıkıştırılmış .zip dosyası dağıtmak kolaydır. Bu .zip dosyası dağıtım aynı Kudu hizmeti de dahil olmak üzere bu powers sürekli tümleştirme tabanlı dağıtımlar, kullanır:

+ Önceki dağıtımlarından kalan dosyaları silme.
+ Dağıtım betikleri çalıştırma dahil dağıtım özelleştirme.
+ Dağıtım günlükleri.
+ İşlev Tetikleyicileri eşitleniyor bir [tüketim planı](functions-scale.md) işlev uygulaması.

Daha fazla bilgi için bkz: [.zip itme dağıtım başvurusu](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Dağıtım .zip dosyası gereksinimleri
Anında iletme dağıtımı için kullandığınız .zip dosyası tüm proje dosyalarını işlevi kodunuzu dahil olmak üzere işlevi uygulamanızda içermelidir. 

>[!IMPORTANT]
> .Zip itme dağıtım kullandığınızda, .zip dosyasında bulunan olmayan mevcut bir dağıtım dosyalarından işlevi uygulamanızdan silinir.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Bir işlev uygulaması tüm dosyaları ve klasörleri içeren `wwwroot` dizin. Bir .zip dosyası dağıtım içeriğini içeren `wwwroot` dizin, ancak dizin kendisini değil.  

## <a name="download-your-function-app-files"></a>İşlev uygulama dosyaları indirme

Yerel bir bilgisayarda geliştirirken, geliştirme bilgisayarınızda bir .zip dosyası işlevi uygulama projesi klasörünün oluşturmak kolaydır. 

Ancak, işlevlerinizi Azure portalında Düzenleyicisi'ni kullanarak oluşturduğunuz. Varolan işlevi uygulama projesinde şu yöntemlerden birini kullanarak yükleyebilirsiniz: 

+ **Azure portalından:** 

    1. Oturum [Azure portal](https://portal.azure.com)ve ardından işlevi uygulamanıza gidin.

    2. Üzerinde **genel bakış** sekmesine **uygulama içeriğini indirmek**. Yükleme seçeneklerinizi belirleyin ve ardından **karşıdan**.     

        ![İşlev uygulaması projesi indirme](./media/deployment-zip-push/download-project.png)

    İşlev uygulamanıza .zip itme dağıtımı kullanarak yeniden yayımlanması için doğru biçimde indirilen .zip dosyasıdır. Portal yükleme işlevi uygulamanızı doğrudan Visual Studio'da açmak için gerekli dosyaları da ekleyebilirsiniz.

+ **REST API'lerini kullanarak:** 

    Dosyaları indirmek için aşağıdaki dağıtım alma API kullanın, `<function_app>` proje: 

        https://<function_app>.scm.azurewebsites.net/api/zip/site/wwwroot/

    Dahil olmak üzere `/site/wwwroot/` zip dosyası yalnızca işlevi uygulama projesi dosyaları ve değil tüm site içerir emin olur. Zaten Azure'a oturumunuz açık değil, bunu yapmak istenir. Bir posta gönderme isteği unutmayın `api/zip/` discoraged yönelik bu konuda açıklanan zip dağıtım yöntemi bir API'dir. 

Ayrıca, Github'da depodan bir .zip dosyası indirebilirsiniz. GitHub depo bir .zip dosyası olarak yüklediğinizde, GitHub dal için bir ek klasör düzeyinde ekler aklınızda bulundurun. Github'dan karşıdan doğrudan olarak .zip dosyası dağıtılamıyor ek klasör düzeyinde anlamına gelir. İşlev uygulamanızı korumak için GitHub deposunu kullanıyorsanız, kullanmanız gereken [sürekli tümleştirme](functions-continuous-deployment.md) uygulamanızı dağıtmak için.  

## <a name="cli"></a>Azure CLI kullanarak dağıtma

Bir itme dağıtımı tetiklemek için Azure CLI kullanın. Anında iletme bir .zip dosyası işlevi uygulamanızı kullanarak dağıtın [az functionapp dağıtım kaynağı config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) komutu. Bu komutu kullanmak için Azure CLI Sürüm 2.0.21 kullanın veya sonraki bir sürümü. Kullanmakta olduğunuz hangi Azure CLI sürümünün görmek için `az --version` komutu.

Aşağıdaki komutta, `<zip_file_path>` .zip dosyanızın konumunun yolu ile yer tutucu. Ayrıca, değiştirin `<app_name>` işlevi uygulamanızı benzersiz adı. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Bu komut, azure'da işlevi uygulamanız için proje dosyalarını indirilen .zip dosyasından dağıtır. Sonra uygulamayı yeniden başlatır. Bu işlev uygulaması dağıtımlarda listesini görüntülemek için REST API'lerini kullanmanız gerekir.

Yerel bilgisayarınızda Azure CLI kullanırken `<zip_file_path>` bilgisayarınızdaki .zip dosyasının yolu. Azure CLI ayrıca komutunu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md). Bulut Kabuk kullandığınızda, bulut kabuğu ile ilişkili Azure dosyaları hesabına ilk dağıtım .zip dosyanızın yüklemeniz gerekir. Bu durumda, `<zip_file_path>` bulut Kabuk hesabınızı kullanan depolama konumu. Daha fazla bilgi için bkz: [kalıcı Azure bulut Kabuk dosyalarında](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
