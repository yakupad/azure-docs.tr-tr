---
title: Azure'da bulut tabanlı IoT tahmine dayalı bakım çözümü dağıtma | Microsoft Docs
description: Bu hızlı başlangıçta Tahmine Dayalı Bakım Azure IoT çözümünü dağıtacak ve çözüm panosunda oturum açıp işlevleri kullanacaksınız.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 65c10f393efbeaa111e2b413a0568da053c04567
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001136"
---
# <a name="quickstart-try-a-cloud-based-solution-to-run-a-predictive-maintenance-analysis-on-my-connected-devices"></a>Hızlı başlangıç: Bağlı cihazlarımda tahmine dayalı bakım analizi çalıştırmak için bulut tabanlı bir çözüm deneme

Bu hızlı başlangıçta Azure IoT Tahmine Dayalı Bakım çözümü hızlandırıcısını dağıtarak bulut tabanlı tahmine dayalı bakım simülasyonu çalıştırmayı öğreneceksiniz. Çözüm hızlandırıcısını dağıttıktan sonra, çözümün **Pano** sayfasını kullanarak simülasyon uçak motoru verilerinden yararlanan tahmine dayalı bakım analizi çalıştırırsınız. Bu çözüm hızlandırıcısını, kendi uygulamanızın başlangıç noktası veya bir öğrenme aracı olarak kullanabilirsiniz.

Simülasyonda fabrikam, rekabetçi fiyatlarıyla büyük müşteri deneyimine odaklanan bölgesel bir havayoludur. Uçuş rötarlarının bir nedeni bakım sorunlarıdır ve uçak motorunun bakımı özellikle zordur. Fabrikam’ın ne olursa olsun uçuş sırasında motor arızasını önlemesi gerekmektedir; bu nedenle düzenli olarak motorları muayene eder ve bakım işlemlerini bir plana göre zamanlar. Ancak, uçak motorları her zaman aynı şekilde yıpranmaz. Motorlarda bazı gereksiz bakımlar gerçekleştirilir. Daha da önemlisi, bakım yapılana kadar çıkan sorunlar nedeniyle uçağın yerde kalmasıdır. Uçak, doğru teknisyenlerin ve yedek parçaların olmadığı bir yerdeyse bu sorunlar yüksek maliyetli gecikmelere neden olabilir.

Fabrikam uçağının motorları, uçuş sırasında motor koşullarını izleyen algılayıcılarla donatılmıştır. Motor çalışması ve arızaları verilerini yıllarca biriktirdikten sonra, Fabrikam’ın veri bilim insanları uçak motorunun Kalan Kullanım Ömrü’nü (RUL) tahmin etmek için bir model geliştirmiştir. Bu model, dört motor algılayıcısından alınan veriler ve arızalara neden olan motor yıpranmaları arasındaki bağıntıyı kullanmaktadır. Fabrikam güvenliği sağlamak için normal muayeneleri yapmaya devam ederken, her uçuştan sonra her motor için RUL hesaplayacak modelleri kullanmaktadır. Fabrikam, bir yandan yolcuların ve personelin güvenliğini sağlayarak bir yandan da uçağın yerde kalma süresini en aza indirmesini ve çalıştırma maliyetini düşürmesini sağlamak için gelecekteki arıza noktalarını öngörüp bakım planlayabilir.

Bu hızlı başlangıcı tamamlamak etkin bir Azure aboneliğinizin olması gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Çözüm hızlandırıcısını Azure aboneliğinize dağıttığınızda ayarlamanız gereken yapılandırma seçenekleri vardır.

Azure hesabınızın kimlik bilgilerini kullanarak [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) adresinden oturum açın.

**Tahmine Dayalı Bakım** kutucuğundaki **Şimdi Deneyin** öğesine tıklayın.

![Tahmine Dayalı Bakım seçme](./media/quickstart-predictive-maintenance-deploy/predictivemaintenance.png)

**Tahmine Dayalı Bakım çözümü oluştur** sayfasında Tahmine Dayalı Bakım çözümü hızlandırıcınıza benzersiz bir **Çözüm adı** girin. Bu hızlı başlangıçta **MyPredictiveMaintenance** kullanıyoruz.

Çözüm hızlandırıcısını dağıtırken kullanmak istediğiniz **Subscription** (Abonelik) ve **Region** (Bölge) seçimini yapın. Genelde size en yakın bölgeyi seçmeniz gerekir. Bu hızlı başlangıçta **Visual Studio Enterprise** ve **Doğu ABD** kullanıyoruz. Abonelikte [genel yönetici veya kullanıcı](iot-accelerators-permissions.md) olmanız gerekir.

Dağıtımı başlatmak için **Create Solution** (Çözüm Oluştur) öğesine tıklayın. Bu işlemin çalışması en az beş dakika sürer:

![Tahmine Dayalı Bakım çözümü ayrıntıları](./media/quickstart-predictive-maintenance-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Çözümde oturum açma

Azure aboneliğinize dağıtım tamamlandığında, çözüm dosyasında yeşil bir onay işareti ve **Hazır** yazısı görürsünüz. Tahmine Dayalı Bakım çözüm hızlandırıcısı panonuzda artık oturum açabilirsiniz.

**Sağlanan Çözümler** sayfasında yeni Tahmine Dayalı Bakım çözümü hızlandırıcınıza tıklayın. Açılan panelde çözüm hızlandırıcınızla ilgili bilgileri görüntüleyebilirsiniz. Tahmine Dayalı Bakım çözümü hızlandırıcısını görüntülemek için **Çözüm panosu**'nu seçin:

![Çözüm paneli](./media/quickstart-predictive-maintenance-deploy/solutionpanel.png)

İzin isteğini kabul etmek için **Kabul Et**'e tıklayın. Tahmine Dayalı Bakım çözümü panosu tarayıcınızda görüntülenir:

![Çözüm panosu](./media/quickstart-predictive-maintenance-deploy/solutiondashboard.png)

Benzetimi başlatmak için **Benzetimi başlat**’a tıklayın. Algılayıcı geçmişi, RUL, Döngüler ve RUL geçmişi panoda yer alır:

![Benzetim çalışıyor](./media/quickstart-predictive-maintenance-deploy/simulationrunning.png)

RUL değeri 160’tan (gösterim amaçlı seçilen rastgele bir eşik) azsa, çözüm portalında RUL görüntüsünün yanında bir uyarı simgesi görüntülenir. Çözüm portalı da uçak motorunu sarı renkle vurgular. RUL değerlerinde topluca genel bir düşüş eğilimi olsa da aşağı ve yukarı sıçramalar da olduğunu fark edebilirsiniz. Bu davranış, değişen döngü uzunlukları ve model doğruluğundan sonuçlanır.

![Benzetim uyarısı](./media/quickstart-predictive-maintenance-deploy/simulationwarning.png)

Tam benzetim, 148 döngüyü tamamlamak için yaklaşık 35 dakika sürer. 160 RUL eşiği ilk seferinde yaklaşık 5 dakikayı karşılar, her iki motor da yaklaşık 8 dakikada eşiği yakalar.

Benzetim 148 döngü için tam veri kümesinde çalışır, son RUL ve döngü değerlerinde de kapanır.

Benzetimi istediğiniz an durdurabilirsiniz; ancak, **Benzetimi Başlat**’a tıkladığınızda benzetim veri kümesinin başından başlayarak yeniden oynatılır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla incelemeyi planlıyorsanız, Tahmine Dayalı Bakım çözümü hızlandırıcısını dağıtımda bırakın.

Çözüm hızlandırıcısına ihtiyacınız kalmadıysa [Sağlanan çözümler](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasında hızlandırıcıyı seçip **Çözümü Sil**’e tıklayarak bunu silebilirsiniz:

![Çözümü sil](media/quickstart-predictive-maintenance-deploy/deletesolution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Tahmine Dayalı Bakım çözümü hızlandırıcısını dağıttınız ve bir benzetim çalıştırdınız.

Çözüm hızlandırıcısı ve simülasyon uçak motorları hakkında daha fazla bilgi için aşağıdaki makaleyle devam edin.

> [!div class="nextstepaction"]
> [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](iot-accelerators-predictive-walkthrough.md)
