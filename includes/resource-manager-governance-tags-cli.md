---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b9484336add0719749e9f0af56bdd70fa3906ef5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38752668"
---
Kaynak grubuna iki etiket eklemek için [az group update](/cli/azure/group#az_group_update) komutunu kullanın:

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Şimdi üçüncü bir etiket istediğinizi varsayalım. Komutu yeni etiketle bir kez daha çalıştırın. Bu, mevcut etiketlere eklenir.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Kaynaklar, kaynak grubundan etiketleri devralmaz. Şu anda, kaynak grubunuzun üç etiketi vardır ama kaynakların hiç etiketi yoktur. Kaynak grubundaki tüm etiketleri kaynaklarına uygulamak ve kaynaklardaki mevcut etiketleri korumak için aşağıdaki betiği kullanın:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

Alternatif olarak, mevcut etiketleri korumadan kaynak grubundaki etiketleri kaynaklara uygulayabilirsiniz:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Birkaç değeri tek etikette birleştirmek için bir JSON dizesi kullanın.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Kaynak grubundaki tüm etiketleri kaldırmak için şunu kullanın:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
