---
title: Apache Flink Azure Event Hubs ile Apache Kafka için kullanın. | Microsoft Docs
description: Olay hub'ı bir kafka'ya bağlanma Apache Flink etkin
services: event-hubs
documentationcenter: ''
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: mvc
ms.date: 06/06/2018
ms.author: bahariri
ms.openlocfilehash: ce1665c3cfd58d0d5aa8e253b5db317505b1959e
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284586"
---
# <a name="use-apache-flink-with-azure-event-hubs-for-apache-kafka"></a>Apache Flink Azure Event Hubs ile Apache Kafka için kullanın.

Apache Kafka kullanmanın önemli avantajları için bağlanabilir çerçeveleri ekosistemi biridir. Kafka ölçeklenebilirliği, tutarlılık ve Azure ekosistemi desteği ile Kafka'nın esneklik olay hub'ları birleştiren etkin.

Bu öğreticide Protokolü istemcilerinize değiştirme veya kendi kümeleri çalıştıran Apache Flink Kafka özellikli event hubs'a bağlanma gösterilmektedir. Azure Event hubs'ı destekleyen [Apache Kafka sürüm 1.0.](https://kafka.apache.org/10/documentation.html)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* [İndirme](http://maven.apache.org/download.cgi) ve [yükleme](http://maven.apache.org/install.html) Maven ikili Arşivi
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Tüm Event Hubs hizmetinden gönderileceği ve alınacağı bir Event Hubs ad alanı gereklidir. Bkz: [Event Hubs etkin Kafka oluşturma](event-hubs-create-kafka-enabled.md) bir olay hub'ları Kafka uç nokta alma hakkında bilgi için. Daha sonra kullanmak için Event Hubs bağlantı dizesi kopyaladığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projesini kopyalama

Event Hubs, Kafka özellikli bir bağlantı dizesi olduğuna göre Azure Event Hubs depoyu kopyalamak ve gidin `flink` alt klasörü:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/flink
```

## <a name="flink-producer"></a>Flink üretici

Sağlanan Flink üretici örneği kullanarak Event Hubs hizmeti için iletiler gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç noktası sağlayın

#### <a name="producerconfig"></a>Producer.config

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `producer/src/main/resources/producer.config` üretici doğru kimlik doğrulaması ile Event Hubs Kafka uç noktasına yönlendirmek için.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Üretici komut satırından çalıştırma

Üretici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya daha sonra sınıf için gerekli Kafka JAR(s) ekleyerek Java'da çalıştırma Maven kullanarak bir JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

Üretici artık başlayacak olayları göndermek için Kafka, olay hub'ı konusuna etkin `test` ve stdout olaylara yazdırma.

## <a name="flink-consumer"></a>Flink tüketici

Kuyruktan alma sağlanan tüketici örneği kullanarak Event Hubs Kafka etkin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Bir olay hub'ları Kafka uç noktası sağlayın

#### <a name="consumerconfig"></a>Consumer.config

Güncelleştirme `bootstrap.servers` ve `sasl.jaas.config` değerler `consumer/src/main/resources/consumer.config` tüketici doğru kimlik doğrulaması ile Event Hubs Kafka uç noktasına yönlendirmek için.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Tüketici komut satırından çalıştırma

Tüketici komut satırından çalıştırmak için JAR oluşturmak ve ardından Maven içinde çalıştırın (veya daha sonra sınıf için gerekli Kafka JAR(s) ekleyerek Java'da çalıştırma Maven kullanarak bir JAR oluşturmak):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Kafka özellikli bir olay hub'ı olayları (örneğin, üretici da çalışıyorsa) varsa, ardından tüketici artık konu başlığından olayları almaya başlar `test`.

Kullanıma [Flink'ın Kafka bağlayıcı Kılavuzu](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) Flink Kafka'ya bağlanma hakkında daha ayrıntılı bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
* [Kafka için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
* [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) kullanarak [olayları Kafka şirket içinden bulutta Kafka etkin Event Hubs’a akışla aktarın.](event-hubs-kafka-mirror-maker-tutorial.md)
* Kafka akışı yapmayı öğrenin etkin Event Hubs kullanarak [yerel Kafka uygulamalar](event-hubs-quickstart-kafka-enabled-event-hubs.md) veya [Akka akışları](event-hubs-kafka-akka-streams-tutorial.md).
