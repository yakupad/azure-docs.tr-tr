---
title: Hızlı Başlangıç - İlk Azure Container Instances kapsayıcınızı oluşturma
description: Bu hızlı başlangıçta, Azure Container Instances’da bir kapsayıcı dağıtmak için Azure CLI’yı kullanacaksınız
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: da022af164af640c01c09a64ffcc64f2a67d25fc
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163013"
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Hızlı başlangıç: Azure Container Instances’da ilk kapsayıcınızı oluşturma

Azure Container Instances, sanal makine sağlamak veya daha yüksek düzey bir hizmet benimsemek zorunda kalmadan Azure’da Docker kapsayıcıları oluşturmayı ve yönetmeyi kolaylaştırır. Bu hızlı başlangıç içeriğinde, bir kapsayıcı oluşturacak ve bu kapsayıcıyı tam etki alanı adı (FQDN) ile İnternet üzerinden kullanıma sunacaksınız. Bu işlem tek bir komutla tamamlanır. Yalnızca birkaç saniye içinde tarayıcınızda şunu görürsünüz:

![Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor][aci-app-browser]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıcı tamamlamak için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.27 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm Azure kaynakları gibi Azure kapsayıcı örnekleri de Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir koleksiyon olan Azure kaynak grubuna yerleştirilmelidir.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

[az container create][az-container-create] komutuna bir ad, Docker görüntüsü ve bir Azure kaynak grubu sağlayarak kapsayıcı oluşturabilirsiniz. İsteğe bağlı olarak, bir DNS ad etiketi belirterek kapsayıcıyı internet üzerinden kullanıma sunabilirsiniz. Bu hızlı başlangıçta, [Node.js][node-js] içinde yazılan küçük bir web uygulamasını barındıran bir kapsayıcı dağıtırsınız.

Bir kapsayıcı örneği başlatmak için aşağıdaki komutu yürütün. `--dns-name-label` değeri, örneği oluşturduğunuz Azure bölgesi içinde benzersiz olmalıdır, bu yüzden benzersizliği sağlamak için bu değeri değiştirmeniz gerekebilir.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Birkaç saniye içinde isteğinize yanıt almanız gerekir. Kapsayıcı başlangıçta **Oluşturuluyor** durumunda olacaktır ancak birkaç saniye içinde başlar. Durumu [az container show][az-container-show] komutunu kullanarak denetleyebilirsiniz:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Komutu çalıştırdığınızda, kapsayıcının tam etki alanı adı (FQDN) ve sağlama durumu görüntülenir:

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Kapsayıcı **Başarılı** durumuna geçtiğinde, tarayıcınızda FQDN’sine gidin:

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü][aci-app-browser]

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Kapsayıcı örneğinin günlüklerini görüntülemek, kapsayıcınızın veya çalıştırdığı uygulamanın sorunlarını gidermede yardımcı olur.

[az container logs][az-container-logs] komutu ile kapsayıcının günlüklerini çekin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıkış, kapsayıcının günlüklerini görüntüler ve uygulamayı tarayıcınızda görüntülediğinizde oluşturulan HTTP GET isteklerini göstermelidir.

```console
$ az container logs --resource-group myResourceGroup -n mycontainer
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="attach-output-streams"></a>Çıkış akışları ekleme

Günlükleri kuyruğa almaya ek olarak, yerel standart çıkış ve standart hata akışlarınızı kapsayıcınınkine ekleyebilirsiniz.

İlk olarak [az container attach][az-container-attach] komutunu yürüterek yerel konsolunuzu kapsayıcının çıkış akışlarına ekleyin:

```azurecli-interactive
az container attach --resource-group myResourceGroup -n mycontainer
```

Bağlandıktan sonra, ek çıkışlar oluşturmak için tarayıcınızı birkaç defa yenileyin. Son olarak, `Control+C` ile konsolunuzu ayırın. Aşağıdakine benzer bir çıktı görmeniz gerekir:

```console
$ az container attach --resource-group myResourceGroup -n mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcıyla işiniz bittiğinde [az container delete][az-container-delete] komutunu kullanarak kapsayıcıyı kaldırın:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Kapsayıcının silindiğini doğrulamak için, [az container list](/cli/azure/container#az-container-list) komutunu yürütün:

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

**mycontainer** kapsayıcısı komut çıkışında görünmemelidir. Kaynak grubunda başka kapsayıcınız yoksa, çıkış görüntülenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, genel bir Docker Hub deposundaki bir görüntüden bir Azure kapsayıcı örneği oluşturdunuz. Kapsayıcı görüntüsünü kendiniz oluşturup özel bir Azure kapsayıcı kayıt defterinden Azure Container Instances’a dağıtmak istiyorsanız Azure Container Instances öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi](./container-instances-tutorial-prepare-app.md)

Kapsayıcıları Azure’da bir düzenleme sistemi içinde çalıştırma seçenekleri için, [Service Fabric][service-fabric] veya [Azure Kubernetes Hizmeti (AKS)][container-service] hızlı başlangıçlarına bakın.

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-container-list]: /cli/azure/container#az-container-list
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
