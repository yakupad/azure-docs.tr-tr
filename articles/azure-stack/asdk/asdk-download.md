---
title: İndirin ve ayıklayın Azure Stack geliştirme Seti'ni (ASDK) | Microsoft Docs
description: İndirmeyi ve ayıklamayı Azure Stack geliştirme Seti'ni (ASDK) açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: b55bc7f6aab522a6313498b6fdccc88870796224
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213036"
---
# <a name="download-and-extract-the-azure-stack-development-kit-asdk"></a>İndirin ve Azure Stack geliştirme Seti'ni (ASDK) ayıklayın
Geliştirme Seti ana bilgisayarınız ASDK yüklemeye yönelik temel gereksinimleri karşıladığından emin olduktan sonra indirmeyi ve ayıklamayı ASDK dağıtım paketi Cloudbuilder.vhdx almak için sonraki adım olacaktır.

## <a name="download-the-asdk"></a>ASDK indirin
1. Yükleme başlamadan önce bilgisayarınızın aşağıdaki gereksinimleri karşıladığından emin olun:

  - Bilgisayarda en az 60 işletim sistemi diski için ayrıca boş dört ayrı, aynı mantıksal sabit disk sürücüler üzerinde disk alanı GB olmalıdır.
  - [.NET framework 4.6 (veya sonraki bir sürümü)](https://aka.ms/r6mkiy) yüklü olması gerekir.

2. [Başlangıç sayfasına gidin](https://azure.microsoft.com/overview/azure-stack/try/?v=try) , Azure Stack geliştirme Seti'ni indirme, ayrıntılarınızı girin ve ardından **Gönder**.
3. İndirme ve çalıştırma [Azure Stack Geliştirme Seti için dağıtım denetleyicisi](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) önkoşul denetleyicisini komut dosyası. Bu tek başına komut dosyası için Azure Stack geliştirme Seti'ni Kurulumu tarafından yapılan ön koşullar denetimleri geçer. Azure Stack Geliştirme Seti için daha büyük paketi yüklemeden önce donanım ve yazılım gereksinimleri karşılayıp onaylamak için bir yol sunar.
4. Altında **yazılım indirme**, tıklayın **Azure Stack geliştirme Seti'ni**.

  > [!NOTE]
  > ASDK indirme (AzureStackDevelopmentKit.exe) yaklaşık 10 GB'dir.

## <a name="extract-the-asdk"></a>ASDK ayıklayın
1. İndirme tamamlandıktan sonra tıklayın **çalıştırma** ASDK ayıklayıcısı (AzureStackDevelopmentKit.exe) başlatmak için.
2. Gözden geçirin ve görüntülenen Lisans Sözleşmesi'nden kabul **Lisans Sözleşmesi** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**.
3. Görüntülenen gizlilik bildirimi bilgileri gözden geçirin **önemli bildirim** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**.
4. Açık olarak ayıklanacak Azure Stack kurulum dosyalarının konumunu seçin **hedef konum seçin** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **sonraki**. Varsayılan konum *geçerli klasör*\Azure Stack Geliştirme Seti. 
5. Hedef konumun özetini inceleyin **ayıklamaya hazır** ayıklayıcısı Sihirbazı'nı ve ardından sayfanın **ayıklamak** (yaklaşık 25 GB) CloudBuilder.vhdx ayıklanacak ve ThirdPartyLicenses.rtf dosyaları. Bu işlemin tamamlanması biraz zaman alabilir.
6. Kopyalayın veya CloudBuilder.vhdx dosya ASDK ana bilgisayarda C:\ sürücüsüne (C:\CloudBuilder.vhdx) kök dizinine taşıyın.

> [!NOTE]
> Dosyaları ayıklayın sonra silebilirsiniz. EXE ve. Sabit disk alanını kurtarmak için DEPO dosyaları'nı tıklatın. Veya ASDK tekrar dağıtmanız gerekirse dosyaları tekrar indirmeniz gerekmez, bu dosyaları yedekleyin.


## <a name="next-steps"></a>Sonraki adımlar
[ASDK ana bilgisayarını hazırlayın](asdk-prepare-host.md)