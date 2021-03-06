---
title: Azure'a bağlı bir Video Indexer hesabı oluşturma | Microsoft Docs
description: Bu makalede, Azure'a bağlı bir Video Indexer hesabının nasıl oluşturulacağını gösterir.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 849a8dde07c248b6d7b317de5908b3a46dfad777
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259019"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Azure'a bağlı bir Video Indexer hesabı oluşturun

Video Indexer ücretsiz bir deneme hesabı kullanırken, video dizine ekleyebilir ve kota sınırlandırılırsınız. Bu makalede, bu sınırları serbest bırakır ve Kullandıkça Öde fiyatlandırması kullanıldığı bir Azure aboneliğine bağlı bir Video Indexer hesabının nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. 

    Henüz Azure aboneliğiniz yoksa kaydolmak [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

* Bir Azure Active Directory (AD) etki alanı. 

    Azure AD etki alanı yoksa, bu etki alanı, Azure aboneliğiniz ile oluşturun. Daha fazla bilgi için [Azure Active Directory'de özel etki alanı adlarını yönetme](../../active-directory/users-groups-roles/domains-manage.md)

* Kullanıcı ve Azure AD etki alanınızı üye. Bu üye, Video Indexer hesabınız Azure'a bağlanırken kullanacaksınız.

    Bu kullanıcı, bu ölçütleri karşılamalıdır:

    * Bir iş veya Okul hesabı, kişisel hesabı değil, outlook.com, live.com veya hotmail.com gibi bir Azure AD kullanıcısı olabilir.
        
        ![Tüm AAD kullanıcıları](./media/create-account/all-aad-users.png)

    *  Azure aboneliğinizde bir sahip rolü veya katkıda bulunan hem de kullanıcı erişimi yöneticisi rol üyesi olabilir. Bir kullanıcının ile 2 rolü iki kez eklenebilir. Katkıda bulunan ve bir kez kullanıcı erişimi Yöneticisi ile bir kez.

        ![Erişim denetimi](./media/create-account/access-control-iam.png)

## <a name="connect-to-azure"></a>Azure'a Bağlanma

1. Oturum, kullanıcı ile oturum açın ve tıklayarak **Azure'a bağlanma** düğmesi:

    ![Azure'a bağlanma](./media/create-account/connect-to-azure.png)

2. Abonelik listesi göründüğünde, kullanmak istediğiniz aboneliği seçin. 

    ![Video Indexer'ı Azure'a bağlanma](./media/create-account/connect-vi-to-azure-subscription.png)

3. Desteklenen konumlardan Azure bölgesini seçin: Batı ABD 2, Kuzey Avrupa ve Doğu Asya.
4. Altında **Azure Media Services hesabı**, aşağıdaki seçeneklerden birini seçin:

    * Yeni bir Media Services hesabı oluşturmak için Seç **yeni kaynak grubu oluştur**. Kaynak grubunuz için bir ad sağlayın.

        Azure, yeni bir Azure depolama hesabı dahil aboneliğinizde, yeni hesabı oluşturur. Varsayılan başlangıç yapılandırmasını akış uç noktası ve 10 S3 ayrılmış birim ile yeni Media Services hesabınız var.
    * Mevcut bir Media Services hesabını kullanmayı tercih **var olan kaynağı kullanın**. Hesapları listesinden hesabınızı seçin.

        Media Services hesabınızı, Video Indexer hesabınız ile aynı bölgede olması gerekir. Dizin oluşturma süresi ve aktarım hızının düşük olmasını en aza indirmek için ayrılmış birim sayısı ve türü ayarlamak **10 S3 ayrılmış birim** Media Services hesabı.
    * Bağlantınız el ile yapılandırmak için tıklayın **el ile yapılandırmaya geçiş** bağlamak ve gerekli bilgileri sağlayın:

    ![Video Indexer'ı Azure'a bağlanma](./media/create-account/connect-vi-to-azure-subscription-2.png)

5. İşiniz bittiğinde seçin **Connect**. Bu işlem birkaç dakika sürebilir. 

    Azure'a bağlandıktan sonra yeni Video Indexer hesabınız hesap listesinde görüntülenir:

    ![Yeni hesap](./media/create-account/new-account.png)

6. Yeni hesabınıza gidin: 

    ![Video Indexer hesabınız](./media/create-account/vi-account.png)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki Azure Media Services ilgili önemli noktalar geçerlidir:

* Yeni bir Media Services hesabına bağladıysanız, Azure aboneliğinizde yeni bir kaynak grubu, Media Services hesabı ve depolama hesabı görürsünüz.
* Yeni bir Media Services hesabına bağladıysanız, Video Indexer ortam ayarlamanız **ayrılmış birim** 10 S3 birimleri:

    ![Media Services için ayrılmış birimleri](./media/create-account/ams-reserved-units.png)

* Video Indexer mevcut bir Media Services hesabına bağladıysanız, var olan medya değiştirmez **ayrılmış birim** yapılandırma.

    Medya sayısını ve türünü ayarlamak ihtiyacınız olabilecek **ayrılmış birim**, planlanan yük göre. Yük yüksektir ve yeterli birimleri veya hızı yoksa, video işleme zaman aşımı hataları neden göz önünde bulundurun.

* Yeni bir Media Services hesabına bağladıysanız, Video Indexer otomatik olarak başlatan bir **akış uç noktası** da:

    ![Media Services akış uç noktası](./media/create-account/ams-streaming-endpoint.png)

* Video Indexer, var olan bir Media Services hesabına bağladıysanız, akış uç noktaları yapılandırma değiştirmez. Hiçbir çalışan varsa **akış uç noktası**, bu Media Services hesabı veya Video Indexer videoları mümkün olmayacaktır.

## <a name="use-video-indexer-apis-v2"></a>Video Indexer API v2

Deneme hesabınız ile ve/veya'ndaki yönergeleri takip ederek azure'a bağlı Video Indexer hesaplarınızı ile program aracılığıyla etkileşim kurabilir: [kullanım API'leri](video-indexer-use-apis.md).

Azure'a bağlanırken kullandığınız aynı Azure AD kullanıcı kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Çıkış JSON ayrıntılarını incelemek](video-indexer-output-json-v2.md).

