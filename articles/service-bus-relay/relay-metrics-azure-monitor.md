---
title: Azure İzleyici (Önizleme) Azure geçişi ölçümlerde | Microsoft Docs
description: Azure geçişi izlemek için Azure izleme kullanın
services: service-bus-relay
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 3bcea7ee15bea137fecc55f1e641b84e2e72d3ff
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247447"
---
# <a name="azure-relay-metrics-in-azure-monitor-preview"></a>Azure İzleyici (Önizleme), Azure geçişi ölçümleri

Azure geçişi ölçümleri, Azure aboneliğinizdeki kaynakları durumunu sağlar. Zengin ölçüm verileri ile genel Relay kaynakları, yalnızca ad alanı düzeyinde, aynı zamanda varlık düzeyinde durumunu değerlendirebilirsiniz. Bu istatistikler Azure geçişi durumunu izlemek için yardımcı önemli olabilir. Ölçümler, Azure desteğine başvurun gerek kalmadan kök neden sorunlarını da yardımcı olabilir.

Azure İzleyici, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için [Microsoft Azure'da izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve [almak Azure İzleyici ölçümleri .NET ile](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) GitHub üzerinde örnek.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Ya da erişim ölçümleri ile yapabilecekleriniz [Azure portalında](https://portal.azure.com), veya Azure İzleyici API'leri (REST ve .NET) ve Operations Management Suite ve Event Hubs gibi analiz çözümleri kullanın. Daha fazla bilgi için [Azure İzleyici ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

Ölçümler, varsayılan olarak etkindir ve en son 30 Günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu yapılandırılan [tanılama ayarları](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) Azure İzleyici'de.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümlerini

Zaman içinde ölçümleri izleyebilirsiniz [Azure portalında](https://portal.azure.com). Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için ad alanını seçin ve ardından **ölçümleri (Peview)**. 

Boyutlar destekleyen ölçümler için istenen boyut değeri ile filtrelemesi gerekir.

## <a name="billing"></a>Faturalandırma

Ölçümleri kullanarak Azure İzleyici'de önizleme şu an ücretsizdir. Ölçüm verilerini alma, ek çözümleri kullanırsanız, ancak bu çözümler tarafından faturalandırılırsınız. Örneğin, ölçüm verileri bir Azure depolama hesabına arşivleme, Azure Depolama tarafından faturalandırılır. Gelişmiş analiz için ölçüm verileri Log analytics'e akış sahipse Log Analytics tarafından ayrıca faturalandırılır.

Aşağıdaki ölçümler size sistem hizmetinizin genel bakış sunar. 

> [!NOTE]
> Farklı bir adla geçildiği size çeşitli ölçümleri kullanımdan. Bu, başvurularını güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretli ölçümleri, bundan sonra desteklenmeyecek.

Azure İzleyici, tüm ölçüm değerleri dakikada gönderilir. Zaman ayrıntı düzeyi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm Azure geçişi ölçümler için desteklenen zaman aralığı 1 dakikadır.

## <a name="connection-metrics"></a>Bağlantı ölçümü

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Microsoft.Relay başarılı (Önizleme) | Azure geçişi için belirtilen bir süredeki yapılan başarılı bir dinleyici bağlantı sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay-Senderconnections'da (Önizleme)|Belirli bir dönem boyunca dinleyici bağlantılarında istemci hataları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay-ServerError (Önizleme)|Belirli bir dönem boyunca dinleyici bağlantılarda sunucu hataları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections başarılı (Önizleme)|Belirtilen bir zaman dilimi içerisinde yapılan başarılı gönderen bağlantılarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections-Senderconnections'da (Önizleme)|Belirli bir dönem boyunca gönderen bağlantılarında istemci hataları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections-ServerError (Önizleme)|Gönderen bağlantısı belirli bir süre içinde sunucu hataları sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay-TotalRequests (Önizleme)|Dinleyici bağlantı belirli bir dönem boyunca toplam sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Microsoft.Relay SenderConnections-TotalRequests (Önizleme)|Belirtilen bir süredeki Gönderenler tarafından yapılan bağlantı istekleri.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ActiveConnections (Önizleme)|Belirtilen bir süre boyunca etkin bağlantı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ActiveListeners (Önizleme)|Belirtilen bir süre boyunca etkin dinleyiciler sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerDisconnects (Önizleme)|Belirli bir dönem boyunca bağlantısı kesilmiş dinleyicileri sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderDisconnects (Önizleme)|Belirli bir dönem boyunca bağlantısı kesilmiş Gönderenler sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="memory-usage-metrics"></a>Bellek kullanım ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|BytesTransferred (Önizleme)|Belirtilen bir süredeki aktarılan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure geçişi, Azure İzleyicisi'nde ölçümler için aşağıdaki boyutlarını destekler. Boyutları için ölçümlerinizi eklemek isteğe bağlıdır. Ölçümleri, boyutları eklemezseniz, ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Azure geçişi, Mesajlaşma varlıkları ad alanı altında destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




