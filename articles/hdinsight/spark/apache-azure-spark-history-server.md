---
title: Genişletilmiş hata ayıklama ve tanılama Spark uygulamaları - Azure HDInsight için Spark geçmiş sunucusu kullanın | Microsoft Docs
description: Kullanım, hata ayıklama ve tanılama Spark uygulamaları - Azure HDInsight için Spark geçmiş sunucusu genişletilmiş.
services: hdinsight
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: ''
ms.topic: article
ms.date: 07/12/2018
ms.author: jejiang
ms.openlocfilehash: 61e857b01f75cb30e3d213703bd8ee8014d902d6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309929"
---
# <a name="use-extended-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Hata ayıklama ve tanılama Spark uygulamaları için genişletilmiş Spark geçmiş sunucusu kullanın

Bu makalede, hata ayıklama ve tanılama tamamlanmış ve çalışan Spark uygulamaları için Spark geçmiş sunucusu nasıl kullanılacağı hakkında yönergeler genişletilmiş sağlar. Uzantı şu anda veri ve graf sekmelerini içerir. Veri sekmesinde, kullanıcıların bir Spark işi, girdi ve çıktı verilerini denetleyebilirsiniz. Graf sekmesinde, kullanıcılar veri akışını kontrol edin ve iş grafiği yeniden yürütün.

## <a name="open-the-spark-history-server"></a>Spark geçmiş sunucusu açın

Spark geçmiş tamamlanmış ve çalışan Spark uygulamaları için kullanıcı Arabirimi web sunucusudur. 

### <a name="to-open-the-spark-history-server-web-ui-from-azure-portal"></a>Azure Portalı'ndan Spark geçmiş sunucusu Web kullanıcı arabirimini açmak için

1. Gelen [Azure portalında](https://portal.azure.com/), Spark kümesini açın. Daha fazla bilgi için [kümeleri Listele ve Göster](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Gelen **hızlı bağlantılar**, tıklayın **küme Panosu**ve ardından **Spark geçmiş sunucusu**. İstendiğinde, Spark küme için yönetici kimlik bilgilerini girin. 

    ![Spark geçmiş sunucusu](./media/apache-azure-spark-history-server/launch-history-server.png "Spark geçmiş sunucusu")

### <a name="to-open-the-spark-history-server-web-ui-by-url"></a>URL'ye göre Spark geçmiş sunucusu Web kullanıcı arabirimini açmak için
Aşağıdaki URL'ye atarak Spark geçmiş sunucusu açık değiştirin <ClusterName> müşteri Spark kümesi adı.

   ```
   https://<ClusterName>.azurehdinsight.net/sparkhistory
   ```

Spark geçmiş sunucusu web kullanıcı Arabirimi gibi görünür:

![HDInsight Spark geçmiş sunucusu](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)


## <a name="open-the-data-tab-from-spark-history-server"></a>Spark geçmiş sunucusundan veri sekmesini açın
İş Kimliği'ni seçin'a tıklayın **veri** aracı menüsünde veri görünüm elde edin.

+ Denetleme **girişleri**, **çıkışları**, ve **tablo işlemleri** sekmeleri ayrı olarak seçerek.

    ![Veri sekmeleri](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Düğmesine tıklayarak tüm satırları Kopyala **kopyalama**.

    ![Veri kopyalama](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Düğmesine tıklayarak tüm verileri CSV dosyası olarak kaydetme **csv**.

    ![Verileri kaydetme](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Arama alanı anahtar sözcükleri girerek **arama**, arama sonucu hemen görüntülenir.

    ![Veri arama](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Tabloyu sıralamak için daha fazla ayrıntı göstermek için bir satır genişletmek için artı işaretine tıklayın veya bir satır daraltmak için eksi işaretini tıklatın için sütun başlığına tıklayın.

    ![Veri tablosu](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Tek Dosyalı düğmesine tıklayarak indirme **kısmi indirme** , getirin, sağ tarafta seçili dosya için yerel olarak yüklenir dosya artık mevcut değilse, hata iletilerini görüntülemek için yeni bir sekme açılır.

    ![Veri indirme satırı](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Tam yol veya göreli yol seçerek kopyalama **tam yol Kopyala**, **göreli yolu Kopyala** indirme menüsünden genişletir. Azure data lake depolama dosyaları **Azure depolama Gezgini'nde Aç** Azure Depolama Gezgini'ni başlatın ve oturum açtığınızda klasörü bulun.

    ![Veri yolu Kopyala](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Tablo sayıya gitmek için çok zaman sayfaları tıklatın birçok satırları bir sayfasında görüntülenecek. 

    ![Veri sayfası](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Araç ipucunu göstermek için veri yanında soru işareti üzerine gelin veya daha fazla bilgi almak için soru işaretine tıklayın.

    ![Verileri daha fazla bilgi](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Tıklayarak sorunları ile geri bildirim gönder **geri bildirim sağlayın**.

    ![Grafik geri bildirim](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="open-the-graph-tab-from-spark-history-server"></a>Spark geçmiş sunucudan graf sekmesini açın
İş Kimliği'ni seçin'a tıklayın **grafik** iş graf görünümünü almak için aracı menüsünde.

+ Genel Bakış işinizin tarafından oluşturulan iş grafiğinin denetleyin. 

+ Varsayılan olarak, tüm işler gösterilir ve göre filtrelenmiş **iş kimliği**.

    ![graf iş kimliği](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ Varsayılan olarak, **ilerleme** olduğu belirlenirse, kullanıcı veri akışı seçerek iade edilemedi **okuma/yazılan** açılan listesinde **görünen**.

    ![grafik görüntüleme](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    Isı Haritası gösteren renkli grafik düğümünü görüntüler.

    ![graf ısı Haritası](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Tıklayarak işi kayıttan yürütme **kayıttan yürütme** Durdur düğmesini tıklayarak dilediğiniz zaman durdurmak ve düğme. Görev görünen farklı durumunu göstermek için renk, kayıttan yürütme:

    + Başarılı olanlar için yeşil: işi başarıyla tamamlandı.
    + Denenen için turuncu: başarısız olan ancak işin kesin sonucunu etkilemeyen bir görev örneği. Bu görevler sahip yinelenen veya daha sonra başarılı olabilecek örnekleri yeniden deneyin.
    + Kırmızı işlemi başarısız oldu: görev başarısız oldu.
    + Mavi çalıştırmak için: Görev çalışıyor.
    + İçin atlandı beyaz veya bekliyor: görev çalıştırılmayı bekliyor veya aşamayı atlandı.

    ![renk örneği çalıştırma grafiği](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    ![graf renk örneği, başarısız oldu](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]
    > Kayıttan yürütme her iş için izin verilir. Kayıttan yürütme, bir işin tüm aşama yok ya da tam henüz desteklenmiyor.


+ Fare kaydırma giren/çıkan iş grafiğinin yakınlaştırma veya **sığdırmak için Yakınlaştır** filtrelenecek sığdırmak için.
 
    ![sığdırmak için grafik yakınlaştırma](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Araç İpucu olmadığında görevler başarısız olduğunu görmek için graf düğümüyle gelin ve sahneye çıkarak aşama sayfasını açmak için tıklayın.

    ![Grafik araç ipucu](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ İş graf düğümüyle her aşaması aşağıdaki bilgileri görüntüler:
    + KİMLİĞİ.
    + Adı ya da açıklaması.
    + Toplam görev sayısı.
    + Okunan veriler: Giriş boyutu ve karışık Okuma boyutu.
    + Veri yazma: çıkış boyutu ve karışık yazma boyutu.
    + Yürütme süresi: yapılan ilk girişim başlangıç saati ve tamamlanma zamanı son girişimi arasındaki zaman.
    + Satır sayısı: Giriş kayıtları, toplam çıkış kayıtları, okunan kayıtlar karışık ve karışık yazma kayıtları.
    + İlerleme durumu.

    > [!NOTE]
    > Varsayılan olarak, iş graf düğümüyle her bir aşama (aşama yürütme süresi dışında) en son bilgileri görüntülenir, ancak kayıttan yürütme grafiği sırasında düğüm her girişimin bilgileri gösterir.

    > [!NOTE]
    > Veri boyutu okuma ve yazma 1 kullandığımız için MB = 1000 KB = 1000 * 1000 baytı.

+ Tıklayarak sorunları ile geri bildirim gönder **geri bildirim sağlayın**.

    ![Grafik geri bildirim](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="faq"></a>SSS

### <a name="1-revert-to-community-version"></a>1. Topluluk sürüme Döndür

Topluluk sürüme dönmek için aşağıdaki adımları uygulayın:

1. Ambari açık kümesinde. Tıklayın **Spark2** sol panelde.
2. Tıklayın **yapılandırmaları** sekmesi.
3. Grubu Genişlet **özel spark2-varsayılanları**.
4. Tıklayın **Özellik Ekle**, ekleme **spark.ui.enhancement.enabled=false**, kaydedin.
5. Özellik ayarlar **false** şimdi.
6. Tıklayın **Kaydet** yapılandırmayı kaydetmek için.

    ![özellik kapatır](./media/apache-azure-spark-history-server/sparkui-turn-off.png)

7. Tıklayın **Spark2** içinde sol paneli, altında **özeti** sekmesinde **Spark2 geçmiş sunucusu**.

    ![Sunucu1'ı yeniden başlatın](./media/apache-azure-spark-history-server/sparkui-restart-1.png) 

8. Geçmiş sunucusu tıklayarak yeniden başlatın. **yeniden** , **Spark2 geçmiş sunucusu**.

    ![Sunucu2'ı yeniden başlatın](./media/apache-azure-spark-history-server/sparkui-restart-2.png)  

9. Spark geçmiş sunucusu web kullanıcı arabirimini yenileyin, topluluk sürümü için döndürülecek.

### <a name="2-upload-history-server-event"></a>2. Geçmiş sunucusu olay karşıya yükleme

Geçmiş sunucusu hata çalıştırırsanız, olay sağlamak için adımları izleyin:
1. Olay tıklayarak indirin **indirme** geçmişi sunucu web kullanıcı Arabirimi.

    ![Olay indirin](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Tıklayın **geri bildirim sağlayın** veri/grafik sekmesinden.

    ![Grafik geri bildirim](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Başlık ve hatanın açıklamasını girin, zip dosyası düzen alanına sürükleyin ve ardından tıklayın **yeni sorunu Gönder**.

    ![Dosya sorunu](./media/apache-azure-spark-history-server/sparkui-file-issue.png)


### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Düzeltme senaryosu için jar dosyası yükseltme

Düzeltme ile yükseltmek istiyorsanız, aşağıdaki spark enhancement.jar* yükseltecek betiği kullanın.

**upgrade_spark_enhancement.sh**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

**Kullanım**: 

`upgrade_spark_enhancement.sh https://${jar_path}`

**Örnek**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.tgz` 

**Azure portalından bash dosyasını kullanmak için**

1. Başlatma [Azure portalı](https://ms.portal.azure.com), kümenizi seçin.
2. Tıklayın **betik eylemlerini**, ardından **gönderme yeni**. Tamamlamak **betik eylemini Gönder** form'a tıklayın **Oluştur** düğmesi.
    
    + **Betik türü**: seçin **özel**.
    + **Ad**: bir betik adı belirtin.
    + **Bash betiği URI'si**: Özel kümeye bash dosyayı karşıya yüklemeyi daha sonra URL'sini buraya kopyalayın. Alternatif olarak, sağlanan URI'ı kullanın.
    
   ```upgrade_spark_enhancement
    https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh
   ```

    + Kontrol **baş** ve **çalışan**.
    + **Parametreleri**: bash kullanım parametreleri izleme ayarlayın.

    ![Günlük karşıya yükleme veya yükseltme düzeltme](./media/apache-azure-spark-history-server/sparkui-upload2.png)


## <a name="known-issue"></a>Bilinen sorun

1.  Şu anda yalnızca Spark 2.3 küme için çalışır.

2.  Girdi/çıktı verilerini RDD kullanarak, veri sekmesinde gösterilmez.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Spark ayarlarını yapılandırma](apache-spark-settings.md)


## <a name="contact-us"></a>Bizimle iletişim kurun

Bir Geri bildiriminiz varsa veya bu aracı kullanırken diğer herhangi bir sorunla karşılaşırsanız, bir e-postası gönder ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)).