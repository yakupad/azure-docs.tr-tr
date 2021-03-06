---
title: Görevler ve düğümleri - Azure Batch için durumları sayısı | Microsoft Docs
description: Azure Batch görevlerin durumunu sayısı ve işlem düğümleri, yönetmek ve Batch çözümlerini izlemek için.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 06/29/2018
ms.author: danlep
ms.openlocfilehash: f4bad3d7058e82a246afce9502d275c7d485cb88
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009178"
---
# <a name="monitor-batch-solutions-by-counting-tasks-and-nodes-by-state"></a>Görevler ve düğümleri duruma göre sayma tarafından Batch çözümlerini izleme

İzleme ve büyük ölçekli Azure Batch çözümlerinin yönetmek için çeşitli durumları kaynakların doğru sayıları gerekir. Azure Batch, toplu işlemi için bu alınacağı verimli işlemler sağlar *görevleri* ve *işlem düğümlerine*. Bu işlemler, görevler veya düğümleri büyük Koleksiyonlar hakkında ayrıntılı bilgi döndürmek için büyük olasılıkla zaman API çağrıları yerine kullanın.

* [Görev sayıları alma] [ rest_get_task_counts] etkin, çalışan ve tamamlanmış bir iş görevleri ve başarılı veya başarısız görevlerin bir toplam sayısını alır. 

  Her durumda görevleri sayılarak daha kolayca işin ilerleme durumunu görüntülemek için bir kullanıcı, veya beklenmeyen gecikmeler veya iş etkileyebilecek hataları algılayın. Al görevi sayar, Batch hizmeti API Sürüm 2017-06-01.5.1 ve ilgili SDK'lar ve Araçlar itibariyle kullanılabilir.

* [Liste düğüm havuzu Sayılmaktadır] [ rest_get_node_counts] alır ayrılmış ve düşük öncelikli işlem çeşitli durumlarda düğümleri her havuzda: oluşturma, boşta, çevrimdışı, etkisiz, yeniden başlatma, görüntüsü yeniden oluşturuluyor, başlangıç ve diğerleri. 

  Her durumda düğüm sayılarak işlerinizi çalıştırmak için yeterli bilgi işlem kaynaklarına sahip ve havuzlarınızı ile ilgili olası sorunları belirlemenize, belirleyebilirsiniz. Liste düğüm havuzu sayıları, Batch hizmeti API sürümü 2018-03-01.6.1 ve ilgili SDK'lar ve Araçlar itibariyle kullanılabilir.

Görev sayısı veya düğüm sayısı işlemleri desteklemez hizmetini bir sürümünü kullanıyorsanız, bu kaynakları saymak için bir liste sorgusu kullanın. Ayrıca uygulamalar, dosyaları ve işleri gibi diğer Batch kaynakları hakkında bilgi almak için bir liste sorgusu kullanın. Liste sorguları için filtrelerini uygulama hakkında daha fazla bilgi için bkz. [Oluştur sorguları listelemek için Batch kaynaklarını verimli bir şekilde](batch-efficient-list-queries.md).

## <a name="task-state-counts"></a>Görev durumu sayısı

Görev sayıları alma işlemi görevleri aşağıdaki durumlara göre sayar:

- **Etkin** -kuyruğa alınmış ve şunları çalıştırın, ancak şu anda bir işlem düğümüne yüklenecek atanmamış bir görev. Ayrıca görevdir `active` ise [üst göreve bağlı](batch-task-dependencies.md) , henüz tamamlanmadı. 
- **Çalışan** -göreve atanan bir işlem düğümüne yüklenecek, ancak henüz tamamlanmadı. Bir görev olarak sayılır `running` durumuna olduğunda ya da `preparing` veya `running`belirtildiği gibi [bir görev hakkında bilgi alma] [ rest_get_task] işlemi.
- **Tamamlanan** -artık, ya da başarıyla tamamlandı veya başarısız tamamlandı ve yeniden deneme sınırına da bitti çalıştırmak uygun bir görev. 
- **Başarılı** -görevi yürütme sonucu olan bir görev `success`. Batch, görev başarılı veya denetleyerek başarısız olduğunu belirler `TaskExecutionResult` özelliği [executionInfo] [ rest_get_exec_info] özelliği.
- **Başarısız** görevi yürütme sonucu olan bir görev `failure`.

Aşağıdaki .NET kod örneği, duruma göre görev sayıları alma işlemi gösterilmektedir: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

Görev sayıları için bir işi almak için REST için benzer bir desen ve diğer desteklenen dilleri kullanabilirsiniz. 
 

### <a name="consistency-checking-for-task-counts"></a>Tutarlılık için görev sayıları denetleniyor

Batch, görev durumu sayıları için ek doğrulama tutarlılık denetimleri sistemin birden çok bileşen karşı gerçekleştirerek sağlar. Olası durumunda tutarlılık hataları bulur, tutarlılık denetimi sonuçlarına göre görevleri sayar alma işleminin sonucu Batch düzeltir.

`validationStatus` Yanıt özelliği, Batch tutarlılık denetimi gerçekleştirip gerçekleştirmediğini gösterir. Durum sayıları sistemde tutulan gerçek durumları karşı toplu açmadıysa sonra `validationStatus` özelliği `unvalidated`. Performansla ilgili nedenlerden dolayı iş birden fazla 200.000 görevleri içeriyorsa, tutarlılık denetimi Batch gerçekleştirmez böylece `validationStatus` özelliği `unvalidated` bu durumda. (Sınırlı veri kaybı olasılığı düşük olduğu gibi görev sayısı bu durumda, mutlaka bir sorun değildir.) 

Toplama işlem hattı, bir görev durumu değiştiğinde değişiklik birkaç saniye içinde işler. Görev sayıları Al işlemi, bu süre içinde güncelleştirilmiş görev sayısı yansıtır. Toplama işlem hattı bir görev durumundaki bir değişikliği isabetsizliği varsa, ancak daha sonra bu değişiklik kadar sonraki doğrulama geçişini kayıtlı değil. Bu süre boyunca görev sayıları eksik olay nedeniyle biraz yanlış olabilir ancak bunlar sonraki doğrulama geçişte düzeltildi.

## <a name="node-state-counts"></a>Düğüm durumu sayar

Liste düğüm havuzu Sayılmaktadır işlemi sayıları işlem düğümleri her havuzda aşağıdaki durumlara göre. Ayrı toplam sayısı, adanmış düğümleri ve her havuzdaki düşük öncelikli düğümleri için sağlanır.

- **Oluşturma** -bir Azure ayrılmış VM, bir havuzu katılmak için henüz başlatılmadı.
- **Boşta** -şu anda bir görev çalışmıyor bir kullanılabilir bilgi işlem düğümü.
- **LeavingPool** -kullanıcı açıkça kaldırılmış olduğundan veya havuz yeniden boyutlandırma veya ölçeklendirme çalışmadığı için havuz bırakarak bir düğüm.
- **Çevrimdışı** -düğüm toplu yeni görevleri zamanlamak için kullanamazsınız.
- **Etkisiz** -Azure VM geri çünkü havuzdan kaldırıldı bir düşük öncelikli düğüm. A `preempted` düğümü yeniden değiştirme düşük öncelikli VM kapasitesi kullanılabilir olduğunda.
- **Yeniden başlatma** -yeniden başlatılıyor bir düğüm.
- **Yeniden görüntü** -bir düğüm üzerinde işletim sistemi yeniden.
- **Çalışan** -(dışında başlangıç görevinin) veya daha fazla görevin çalıştığı düğüm.
- **Başlangıç** -bir düğüm, Batch hizmeti başlatılıyor. 
- **StartTaskFailed** -bir düğümde [başlangıç görevi] [ rest_start_task] başarısız oldu ve tüm yeniden deneme tükendi ve üzerinde `waitForSuccess` başlangıç görevi ayarlayın. Düğüm, görevleri çalıştırmak için kullanılabilir değil.
- **Bilinmeyen** - düğüm Batch hizmeti ile iletişimi kaybedilen ve durumu değil bilinen.
- **Kullanılamayan** -bir düğümü için Görev Yürütme, hataları nedeniyle kullanılamaz.
- **WaitingForStartTask** -bir düğümü başlangıç görevi kullanmaya çalışan, ancak `waitForSuccess` kümesi ve bir başlangıç görevi tamamlanmadı.

Aşağıdaki C# kod parçacığı, düğüm sayısı geçerli hesaptaki tüm havuzları listesi gösterilmektedir:

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts())
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
Aşağıdaki C# kod parçacığı, geçerli hesaptaki verilen havuz düğümü sayıları listesinde gösterilmiştir.

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts(new ODATADetailLevel(filterClause: "poolId eq 'testpool'")))
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
Havuzlar için düğüm sayısını almak için REST için benzer bir desen ve diğer desteklenen dilleri kullanabilirsiniz.
 
## <a name="next-steps"></a>Sonraki adımlar

* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve hizmetin özelliklerine genel bir bakış sağlar.

* Batch kaynaklarını liste sorguları için filtrelerini uygulama hakkında daha fazla bilgi için bkz: [Oluştur sorguları listelemek için Batch kaynaklarını verimli bir şekilde](batch-efficient-list-queries.md).


[rest_get_task_counts]: /rest/api/batchservice/job/gettaskcounts
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
[rest_get_task]: /rest/api/batchservice/task/get
[rest_list_tasks]: /rest/api/batchservice/task/list
[rest_get_exec_info]: /rest/api/batchservice/task/get#executionInfo
[rest_start_task]: /rest/api/batchservice/pool/add#starttask

