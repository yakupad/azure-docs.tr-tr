---
title: Azure DevOps Projesi ile Ruby on Rails için CI/CD işlem hattı oluşturma | Hızlı Başlangıç
description: DevOps Projesi, Azure’u kullanmaya başlamayı kolaylaştırır. Birkaç hızlı adımda, Azure hizmetinde Ruby web uygulamasını başlatmanıza yardımcı olur.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 773e77e0965dfb059460faf564d8cc3d80cef3ea
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967115"
---
# <a name="create-a-cicd-pipeline-for-ruby-on-rails-with-the-azure-devops-project"></a>Azure DevOps Projesi ile Ruby on Rails için CI/CD işlem hattı oluşturma

**Azure DevOps Projesi**'yle Ruby on Rails uygulamanız için sürekli tümleştirmeyi (CI) ve sürekli teslimi (CD) yapılandırın.  Azure DevOps projesi, VSTS derleme ve sürüm işlem hattının ilk yapılandırmasını basitleştirir.

Azure aboneliğiniz yoksa, [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz edinebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projesi VSTS'de bir CI/CD işlem hattı oluşturur.  **Yeni VSTS** hesabı oluşturabilir veya **mevcut bir hesabı** kullanabilirsiniz.  Azure DevOps Projesi ayrıca tercih ettiğiniz **Azure aboneliğinde** **Azure kaynakları** oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol gezinti çubuğunda **Kaynak oluştur** simgesini seçin ve ardından **DevOps projesini** arayın.  **Oluştur**’u seçin.

    ![Sürekli Teslim Başlatılıyor](_img/azure-devops-project-ruby/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. **Ruby** örnek uygulamasını seçin.

1. **Ruby on Rails** uygulama çerçevesini seçin.  İşiniz bittiğinde **İleri**’yi seçin.

1. **Linux üzerinde Web App** varsayılan dağıtım hedefidir.  İsteğe bağlı olarak, Kapsayıcılar için Web App’i seçebilirsiniz.  Önceki adımlarda seçtiğiniz uygulama çerçevesi, burada bulabileceğiniz Azure hizmeti dağıtımı hedefi türünü belirler.  Dilediğiniz **hedef hizmeti** seçin.  İşiniz bittiğinde **İleri**’yi seçin.

## <a name="configure-vsts-and-an-azure-subscription"></a>VSTS'yi ve Azure aboneliğini yapılandırma 

1. **Yeni** bir VSTS hesabı oluşturun veya **mevcut** bir hesabı seçin.  VSTS projeniz için bir **ad** seçin.  **Azure aboneliğinizi**, **konumunuzu** ve uygulamanız için bir **ad** seçin.  İşiniz bittiğinde **Bitti**’yi seçin.

    ![VSTS bilgilerini girme](_img/azure-devops-project-ruby/vstsazureinfo.png)

1. Birkaç dakika içinde **proje panosu** Azure portalında yüklenir.  VSTS hesabınızdaki bir depoda örnek uygulama ayarlanır, bir derleme yürütülür ve uygulamanız Azure’a dağıtılır.  Bu pano **kod deponuza**, **VSTS CI/CD işlem hattına** ve **Azure’daki uygulamanıza** görünürlük sağlar.  Panonun sağ tarafında çalışan uygulamanızı görüntülemek için **Gözat**’ı seçin.

    ![Pano görünümü](_img/azure-devops-project-ruby/dashboardnopreview.png) 

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

Azure DevOps projesi, VSTS veya GitHub hesabınızda bir Git deposu oluşturdu.  Depoyu görüntülemek ve uygulamanızda kod değişiklikleri yapmak için aşağıdaki adımları izleyin.

1. DevOps projesi panosunun sol tarafında **ana** dalınızın bağlantısını seçin.  Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo kopya URL'sini görüntülemek için tarayıcının sağ üst kısmından **Kopya**’yı seçin. Git deponuzu en sevdiğiniz IDE’de kopyalayabilirsiniz.  Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Tarayıcının sol tarafında **app/views/pages/home.html.erb** dosyasına gidin.

1. **Düzenle**’yi seçin ve bir metinde değişiklik yapın.  Örneğin, div etiketlerinden birinin içindeki metni değiştirin.

1. **İşle**’yi seçin ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda **Azure DevOps projesi panosuna** gidin.  Bir derlemenin sürdüğünü görüyor olmanız gerekir.  Yaptığınız değişiklikler otomatik olarak derlenir ve bir VSTS CI/CD işlem hattı üzerinden dağıtılır.

## <a name="examine-the-vsts-cicd-pipeline"></a>VSTS CI/CD işlem hattını inceleme

Azure DevOps projesi, VSTS hesabınızda otomatik olarak tam bir VSTS CI/CD işlem hattı yapılandırdı.  İşlem hattını gerektiği şekilde keşfedin ve özelleştirin.  VSTS derlemesi ve yayın tanımları ile kendinizi alıştırmak için aşağıdaki adımları izleyin.

1. Azure DevOps projesi panosunun **üst** kısmından **Derleme İşlem Hatları**’nı seçin.  Bu bağlantı, bir tarayıcı sekmesi açar ve yeni projeniz için VSTS derleme tanımını açar.

1. **Üç noktayı** seçin.  Bu eylem, yeni bir derlemeyi sıraya alma, derleme duraklatma ve derleme tanımını düzenleme gibi birkaç etkinliği başlatabileceğiniz bir menüyü açar.

1. **Düzenle**’yi seçin.

1. Bu görünümden derleme tanımınızın **çeşitli görevlerini inceleyin**.  Derleme, Git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtım için kullanılan çıkışları yayımlama gibi çeşitli görevleri yürütür.

1. Derleme tanımının üst kısmında **derleme tanımı adını** seçin.

1. Derleme tanımınızın **adını** daha açıklayıcı bir şeyle değiştirin.  **Kaydet ve sıraya al**’ı ve ardından **Kaydet**’i seçin.

1. Derleme tanımı adınızın altında **Geçmiş**’i seçin.  Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz.  VSTS, derleme tanımında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  Azure DevOps projesi otomatik olarak bir CI tetikleyicisi oluşturdu. Depoya yönelik her işleme yeni bir derleme başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  Senaryonuza bağlı olarak, belirli sayıdaki derlemeleri saklayacak veya kaldıracak ilkeleri belirtebilirsiniz.

1. **Derleme ve Yayın**’ı ve ardından **Sürümler**’i seçin.  Azure DevOps projesi Azure'a yönelik dağıtımları yönetmek için bir VSTS sürüm tanımı oluşturdu.

1. Tarayıcının sol tarafında, sürüm tanımınızın yanındaki **üç noktayı** ve ardından **Düzenle**’yi seçin.

1. Sürüm tanımı, sürüm işlemini tanımlayan bir **işlem hattı** içerir.  **Yapıtlar**’ın altında **Bırak**’ı seçin.  Önceki adımlarda incelediğiniz derleme tanımı, yapıt için kullanılan çıkışı üretir. 

1. **Bırak** simgesinin sağ tarafında **Sürekli dağıtım tetikleyicisi**’ni seçin.  Bu sürüm tanımı, yeni bir derleme yapıtı kullanılabilir olduğunda bir dağıtım yürüten CD tetikleyicisini etkinleştirdi.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütme gerektirmesi için tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Tarayıcının sol tarafında **Görevler**’i seçin.  Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir.  Bu örnekte, **Azure App Service**’e dağıtmak üzere bir görev oluşturuldu.

1. Tarayıcının sağ tarafında **Sürümleri görüntüle**’yi seçin.  Bu görünümde sürüm geçmişi gösterilir.

1. Sürümlerinizden birinin yanındaki **üç nokta** simgesini seçin ve sonra da **Aç**'ı seçin.  Bu görünümde keşfedilebilecek sürüm özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  Bu görünümde, belirli bir dağıtımla ilişkilendirilmiş kod işlemeleri gösterilir. 

1. **Günlükler**’i seçin.  Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur.  Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, Azure DevOps projesi panosundaki **Sil** işlevini kullanarak bu hızlı başlangıçta oluşturulmuş olan Azure App Service’i ve ilgili kaynaklarını silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Derleme ve sürüm tanımlarını ekibinizin ihtiyaçlarını karşılayacak şekilde değiştirmeyle ilgili daha fazla bilgi edinmek için bu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/vsts/pipelines/release/define-multistage-release-process?view=vsts)
