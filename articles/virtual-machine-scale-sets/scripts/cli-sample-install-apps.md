---
title: Azure CLI 2.0 Örnekleri - Uygulama yükleme | Microsoft Docs
description: Azure CLI 2.0 Örnekleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 4c55fe94f3bfa6e21a8bc18923012083b37c5f66
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38697475"
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Azure CLI 2.0 ile sanal makine ölçek kümesine uygulama yükleme
Bu betik Ubuntu çalıştıran bir sanal makine ölçek kümesi oluşturur ve Özel Betik Uzantısı kullanarak temel bir web uygulaması yükler. Betiği çalıştırdıktan sonra web uygulamasına web tarayıcısı üzerinden erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/install-apps/install-apps.sh "Install apps into a scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, bir kaynak grubu, sanal makine ölçek kümesi ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/ad/group#az_ad_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vmss create](/cli/azure/vmss#az_vmss_create) | Sanal makine ölçek kümesi oluşturur ve bunu sanal ağa, alt ağa ve ağ güvenlik grubuna bağlar. Birden çok sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini de belirtir.  |
| [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) | Her sanal makine örneğinde veri disklerini hazırlayan bir betik çalıştırmak için Azure Özel Betik Uzantısı’nı yükler. |
| [az network lb rule create](/cli/azure/network/lb/rule#az_network_lb_rule_create) | 80 numaralı TCP bağlantı noktası üzerindeki trafiği ölçek kümesindeki sanal makine örneklerine dağıtmak için bir yük dengeleyici kuralı oluşturur. |
| [az network public-ip show](/cli/azure/network/public-ip#az_network_public_ip_show) | Yük dengeleyici tarafından kullanılan atanan genel IP adresi hakkında bilgi alır. |
| [az group delete](/cli/azure/ad/group#delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
Azure CLI 2.0 hakkında daha fazla bilgi için [Azure CLI 2.0 belgelerine](https://docs.microsoft.com/cli/azure/overview) bakın.

Ek sanal makine ölçek kümesi Azure CLI 2.0 betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../cli-samples.md) bulunabilir.
