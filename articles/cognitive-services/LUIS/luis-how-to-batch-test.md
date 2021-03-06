---
title: Toplu test LUIS uygulamanızı - Azure | Microsoft Docs
description: Language Understanding (LUIS) test toplu konuşma yanlış hedefleri ve varlıkları bulmak için kullanın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: diberry
ms.openlocfilehash: 2c648cdd82f89a9646fa0b311a7f1f68dd4bc4a9
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223590"
---
# <a name="batch-testing"></a>Toplu işe testi
 Toplu test LUIS, performansı ölçmek için geçerli eğitilen modelinizde kapsamlı bir testtir. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Toplu test etmek için bir veri kümesi dosyasını içeri aktarma

1. Seçin **Test** üst çubuk ve ardından **test paneli toplu**.

    ![Batch test bağlantısına](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Seçin **alma dataset**. **Yeni veri kümesi alma** iletişim kutusu görüntülenir. Seçin **Dosya Seç** doğru içeren bir JSON dosyası bulun [JSON biçimine](luis-concept-batch-test.md#batch-file-format) içeren *en fazla 1.000* konuşma test etmek için.

    ![Veri kümesi dosyasını içeri aktar](./media/luis-how-to-batch-test/batchtest-importset.png)

    Alma hataları, tarayıcının en üstündeki kırmızı bildirim çubuğunda bildirilir. Bir içeri aktarma hataları olduğunda, herhangi bir veri kümesi oluşturulur. Daha fazla bilgi için [sık karşılaşılan](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. İçinde **veri kümesi adı** veri kümesi dosyanız için bir ad girin. Veri kümesi dosyası içeren bir **konuşma dizisi** dahil olmak üzere *hedefi etiketli* ve *varlıkları*. Gözden geçirme [örnek toplu iş dosyası](luis-concept-batch-test.md#batch-file-format) söz dizimi. 

4. **Done** (Bitti) öğesini seçin. Veri kümesi dosyası eklenir.

## <a name="run-rename-export-or-delete-dataset"></a>Çalıştırın, yeniden adlandırma, dışarı aktarma veya veri kümesini Sil
Üç nokta çalıştırın, yeniden adlandırma, dışarı aktarma veya veri kümesini silmek için kullanın (***...*** ) veri kümesi satırın sonunda düğmesi.

![Veri kümesi eylemleri](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Toplu test eğitilen uygulamanızı çalıştırma

Testi çalıştırmak için veri kümesi adı seçin. Test tamamlandığında, bu satır kümesinin test sonucu görüntüler.

![Toplu Test sonucu](./media/luis-how-to-batch-test/run-test.png)

İndirilebilir dataset toplu test etmek için karşıya yüklenen aynı dosyadır.

|Durum|Anlamı|
|--|--|
|![Başarılı test yeşil daire simgesine](./media/luis-how-to-batch-test/batch-test-result-green.png)|Tüm konuşma başarılı olur.|
|![Başarısız olan test x simgesi kırmızı](./media/luis-how-to-batch-test/batch-test-result-red.png)|En az bir utterance amacını tahmin eşleşmedi.|
|![Test simgesi hazır](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Test çalıştırmak hazırdır.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Toplu test sonuçlarını görüntüle 
Toplu test sonuçlarını gözden geçirmek için seçin **bkz sonuçları**.

![Toplu test sonuçları](./media/luis-how-to-batch-test/run-test-results.png)

<!--
 Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. 

Green points indicate correct prediction, and red ones indicate incorrect prediction.

The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.

-->


<a name="filter-chart-results-by-intent-or-entity"></a> ## Grafik sonuçlarını filtreleme

Grafiğin belirli bir amaç veya varlık tarafından filtre uygulamak için sağ taraftaki filtre panelinde hedefi veya varlık'ı seçin. Yaptığınız seçime göre grafiğinde veri noktaları ve bunların dağıtım güncelleştirin. 
 
![Görselleştirilen toplu Test sonucu](./media/luis-how-to-batch-test/filter-by-entity.png) 

## <a name="view-single-point-utterance-data"></a>Tek nokta utterance verileri görüntüleme
Grafikte, kendi tahmin kesin puanı görmek için bir veri noktasının gelin. Sayfanın alt kısmındaki konuşma listesinde karşılık gelen kendi utterance almak için bir veri noktasını seçin. 

![Seçili utterance](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Bölüm verileri görüntüleme
Bölüm adı gibi dört bölüm grafikte seçin **yanlış pozitif** grafiğin sağ üst. Grafiğin altında bu bölümdeki tüm konuşma listesinde grafiğin altındaki görüntüler. 

![Seçili konuşma bölümü tarafından](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Bu bir önceki resimde, utterance `switch on` TurnAllOn hedefi ile etiketlenir, ancak tahmin hedefi hiçbiri aldı. Bu, TurnAllOn hedefi beklenen tahminde bulunmak için daha fazla örnek konuşma gereken bir göstergesidir. 

Kırmızı grafiğin iki bölüm beklenen tahmini eşleşmedi konuşma gösterir. Bu konuşma belirtmek hangi LUIS daha fazla eğitimden geçmesi gerekir. 

Yeşil grafikte iki bölümlerini beklenen tahmin ile eşleşmedi.

## <a name="next-steps"></a>Sonraki adımlar

Test LUIS uygulamanızı doğru amaç ve varlıkları tanımıyor gösteriyorsa, daha fazla konuşma etiketleme ya da özellikler ekleyerek LUIS uygulamanızın performansını artırmak için çalışabilir. 

* [LUIS ile önerilen konuşma etiket](luis-how-to-review-endoint-utt.md) 
* [LUIS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 
* [Bu öğreticiyle test toplu anlama](luis-tutorial-batch-testing.md)
* [Batch kavramları test öğrenin](luis-concept-batch-test.md).