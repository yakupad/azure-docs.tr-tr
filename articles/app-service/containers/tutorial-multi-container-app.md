---
title: Kapsayıcılar için Web App uygulamasında çok kapsayıcılı (önizleme) uygulama oluşturma
description: Docker Compose ve Kubernetes yapılandırma dosyalarını bir WordPress ve MySQL uygulamasıyla birlikte kullanarak Azure'da birden fazla kapsayıcı kullanmayı öğrenin.
keywords: azure app service, web uygulaması, linux, docker, oluşturma, çok kapsayıcılı, çoklu kapsayıcı, kapsayıcı, kubernetes, wordpress, mysql için azure db, kapsayıcılara sahip üretim veritabanı
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: dcda4e25932a74313674e91afc7382ea19724613
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129959"
---
# <a name="tutorial-create-a-multi-container-preview-app-in-web-app-for-containers"></a>Öğretici: Kapsayıcılar için Web App uygulamasında çok kapsayıcılı (önizleme) uygulama oluşturma

[Kapsayıcılar için Web App](app-service-linux-intro.md), Docker görüntülerini esnek bir şekilde kullanmanızı sağlar. Bu öğreticide WordPress ve MySQL kullanarak çok kapsayıcılı bir uygulama oluşturmayı öğreneceksiniz. Bu öğreticiyi Cloud Shell'de tamamlayacaksınız ama bu komutları [Cloud Shell](/cli/azure/install-azure-cli) (2.0.32 veya üzeri) ile yerel olarak da çalıştırabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir Docker Compose yapılandırmasını Kapsayıcılar için Web App ile çalışacak biçime dönüştürme
> * Bir Kubernetes yapılandırmasını Kapsayıcılar için Web App ile çalışacak biçime dönüştürme
> * Çok kapsayıcılı bir uygulamayı Azure'a dağıtma
> * Uygulama ayarları ekleme
> * Kapsayıcılarınız için kalıcı depolama kullanma
> * MySQL için Azure Veritabanı'na bağlanma
> * Sorun giderme hataları

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için [Docker Compose](https://docs.docker.com/compose/) veya [Kubernetes](https://kubernetes.io/) konusunda deneyimli olmanız gerekir.

## <a name="download-the-sample"></a>Örneği indirme

Bu öğreticide [Docker](https://docs.docker.com/compose/wordpress/#define-the-project) Compose dosyasını kullanacaksınız ancak bu dosyayı MySQL için Azure Veritabanı, kalıcı depolama ve Redis bileşenlerini içerecek şekilde değiştireceksiniz. Yapılandırma dosyasına [Azure Örnekleri](https://github.com/Azure-Samples/multicontainerwordpress) sayfasından ulaşabilirsiniz.

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

Cloud Shell'de bir öğretici dizini oluşturun ve o dizine geçin.

```bash
mkdir tutorial

cd tutorial
```

Ardından, örnek uygulama deposunu öğretici dizininize kopyalamak için aşağıdaki komutu çalıştırın. Ardından `multicontainerwordpress` dizinine geçin.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress

cd multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Cloud Shell içinde [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *Orta Güney ABD* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. **Standart** katmanda Linux üzerinde App Service için desteklenen tüm konumları görüntülemek için [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz.

Komut tamamlandığında, bir JSON çıkışı size kaynak grubu özelliklerini gösterir.

## <a name="create-an-azure-app-service-plan"></a>Azure App Service planı oluşturma

Cloud Shell’de, kaynak grubunda [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) komutuyla bir App Service planı oluşturun.

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

Aşağıdaki örnek, **Standart** fiyatlandırma katmanı (`--sku S1`) ve bir Linux kapsayıcısı (`--is-linux`) içinde `myAppServicePlan` adlı bir App Service planı oluşturur.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

App Service planı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

## <a name="docker-compose-configuration-options"></a>Docker Compose yapılandırma seçenekleri

Bu öğreticide [Docker](https://docs.docker.com/compose/wordpress/#define-the-project) Compose dosyasını kullanacaksınız ancak bu dosyayı MySQL için Azure Veritabanı, kalıcı depolama ve Redis bileşenlerini içerecek şekilde değiştireceksiniz. Alternatif olarak bir [Kubernetes yapılandırması](#use-a-kubernetes-configuration-optional) da kullanabilirsiniz. Yapılandırma dosyalarına [Azure Örnekleri](https://github.com/Azure-Samples/multicontainerwordpress) sayfasından ulaşabilirsiniz.

Kapsayıcılar için Web App uygulamasında desteklenen ve desteklenmeyen Docker Compose yapılandırma seçenekleri aşağıda listelenmiştir:

### <a name="supported-options"></a>Desteklenen seçenekler

* command
* entrypoint
* environment
* image
* ports
* restart
* services
* volumes

### <a name="unsupported-options"></a>Desteklenmeyen seçenekler

* build (izin verilmez)
* depends_on (yoksayılır)
* networks (yoksayılır)
* secrets (yoksayılır)

> [!NOTE]
> Açıkça çağrılmayan diğer tüm seçenekler de Genel Önizleme'de yoksayılır.

### <a name="docker-compose-with-wordpress-and-mysql-containers"></a>Docker Compose içeriğini WordPress ve MySQL kapsayıcılarıyla kullanma

## <a name="create-a-docker-compose-app"></a>Docker Compose uygulaması oluşturma

Cloud Shell'de [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutunu kullanarak `myAppServicePlan` App Service planında çok kapsayıcılı bir [web uygulaması](app-service-linux-intro.md) oluşturun. _\<app_name>_ yerine benzersiz bir uygulama adı girmeyi unutmayın.

```bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type compose --multicontainer-config-file docker-compose-wordpress.yml
```

Web uygulaması oluşturulduğunda Cloud Shell aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Uygulamaya göz atma

Dağıtılan uygulamaya göz atmak için (`http://<app_name>.azurewebsites.net`) adresine gidin. Uygulamanın yüklenmesi birkaç dakika sürebilir. Bir hatayla karşılaşırsanız birkaç dakika daha bekleyip tarayıcıyı yenileyin. Sorun yaşıyorsanız gidermek için [kapsayıcı günlüklerini](#find-docker-container-logs) gözden geçirin.

![Kapsayıcılar için Web App üzerinde örnek çok kapsayıcılı uygulama][1]

**Tebrikler**, Kapsayıcılar için Web App üzerinde çok kapsayıcılı bir uygulama oluşturdunuz. Şimdi uygulamanızı MySQL için Azure Veritabanı'nı kullanacak şekilde yapılandıracaksınız. WordPress'i şu anda yüklemeyin.

## <a name="connect-to-production-database"></a>Üretim veritabanına bağlanma

Üretim ortamındaki veritabanı kapsayıcılarını kullanmanız önerilmez. Yerel kapsayıcılar ölçeklenebilir özelliklere sahip değildir. Bunun yerine ölçeklendirilebilen MySQL için Azure Veritabanı'nı kullanacaksınız.

### <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma

[`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) komutuyla MySQL için Azure Veritabanı sunucusu oluşturun.

Aşağıdaki komutta, _&lt;mysql_server_name>_ yer tutucusunu gördüğünüz yerde MySQL sunucunuzun adını değiştirin (geçerli karakterler `a-z`, `0-9` ve `-`). Bu ad, MySQL sunucusu ana bilgisayar adının (`<mysql_server_name>.database.windows.net`) bir parçasıdır ve genel olarak benzersiz olması gerekir.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql_server_name>  --location "South Central US" --admin-user adminuser --admin-password My5up3rStr0ngPaSw0rd! --sku-name B_Gen4_1 --version 5.7
```

Sunucunun oluşturulması birkaç dakika sürebilir. MySQL sunucusu oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "southcentralus",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Sunucu güvenlik duvarını yapılandırma

[`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) komutunu kullanarak MySQL sunucunuzun istemci bağlantılarına izin vermesi için bir güvenlik duvarı kuralı oluşturun. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır.

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

### <a name="create-the-wordpress-database"></a>WordPress veritabanını oluşturma

```bash
az mysql db create --resource-group myResourceGroup --server-name <mysql_server_name> --name wordpress
```

Veritabanı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "additionalProperties": {},
  "charset": "latin1",
  "collation": "latin1_swedish_ci",
  "id": "/subscriptions/12db1644-4b12-4cab-ba54-8ba2f2822c1f/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>/databases/wordpress",
  "name": "wordpress",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DBforMySQL/servers/databases"
}
```

### <a name="configure-database-variables-in-wordpress"></a>WordPress'te veritabanı değişkenlerini yapılandırma

WordPress uygulaması ile yeni oluşturduğunuz MySQL sunucusu arasında bağlantı kurmak için `MYSQL_SSL_CA` ile tanımlanan SSL CA yolu da dahil olmak üzere WordPress'e özgü birkaç ortam değişkenini yapılandıracaksınız. [DigiCert](http://www.digicert.com/) tarafından sağlanan [Baltimore CyberTrust Root](https://www.digicert.com/digicert-root-certificates.htm), aşağıdaki [özel görüntüde](https://docs.microsoft.com/en-us/azure/app-service/containers/tutorial-multi-container-app#use-a-custom-image-for-mysql-ssl-and-other-configurations) gösterilmiştir.

Bu değişiklikleri yapmak için Cloud Shell'de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WORDPRESS_DB_HOST="<mysql_server_name>.mysql.database.azure.com" WORDPRESS_DB_USER="adminuser@<mysql_server_name>" WORDPRESS_DB_PASSWORD="My5up3rStr0ngPaSw0rd!" WORDPRESS_DB_NAME="wordpress" MYSQL_SSL_CA="BaltimoreCyberTrustroot.crt.pem"
```

Uygulama ayarı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
[
  {
    "name": "WORDPRESS_DB_HOST",
    "slotSetting": false,
    "value": "<mysql_server_name>.mysql.database.azure.com"
  },
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql_server_name>"
  },
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WORDPRESS_DB_PASSWORD",
    "slotSetting": false,
    "value": "My5up3rStr0ngPaSw0rd!"
  },
  {
    "name": "MYSQL_SSL_CA",
    "slotSetting": false,
    "value": "BaltimoreCyberTrustroot.crt.pem"
  }
]
```

### <a name="use-a-custom-image-for-mysql-ssl-and-other-configurations"></a>MySQL SSL ve diğer yapılandırmalar için özel görüntü kullanma

Varsayılan olarak MySQL için Azure Veritabanı için SSL kullanılır. WordPress'te MySQL ile birlikte SSL kullanmak için ek yapılandırma gerekir. WordPress'in "resmi görüntüsü" ek yapılandırma sağlamaz ancak kolaylık olması açısından sizin için bir [özel görüntü](https://hub.docker.com/r/microsoft/multicontainerwordpress/builds/) hazırlanmıştır. Normalde yapmak istediğiniz değişiklikleri kendi görüntünüze eklemeniz gerekir.

Özel görüntü, [Docker Hub üzerindeki WordPress](https://hub.docker.com/_/wordpress/) "resmi görüntüsünü" temel almaktadır. Bu özel görüntüde MySQL için Azure Veritabanı'na özgü aşağıdaki değişiklikler yapılmıştır:

* [MySQL ile SSL bağlantısı için Baltimore Cyber Trust Kök Sertifika dosyasını ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L61)
* [WordPress wp-config.php dosyasında MySQL SSL Sertifika Yetkilisi Uygulama Ayarını kullanır.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L163)
* [MySQL SSL'de gerekli olan MYSQL_CLIENT_FLAGS için ilgili WordPress tanımını ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L164)

Redis için aşağıdaki değişiklikler yapılmıştır (daha sonraki bir bölümde kullanılmak üzere):

* [Redis v4.0.2 için PHP uzantısını ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/Dockerfile#L35)
* [Dosyaların ayıklanması için gerekli olan sıkıştırma açma özelliğini ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L71)
* [Redis Object Cache 1.3.8 WordPress eklentisini ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L74)
* [WordPress wp-config.php dosyasında Redis ana bilgisayar adı Uygulama Ayarını kullanır.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L162)

Özel görüntüyü kullanmak için docker-compose-wordpress.yml dosyanızı güncelleştirmeniz gerekir. Cloud Shell'de, nano metin düzenleyicisini açmak için `nano docker-compose-wordpress.yml` yazın. `image: microsoft/multicontainerwordpress` kullanmak için `image: wordpress` üzerinde değişiklik yapın. Veritabanı kapsayıcıya artık ihtiyacınız yoktur. Yapılandırma dosyasındaki `db`, `environment`, `depends_on` ve `volumes` bölümlerini kaldırın. Dosyanız aşağıdaki kod gibi görünmelidir:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     ports:
       - "8000:80"
     restart: always
```

Değişikliklerinizi kaydedin ve nanodan çıkın. Kaydetmek için `^O` ve çıkmak için `^X` komutunu kullanın.

### <a name="update-app-with-new-configuration"></a>Uygulamayı yeni yapılandırmayla güncelleştirme

Cloud Shell'de [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) komutunu kullanarak çok kapsayıcılı [web uygulamanızı](app-service-linux-intro.md) yeniden yapılandırın. _\<app_name>_ yerine önceki adımlarda oluşturduğunuz web uygulamasının adını yazmayı unutmayın.

```bash
az webapp config container set --resource-group myResourceGroup --name <app_name> --multicontainer-config-type compose --multicontainer-config-file docker-compose-wordpress.yml
```

Uygulama yeniden yapılandırıldığında Cloud Shell aşağıda yer alan örnekteki gibi bilgiler gösterir:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICB3b3JkcHJlc3M6CiAgICAgaW1hZ2U6IG1zYW5nYXB1L3dvcmRwcmVzcwogICAgIHBvcnRzOgogICAgICAgLSAiODAwMDo4MCIKICAgICByZXN0YXJ0OiBhbHdheXM="
  }
]
```

### <a name="browse-to-the-app"></a>Uygulamaya göz atma

Dağıtılan uygulamaya göz atmak için (`http://<app_name>.azurewebsites.net`) adresine gidin. Uygulama artık MySQL için Azure Veritabanı'nı kullanıyor.

![Kapsayıcılar için Web App üzerinde örnek çok kapsayıcılı uygulama][1]

## <a name="add-persistent-storage"></a>Kalıcı depolama ekleme

Çok kapsayıcılı uygulamanız şimdi Kapsayıcılar için Web App üzerinde çalışıyor. Ancak WordPress'i yükleyip uygulamanızı yeniden başlattığınızda WordPress yüklemenizin silindiğini göreceksiniz. Bunun nedeni, Docker Compose yapılandırmanızın şu anda kapsayıcınızın içindeki bir depolama konumunu kullanıyor olmasıdır. Kapsayıcınıza yüklediğiniz dosyalar, uygulama yeniden başlatıldığında saklanmaz. Bu bölümde WordPress kapsayıcınıza kalıcı depolama eklemeyi öğreneceksiniz.

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Kalıcı depolamayı kullanmak için App Service'te ilgili ayarı etkinleştirmeniz gerekir. Bu değişikliği yapmak için Cloud Shell'de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Uygulama ayarı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
[
  < JSON data removed for brevity. >
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  }
]
```

### <a name="modify-configuration-file"></a>Yapılandırma dosyasını değiştirme

Cloud Shell'de, nano metin düzenleyicisini açmak için `nano docker-compose-wordpress.yml` yazın.

`volumes` seçeneği, dosya sistemini kapsayıcı içindeki bir dizinle eşler. `${WEBAPP_STORAGE_HOME}`, App Service içinde bulunan ve uygulamanız için kalıcı depolamayla eşlenmiş olan bir ortam değişkenidir. Bu ortam değişkenini volumes seçeneğinde kullanarak WordPress dosyalarının kapsayıcı yerine kalıcı depolama alanına yüklenmesini sağlayabilirsiniz. Dosyada aşağıdaki değişiklikleri yapın:

`wordpress` bölümünde bir `volumes` seçeneği ekleyerek aşağıdaki kod gibi görünmesini sağlayın:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     volumes:
      - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
     ports:
       - "8000:80"
     restart: always
```

### <a name="update-app-with-new-configuration"></a>Uygulamayı yeni yapılandırmayla güncelleştirme

Cloud Shell'de [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) komutunu kullanarak çok kapsayıcılı [web uygulamanızı](app-service-linux-intro.md) yeniden yapılandırın. _\<app_name>_ yerine benzersiz bir uygulama adı girmeyi unutmayın.

```bash
az webapp config container set --resource-group myResourceGroup --name <app_name> --multicontainer-config-type compose --multicontainer-config-file docker-compose-wordpress.yml
```

Komutunuz çalıştıktan sonra aşağıdaki örneğe benzer bir çıkış gösterir:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICBteXNxbDoKICAgICBpbWFnZTogbXlzcWw6NS43CiAgICAgdm9sdW1lczoKICAgICAgIC0gZGJfZGF0YTovdmFyL2xpYi9teXNxbAogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgTVlTUUxfUk9PVF9QQVNTV09SRDogZXhhbXBsZXBhc3MKCiAgIHdvcmRwcmVzczoKICAgICBkZXBlbmRzX29uOgogICAgICAgLSBteXNxbAogICAgIGltYWdlOiB3b3JkcHJlc3M6bGF0ZXN0CiAgICAgcG9ydHM6CiAgICAgICAtICI4MDAwOjgwIgogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgV09SRFBSRVNTX0RCX1BBU1NXT1JEOiBleGFtcGxlcGFzcwp2b2x1bWVzOgogICAgZGJfZGF0YTo="
  }
]
```

### <a name="browse-to-the-app"></a>Uygulamaya göz atma

Dağıtılan uygulamaya göz atmak için (`http://<app_name>.azurewebsites.net`) adresine gidin.

WordPress kapsayıcısı artık MySQL için Azure Veritabanı'nı ve kalıcı depolamayı kullanıyor.

## <a name="add-redis-container"></a>Redis kapsayıcısı ekleme

 WordPress'in "resmi görüntüsü" Redis bağımlılıklarını içermez. WordPress'i Redis ile birlikte kullanabilmeniz için gerekli olan bu bağımlılıklar ve ek yapılandırma adımları bu [özel görüntüde](https://github.com/Azure-Samples/multicontainerwordpress) sizin için hazırlanmıştır. Normalde yapmak istediğiniz değişiklikleri kendi görüntünüze eklemeniz gerekir.

Özel görüntü, [Docker Hub üzerindeki WordPress](https://hub.docker.com/_/wordpress/) "resmi görüntüsünü" temel almaktadır. Bu özel görüntüde Redis'e özgü aşağıdaki değişiklikler yapılmıştır:

* [Redis v4.0.2 için PHP uzantısını ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/Dockerfile#L35)
* [Dosyaların ayıklanması için gerekli olan sıkıştırma açma özelliğini ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L71)
* [Redis Object Cache 1.3.8 WordPress eklentisini ekler.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L74)
* [WordPress wp-config.php dosyasında Redis ana bilgisayar adı Uygulama Ayarını kullanır.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L162)

Redis kapsayıcısını yapılandırma dosyasının en altına ekleyerek aşağıdaki örnek gibi görünmesini sağlayın:

[!code-yml[Main](../../../azure-app-service-multi-container/compose-wordpress.yml)]

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Redis'i kullanmak için App Service'te `WP_REDIS_HOST` ayarını etkinleştirmeniz gerekir. Bu, WordPress'in Redis ana bilgisayarıyla iletişim kurabilmesi için *yapılması gerekli olan bir ayardır*. Bu değişikliği yapmak için Cloud Shell'de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WP_REDIS_HOST="redis"
```

Uygulama ayarı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
[
  < JSON data removed for brevity. >
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql_server_name>"
  },
  {
    "name": "WP_REDIS_HOST",
    "slotSetting": false,
    "value": "redis"
  }
]
```

### <a name="update-app-with-new-configuration"></a>Uygulamayı yeni yapılandırmayla güncelleştirme

Cloud Shell'de [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) komutunu kullanarak çok kapsayıcılı [web uygulamanızı](app-service-linux-intro.md) yeniden yapılandırın. _\<app_name>_ yerine benzersiz bir uygulama adı girmeyi unutmayın.

```bash
az webapp config container set --resource-group myResourceGroup --name <app_name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Komutunuz çalıştıktan sonra aşağıdaki örneğe benzer bir çıkış gösterir:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICBteXNxbDoKICAgICBpbWFnZTogbXlzcWw6NS43CiAgICAgdm9sdW1lczoKICAgICAgIC0gZGJfZGF0YTovdmFyL2xpYi9teXNxbAogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgTVlTUUxfUk9PVF9QQVNTV09SRDogZXhhbXBsZXBhc3MKCiAgIHdvcmRwcmVzczoKICAgICBkZXBlbmRzX29uOgogICAgICAgLSBteXNxbAogICAgIGltYWdlOiB3b3JkcHJlc3M6bGF0ZXN0CiAgICAgcG9ydHM6CiAgICAgICAtICI4MDAwOjgwIgogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgV09SRFBSRVNTX0RCX1BBU1NXT1JEOiBleGFtcGxlcGFzcwp2b2x1bWVzOgogICAgZGJfZGF0YTo="
  }
]
```

### <a name="browse-to-the-app"></a>Uygulamaya göz atma

Dağıtılan uygulamaya göz atmak için (`http://<app_name>.azurewebsites.net`) adresine gidin.

Adımları tamamlayın ve WordPress'i yükleyin.

### <a name="connect-wordpress-to-redis"></a>WordPress'i Redis'e bağlama

WordPress admin oturumu açın. Sol gezinti bölmesinde **Eklentiler**'i ve ardından **Yüklü Eklentiler**'i seçin.

![WordPress Eklentileri'ni seçin][2]

Tüm eklentiler burada gösterilir

Eklentiler sayfasında **Redis Object Cache**'i bulun ve **Etkinleştir**'e tıklayın.

![Redis'i etkinleştirme][3]

**Ayarlar**’a tıklayın.

![Ayarlar'a tıklayın][4]

**Enable Object Cache** (Nesne Önbelleğini Etkinleştir) düğmesine tıklayın.

!["Enable Object Cache" (Nesne Önbelleğini Etkinleştir) düğmesine tıklayın][5]

WordPress, Redis sunucusuna bağlanır. Bağlantı **durumu** aynı sayfada görüntülenir.

![WordPress, Redis sunucusuna bağlanır. Bağlantı **durumu** aynı sayfada görüntülenir.][6]

**Tebrikler**, WordPress'i Redis'e bağladınız. Üretime hazır uygulama artık **MySQL için Azure Veritabanı, kalıcı depolama ve Redis**'i kullanıyor. Artık App Service Planınızı birden fazla örnek olacak şekilde ölçeklendirebilirsiniz.

## <a name="use-a-kubernetes-configuration-optional"></a>Kubernetes yapılandırması kullanma (isteğe bağlı)

Bu bölümde birden fazla kapsayıcı dağıtmak için Kubernetes yapılandırması kullanmayı öğreneceksiniz. Yukarıda yer alan [kaynak grubu](#create-a-resource-group) ve [App Service planı](#create-an-azure-app-service-plan) oluşturma adımlarını tamamladığınızdan emin olun. Adımların çoğu Compose bölümüyle benzerlik gösterdiğinden yapılandırma dosyası sizin için birleştirilmiştir.

### <a name="supported-kubernetes-options-for-multi-container"></a>Çoklu kapsayıcı için desteklenen Kubernetes seçenekleri

* args
* command
* containers
* image
* ad
* ports
* spec

> [!NOTE]
>Açıkça çağrılmayan diğer tüm Kubernetes seçenekleri de Genel Önizleme'de desteklenmez.
>

### <a name="kubernetes-configuration-file"></a>Kubernetes yapılandırma dosyası

Öğreticinin bu bölümünde *kubernetes-wordpress.yml* dosyasını kullanacaksınız. Başvuru amacıyla burada gösterilmiştir:

[!code-yml[Main](../../../azure-app-service-multi-container/kubernetes-wordpress.yml)]

### <a name="create-an-azure-database-for-mysql-server"></a>MySQL için Azure Veritabanı sunucusu oluşturma

MySQL için Azure Veritabanı (Önizleme) içinde [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) komutu ile bir sunucu oluşturun.

Aşağıdaki komutta, _&lt;mysql_server_name>_ yer tutucusunu gördüğünüz yerde MySQL sunucunuzun adını değiştirin (geçerli karakterler `a-z`, `0-9` ve `-`). Bu ad, MySQL sunucusu ana bilgisayar adının (`<mysql_server_name>.database.windows.net`) bir parçasıdır ve genel olarak benzersiz olması gerekir.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql_server_name>  --location "South Central US" --admin-user adminuser --admin-password My5up3rStr0ngPaSw0rd! --sku-name B_Gen4_1 --version 5.7
```

MySQL sunucusu oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "southcentralus",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Sunucu güvenlik duvarını yapılandırma

[`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) komutunu kullanarak MySQL sunucunuzun istemci bağlantılarına izin vermesi için bir güvenlik duvarı kuralı oluşturun. Hem başlangıç hem bitiş IP’si 0.0.0.0 olarak ayarlandığında, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır.

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> [Yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../app-service-ip-addresses.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) güvenlik duvarı kurallarınızda daha da kısıtlayıcı olabilirsiniz.
>

### <a name="create-the-wordpress-database"></a>WordPress veritabanını oluşturma

Henüz yapmadıysanız bir [MySQL için Azure Veritabanı sunucusu](#create-an-azure-database-for-mysql-server) oluşturun.

```bash
az mysql db create --resource-group myResourceGroup --server-name <mysql_server_name> --name wordpress
```

Veritabanı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "additionalProperties": {},
  "charset": "latin1",
  "collation": "latin1_swedish_ci",
  "id": "/subscriptions/12db1644-4b12-4cab-ba54-8ba2f2822c1f/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>/databases/wordpress",
  "name": "wordpress",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DBforMySQL/servers/databases"
}
```

### <a name="configure-database-variables-in-wordpress"></a>WordPress'te veritabanı değişkenlerini yapılandırma

WordPress uygulaması ile yeni oluşturduğunuz MySQL sunucusu arasında bağlantı kurmak için WordPress'e özgü birkaç ortam değişkenini yapılandıracaksınız. Bu değişikliği yapmak için Cloud Shell'de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WORDPRESS_DB_HOST="<mysql_server_name>.mysql.database.azure.com" WORDPRESS_DB_USER="adminuser@<mysql_server_name>" WORDPRESS_DB_PASSWORD="My5up3rStr0ngPaSw0rd!" WORDPRESS_DB_NAME="wordpress" MYSQL_SSL_CA="BaltimoreCyberTrustroot.crt.pem"
```

Uygulama ayarı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
[
  {
    "name": "WORDPRESS_DB_HOST",
    "slotSetting": false,
    "value": "<mysql_server_name>.mysql.database.azure.com"
  },
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql_server_name>"
  },
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WORDPRESS_DB_PASSWORD",
    "slotSetting": false,
    "value": "My5up3rStr0ngPaSw0rd!"
  }
]
```

### <a name="add-persistent-storage"></a>Kalıcı depolama ekleme

Çok kapsayıcılı uygulamanız şimdi Kapsayıcılar için Web App üzerinde çalışıyor. Dosyalar kalıcı olmadığından yeniden başlatma sonrasında veriler silinecektir. Bu bölümde WordPress kapsayıcınıza kalıcı depolama eklemeyi öğreneceksiniz.

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Kalıcı depolamayı kullanmak için App Service'te ilgili ayarı etkinleştirmeniz gerekir. Bu değişikliği yapmak için Cloud Shell'de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```bash
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Uygulama ayarı oluşturulduğunda Cloud Shell, aşağıdaki örneğe benzer bilgiler gösterir:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  }
]
```

### <a name="create-a-multi-container-app-kubernetes"></a>Çok kapsayıcılı uygulama oluşturma (Kubernetes)

Cloud Shell'de [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutunu kullanarak `myResourceGroup` kaynak grubunda ve `myAppServicePlan` App Service planında çok kapsayıcılı bir [web uygulaması](app-service-linux-intro.md) oluşturun. _\<app_name>_ yerine benzersiz bir uygulama adı girmeyi unutmayın.

```bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --multicontainer-config-type kube --multicontainer-config-file kubernetes-wordpress.yml
```

Web uygulaması oluşturulduğunda Cloud Shell aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Uygulamaya göz atma

Dağıtılan uygulamaya göz atmak için (`http://<app_name>.azurewebsites.net`) adresine gidin.

Uygulama artık Kapsayıcılar için Web App üzerinde birden fazla kapsayıcıda çalışıyor.

![Kapsayıcılar için Web App üzerinde örnek çok kapsayıcılı uygulama][1]

**Tebrikler**, Kapsayıcılar için Web App üzerinde çok kapsayıcılı bir uygulama oluşturdunuz.

Redis'i kullanmak için [WordPress'i Redis'e bağlama](#connect-wordpress-to-redis) bölümündeki adımları takip edin.

## <a name="find-docker-container-logs"></a>Docker Kapsayıcısı günlüklerini bulma

Birden fazla kapsayıcı kullanma konusunda sorun yaşarsanız şu konumdan kapsayıcı günlüklerine ulaşabilirsiniz: `https://<app_name>.scm.azurewebsites.net/api/logs/docker`.

Aşağıdaki örneğe benzer bir çıktı görürsünüz:

```json
[
   {
      "machineName":"RD00XYZYZE567A",
      "lastUpdated":"2018-05-10T04:11:45Z",
      "size":25125,
      "href":"https://<app_name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_05_10_RD00XYZYZE567A_docker.log",
      "path":"/home/LogFiles/2018_05_10_RD00XYZYZE567A_docker.log"
   }
]
```

Her kapsayıcı için bir günlük ve üst işlem için ek bir günlük görürsünüz. Günlüğü görüntülemek için ilgili `href` değerini tarayıcıya kopyalayın.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Bir Docker Compose yapılandırmasını Kapsayıcılar için Web App ile çalışacak biçime dönüştürme
> * Bir Kubernetes yapılandırmasını Kapsayıcılar için Web App ile çalışacak biçime dönüştürme
> * Çok kapsayıcılı bir uygulamayı Azure'a dağıtma
> * Uygulama ayarları ekleme
> * Kapsayıcılarınız için kalıcı depolama kullanma
> * MySQL için Azure Veritabanı'na bağlanma
> * Sorun giderme hataları

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kapsayıcılar için Web App'e yönelik özel Docker görüntüsü kullanma](tutorial-custom-docker-image.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
[2]: ./media/tutorial-multi-container-app/wordpress-plugins.png
[3]: ./media/tutorial-multi-container-app/activate-redis.png
[4]: ./media/tutorial-multi-container-app/redis-settings.png
[5]: ./media/tutorial-multi-container-app/enable-object-cache.png
[6]: ./media/tutorial-multi-container-app/redis-connected.png
