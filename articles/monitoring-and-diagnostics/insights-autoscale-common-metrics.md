---
title: Otomatik ölçeklendirme ortak ölçümleri
description: Hangi ölçümleri otomatik ölçeklendirmeyi için yaygın olarak kullanılan bilgi bulut hizmetlerinizi, sanal makineler ve Web uygulamaları.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/6/2016
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 7b6f454a8d4c8794b8c56494fd9ed573f8b79852
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262248"
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure İzleyici otomatik ölçeklendirmeyi ortak ölçümleri
Azure İzleyici otomatik ölçeklendirmeyi telemetri verilerini (ölçüm) esas yukarı veya aşağı çalışan örnek sayısını ölçeklendirmek sağlar. Bu belgeyi kullanmak isteyebilirsiniz ortak ölçümleri açıklar. Bulut Hizmetleri ve sunucu grupları için Azure portalında tarafından ölçeklendirmek için kaynak ölçüm seçebilirsiniz. Ancak, aynı zamanda herhangi bir ölçümü tarafından ölçeklendirmek için farklı bir kaynak seçebilirsiniz.

Azure İzleyici otomatik ölçeklendirme uygular yalnızca [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/), ve [uygulama hizmeti - Web Apps](https://azure.microsoft.com/services/app-service/web/). Diğer Azure hizmetleriyle farklı ölçekleme yöntemlerini kullanın.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Resource Manager tabanlı VM'ler için ölçümleri işlem
Varsayılan olarak, Resource Manager tabanlı sanal makineler ve sanal makine ölçek kümeleri temel (ana bilgisayar düzeyinde) ölçümleri yayma. Ayrıca, bir Azure VM ve VMSS için tanılama veri toplama yapılandırdığınızda, Azure tanılama uzantısını da konuk işletim sistemi performans sayaçları (genellikle "Konuk işletim sistemi ölçümleri" da bilinir) yayar.  Bu ölçümleri otomatik ölçeklendirme kuralları kullanır.

Kullanabileceğiniz `Get MetricDefinitions` VMSS kaynağınız için kullanılabilir ölçümleri görüntülemek üzere API/PoSH/CLI.

VM ölçek kümesi kullanıyorsanız ve listelenen belirli bir ölçüm görmüyorum durumunda büyük olasılıkla *devre dışı* tanılama uzantı.

Belirli bir ölçü değil yaşanıyorsa örneklenen veya istediğiniz sıklıkta transfer, tanılama yapılandırması güncelleştirebilirsiniz.

Yukarıdaki her iki durumda true ise, daha sonra gözden [kullanım Windows çalıştıran bir sanal makine Azure Tanılama'yı etkinleştirmek için PowerShell](../virtual-machines/windows/ps-extensions-diagnostics.md) yapılandırmak ve ölçüm etkinleştirmek için Azure VM tanılama uzantısını güncelleştirmek için PowerShell hakkında. Bu makale, ayrıca bir örnek tanılama yapılandırma dosyası içerir.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Resource Manager tabanlı Windows ve Linux VM'ler için ana ölçümleri
Aşağıdaki ana bilgisayar düzeyinde ölçümleri varsayılan olarak Azure VM ve VMSS için hem Windows hem de Linux örneklerde gösterilen. Bu ölçümler Azure VM açıklamaktadır, ancak Azure VM ana bilgisayardan yerine Konuk sanal makinede yüklü Aracısı üzerinden toplanır. Bu ölçümleri otomatik ölçeklendirmeyi kurallarında kullanabilir.

- [Resource Manager tabanlı Windows ve Linux VM'ler için ana ölçümleri](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)
- [Resource Manager tabanlı Windows ve Linux VM ölçek kümesi için ana ölçümleri](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Konuk işletim sistemi ölçümleri Resource Manager tabanlı Windows VM'ler
Azure'da VM oluşturduğunuzda tanılama tanılama uzantısını kullanarak etkinleştirilir. Tanılama uzantısını VM içinde alınan ölçümler bir dizi yayar. Başka bir deyişle, varsayılan olarak gösterilen değil ölçümleri dışına otomatik ölçekleme yapabilirsiniz.

PowerShell'de aşağıdaki komutu kullanarak, ölçümleri listesi oluşturabilirsiniz.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Aşağıdaki ölçümler için bir uyarı oluşturabilirsiniz:

| Ölçüm Adı | Birim |
| --- | --- |
| \Processor(_Total)\% Processor Time |Yüzde |
| \Processor(_Total)\% ayrıcalıklı zaman |Yüzde |
| \Processor(_Total)\% kullanıcı zamanı |Yüzde |
| \Processor bilgileri (_Total) \Processor sıklığı |Sayı |
| \System\Processes |Sayı |
| \Process (_Total) \Thread sayısı |Sayı |
| \Process (_Total) \Handle sayısı |Sayı |
| \Memory\% Kaydedilmiş Bayt yüzdesi |Yüzde |
| \Memory\Available Bytes |Bayt |
| \Memory\Committed bayt |Bayt |
| \Memory\Commit sınırı |Bayt |
| Disk belleği \Memory\Pool bayt |Bayt |
| \Memory\Pool belleği olmayan havuz bayt sayısı |Bayt |
| \PhysicalDisk(_Total)\% disk zamanı |Yüzde |
| \PhysicalDisk(_Total)\% disk okuma süresi |Yüzde |
| \PhysicalDisk(_Total)\% disk yazma süresi |Yüzde |
| \PhysicalDisk (_Total) \Disk aktarımı/sn |CountPerSecond |
| \PhysicalDisk (_Total) \Disk Okuma/sn |CountPerSecond |
| \PhysicalDisk (_Total) \Disk Yazma/sn |CountPerSecond |
| \PhysicalDisk (_Total) \Disk bayt/sn |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk Okuma Bayt/sn |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk Yazma Bayt/sn |BytesPerSecond |
| \PhysicalDisk (_Total) \Avg. Disk Kuyruğu Uzunluğu |Sayı |
| \PhysicalDisk (_Total) \Avg. Disk okuma sırası uzunluğu |Sayı |
| \PhysicalDisk (_Total) \Avg. Disk yazma sırası uzunluğu |Sayı |
| \LogicalDisk(_Total)\% boş alanı |Yüzde |
| \LogicalDisk (_Total) \Free megabayt |Sayı |

### <a name="guest-os-metrics-linux-vms"></a>Konuk işletim sistemi ölçümleri Linux VM'ler
Azure'da VM oluşturduğunuzda, tanılama tanılama uzantısını kullanarak varsayılan olarak etkindir.

PowerShell'de aşağıdaki komutu kullanarak, ölçümleri listesi oluşturabilirsiniz.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Aşağıdaki ölçümler için bir uyarı oluşturabilirsiniz:

| Ölçüm Adı | Birim |
| --- | --- |
| \Memory\AvailableMemory |Bayt |
| \Memory\PercentAvailableMemory |Yüzde |
| \Memory\UsedMemory |Bayt |
| \Memory\PercentUsedMemory |Yüzde |
| \Memory\PercentUsedByCache |Yüzde |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Bayt |
| \Memory\PercentAvailableSwap |Yüzde |
| \Memory\UsedSwap |Bayt |
| \Memory\PercentUsedSwap |Yüzde |
| \Processor\PercentIdleTime |Yüzde |
| \Processor\PercentUserTime |Yüzde |
| \Processor\PercentNiceTime |Yüzde |
| \Processor\PercentPrivilegedTime |Yüzde |
| \Processor\PercentInterruptTime |Yüzde |
| \Processor\PercentDPCTime |Yüzde |
| \Processor\PercentProcessorTime |Yüzde |
| \Processor\PercentIOWaitTime |Yüzde |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Saniye |
| \PhysicalDisk\AverageWriteTime |Saniye |
| \PhysicalDisk\AverageTransferTime |Saniye |
| \PhysicalDisk\AverageDiskQueueLength |Sayı |
| \NetworkInterface\BytesTransmitted |Bayt |
| \NetworkInterface\BytesReceived |Bayt |
| \NetworkInterface\PacketsTransmitted |Sayı |
| \NetworkInterface\PacketsReceived |Sayı |
| \NetworkInterface\BytesTotal |Bayt |
| \NetworkInterface\TotalRxErrors |Sayı |
| \NetworkInterface\TotalTxErrors |Sayı |
| \NetworkInterface\TotalCollisions |Sayı |

## <a name="commonly-used-web-server-farm-metrics"></a>Yaygın olarak kullanılan Web (sunucu grubu) ölçümleri
Otomatik ölçeklendirme Http sırası uzunluğu gibi ortak web sunucusu ölçümlerini göre de gerçekleştirebilirsiniz. Buna ait ölçüm adı **HttpQueueLength**.  Aşağıdaki bölümde kullanılabilir sunucu grubu (Web uygulamaları) ölçümleri listelenmektedir.

### <a name="web-apps-metrics"></a>Web uygulamaları ölçümleri
PowerShell'de aşağıdaki komutu kullanarak, Web uygulamaları ölçümleri listesi oluşturabilirsiniz.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Uyar veya ölçeklendirmek bu ölçümleri tarafından.

| Ölçüm Adı | Birim |
| --- | --- |
| CpuPercentage |Yüzde |
| MemoryPercentage |Yüzde |
| DiskQueueLength |Sayı |
| HttpQueueLength |Sayı |
| BytesReceived |Bayt |
| BytesSent |Bayt |

## <a name="commonly-used-storage-metrics"></a>Yaygın olarak kullanılan depolama ölçümleri
Depolama sıradaki ileti sayısı depolama sırası uzunluğu tarafından ölçeklendirebilirsiniz. Depolama sırası uzunluğu özel ölçüm ve eşik örneği başına iletilerinin sayısı. Sıradaki iletilerin toplam sayısını 200 Örneğin, iki örnek varsa ve Eşiği 100'e ayarlanırsa, ölçekleme oluşur. Örneği başına 100 iletileri, 200 veya daha fazla kadar ekleyen 120 ve 80 veya herhangi diğer birleşimi olabilir.

Bu ayar Azure portalında yapılandırmak **ayarları** dikey. VM ölçek kümesi için kullanılacak Resource Manager şablonu otomatik ölçeklendirme ayarında güncelleştirebilirsiniz *metricName* olarak *ApproximateMessageCount* ve depolama kuyruğu Kimliğini geçirin *metricResourceUri*.

Örneğin, bir Klasik depolama hesabıyla otomatik ölçeklendirme ayarı metricTrigger şunlardır:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

MetricTrigger (Klasik olmayan) depolama hesabı için aşağıdakileri içerir:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Yaygın olarak kullanılan hizmet veri yolu ölçümleri
Service Bus kuyruğundaki iletileri sayısı Service Bus sırası uzunluğu tarafından ölçeklendirebilirsiniz. Hizmet veri yolu kuyruğu uzunluğu özel ölçüm ve eşik örneği başına iletilerinin sayısı. Sıradaki iletilerin toplam sayısını 200 Örneğin, iki örnek varsa ve Eşiği 100'e ayarlanırsa, ölçekleme oluşur. Örneği başına 100 iletileri, 200 veya daha fazla kadar ekleyen 120 ve 80 veya herhangi diğer birleşimi olabilir.

VM ölçek kümesi için kullanılacak Resource Manager şablonu otomatik ölçeklendirme ayarında güncelleştirebilirsiniz *metricName* olarak *ApproximateMessageCount* ve depolama kuyruğu Kimliğini geçirin *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Hizmet veri yolu için kaynak grubu kavram yok ancak Azure Resource Manager her bölge varsayılan bir kaynak grubu oluşturur. Kaynak grubu genellikle 'Default - ServiceBus-[Bölge]' biçimindedir. Örneğin, 'Varsayılan-ServiceBus-EastUS', 'Varsayılan-ServiceBus-WestUS', 'Varsayılan-ServiceBus-AustraliaEast' vb.
>
>
