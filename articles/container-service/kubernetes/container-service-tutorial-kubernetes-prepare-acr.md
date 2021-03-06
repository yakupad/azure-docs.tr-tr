---
title: Azure Container Service öğreticisi - ACR Hazırlama
description: Azure Container Service öğreticisi - ACR Hazırlama
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f58a8d76cc46ac25474c7b91e464974612876a06
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32164971"
---
# <a name="deploy-and-use-azure-container-registry"></a>Azure Container Registry’yi dağıtma ve kullanma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Azure Container Registry (ACR), Docker kapsayıcı görüntüleri için Azure tabanlı özel bir kayıt defteridir. Yedi öğreticiden oluşan bu serinin ikinci kısmında, Azure Container Registry örneği dağıtma ve bu örneğe kapsayıcı görüntüsü göndermeyle ilgili bilgi verilmektedir. Tamamlanan adımlar:

> [!div class="checklist"]
> * Azure Container Registry (ACR) örneği dağıtma
> * ACR için kapsayıcı görüntüsü etiketleme
> * Görüntüyü ACR’ye yükleme

Sonraki öğreticilerde, bu ACR örneği Azure Container Service’teki Kubernetes kümesiyle tümleştirilecektir. 

## <a name="before-you-begin"></a>Başlamadan önce

[Önceki öğreticide](./container-service-tutorial-kubernetes-prepare-app.md), basit bir Azure Voting uygulaması için kapsayıcı görüntüsü oluşturulacaktır. Azure Voting uygulaması görüntüsünü oluşturmadıysanız [Öğretici 1 - Kapsayıcı görüntüleri oluştur](./container-service-tutorial-kubernetes-prepare-app.md)’a dönün.

Bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Azure Container Registry’yi dağıtma

Bir Azure Container Registry dağıtırken önce bir kaynak grubuna ihtiyaç duyarsınız. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Bu örnekte, `westeurope`bölgesinde `myResourceGroup` adlı bir kaynak grubu oluşturulur.

```azurecli
az group create --name myResourceGroup --location westeurope
```

[az acr create](/cli/azure/acr#az_acr_create) komutuyla Azure Container kayıt defteri oluşturun. Container Registry adı **benzersiz olmalıdır**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bu öğreticinin geri kalan aşamalarında, kapsayıcı kayıt defteri adı için yer tutucu olarak `<acrname>` kullanacağız.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defterinde oturum açma

ACR örneğinde oturum açmak için [az acr login](https://docs.microsoft.com/cli/azure/acr#az_acr_login) komutunu kullanın. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir adı sağlamanız gerekir.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandığında bir “Oturum Başarıyla Açıldı” iletisi döndürür.

## <a name="tag-container-images"></a>Kapsayıcı görüntülerini etiketleme

Mevcut görüntülerin listesini görüntülemek için [docker images](https://docs.docker.com/engine/reference/commandline/images/) komutunu kullanın.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Her kapsayıcı görüntüsünün, kayıt defterinin loginServer adıyla etiketlenmesi gerekir. Bu etiket, görüntü kayıt defterine kapsayıcı görüntüleri gönderilirken kullanılır.

loginServer adını almak için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Şimdi, `azure-vote-front` değerini, kapsayıcı kayıt defterinin loginServer’ıyla etiketleyin. Ayrıca, görüntü adının sonuna `:v1` ekleyin. Bu etiket, görüntü sürümünü belirtir.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Etiketledikten sonra, işlemi doğrulamak için [docker images] (https://docs.docker.com/engine/reference/commandline/images/)) komutunu çalıştırın.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Kayıt defterine görüntü gönderme

`azure-vote-front` görüntüsünü kayıt defterinize gönderin. 

Aşağıdaki örneği kullanarak, ortamınızda, ACR loginServer adını loginServer olarak değiştirin.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

Bu işlemin tamamlanması birkaç dakika sürer.

## <a name="list-images-in-registry"></a>Kayıt defterindeki görüntüleri listeleme

Azure Container Registry’nize gönderilen görüntülerin listesini döndürmek için [az acr repository list](/cli/azure/acr/repository#az_acr_repository_list) komutunu kullanın. Komutu ACR örneği adıyla güncelleştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```azurecli
Result
----------------
azure-vote-front
```

Sonra, belirli bir görüntünün etiketlerini görmek için [az acr repository show-tags](/cli/azure/acr/repository#show-tags) komutunu kullanın.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Çıktı:

```azurecli
Result
--------
v1
```

Öğretici tamamlandığında, kapsayıcı görüntüsü özel bir Azure Container Registry örneğinde depolanır. Sonraki öğreticilerde, bu görüntüyü ACR’den bir Kubernetes kümesine dağıtma işlemi açıklanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ACS Kubernetes kümesinde kullanılmak üzere bir Azure Container Registry hazırlanmıştır. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure Container Registry örneği dağıtıldı
> * ACR için kapsayıcı görüntüsü etiketlendi
> * Görüntü ACR’ye yüklendi

Azure’da Kubernetes kümesi dağıtmayla ilgili daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Kubernetes kümesi dağıtma](./container-service-tutorial-kubernetes-deploy-cluster.md)