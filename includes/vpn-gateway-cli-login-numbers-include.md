---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 70a9e2a8f6327c0af92698b49d5b622bebba3661
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313617"
---
1. [az login](/cli/azure/#login) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Oturum açma hakkında daha fazla bilgi için bkz. [Azure CLI 2.0'ı kullanmaya başlama](/cli/azure/get-started-with-azure-cli).

  ```azurecli
  az login
  ```
2. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini listeleyin.

  ```azurecli
  az account list --all
  ```
3. Kullanmak istediğiniz aboneliği belirtin.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```