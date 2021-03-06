---
title: Azure yığınında komut satırı ile şablonlarını dağıtma | Microsoft Docs
description: Platformlar arası komut satırı arabirimi (CLI) şablonları Azure yığınına dağıtmak için nasıl kullanılacağını öğrenin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 761e09889a230642c42697b6bc4f96dc32fe03a0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30316204"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Azure komut satırı kullanılarak yığınında şablonlarını dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları Azure yığın Geliştirme Seti dağıtmak için komut satırını kullanın. Azure Resource Manager şablonları dağıtın ve tek ve eşgüdümlü bir işlemle uygulamanızda için tüm kaynakları sağlayın.

## <a name="before-you-begin"></a>Başlamadan önce
 - [Yüklemek ve bağlamak](azure-stack-version-profiles-azurecli2.md) Azure CLI ile Azure yığınına
 - Dosyaları karşıdan *azuredeploy.json* ve *azuredeploy.parameters.json* gelen [depolama hesabı örnek şablonu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Şablon dağıtma
Burada bu dosyaları indirilir ve şablonu dağıtmak için aşağıdaki komutu çalıştırın klasöre gidin:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Bu komut kaynak grubuna şablon dağıtır **cliRG** Azure yığın POC'ın varsayılan konumu.

## <a name="validate-template-deployment"></a>Şablon dağıtımı doğrulama
Bu kaynak grubu ve depolama hesabı görmek için aşağıdaki komutları kullanın:

    azure group list

    azure storage account list




