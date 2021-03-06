```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxPollingInterval|60000|Kuyruk yoklamaları arasındaki milisaniye cinsinden en büyük aralık.| 
|visibilityTimeout|0|Bir ileti işlenirken yeniden denemeler arasındaki zaman aralığını başarısız olur.| 
|batchSize|16|İşlevler çalışma zamanı aynı anda alır ve paralel olarak işler sıra iletilerinin sayısı. İşlenmekte olan sayı ne zaman alır aşağı `newBatchThreshold`, çalışma zamanı başka bir toplu iş alır ve bu iletileri işlemeye başlıyor. Eş zamanlı iletileri işlev işlenen sayısı, bu nedenle `batchSize` artı `newBatchThreshold`. Bu sınır, ayrı ayrı her kuyruk ile tetiklenen bir işlev için de geçerlidir. <br><br>Paralel yürütme bir kuyruğa alınan iletileri önlemek istiyorsanız, ayarlayabileceğiniz `batchSize` 1. Ancak, bu ayar yalnızca tek bir sanal makineye (VM) işlev uygulamanızın çalıştırdığı sürece eşzamanlılık ortadan kaldırır. İşlev uygulaması için birden çok VM Ölçeklendirmesi eşitlenene, her VM her kuyruk ile tetiklenen bir işlev bir örneğini çalıştırabilirsiniz.<br><br>En fazla `batchSize` 32'dir. | 
|maxDequeueCount|5|Kaç defa zehirli kuyruğuna taşınmadan önce bir iletiyi işlemeyi deneyin.| 
|newBatchThreshold|batchSize/2|Bu sayıya aynı anda işlenmekte olan ileti sayısını alır olduğunda, başka bir toplu iş çalışma zamanı alır.| 



