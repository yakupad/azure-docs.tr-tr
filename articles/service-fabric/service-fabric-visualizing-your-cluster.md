---
title: Azure Service Fabric Explorer kullanarak kümenizi Görselleştirme | Microsoft Docs
description: Service Fabric Explorer, inceleme ve bulut uygulamaları ve Microsoft Azure Service Fabric kümesindeki düğümlere yönetmek için kullanılan bir uygulamadır.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2018
ms.author: mikhegn
ms.openlocfilehash: 459dd86fd614cb185801b074cea70c36dc7f6ccb
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972341"
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Service Fabric Explorer ile kümenizi görselleştirme

Service Fabric Explorer (SFX), inceleme ve Azure Service Fabric kümelerini yönetmeye yönelik bir açık kaynak aracıdır. Service Fabric Explorer, Windows, macOS ve Linux için bir masaüstü uygulamasıdır.

## <a name="service-fabric-explorer-download"></a>Service Fabric Explorer indirme

Bir masaüstü uygulaması olarak Service Fabric Explorer'ı indirmek için aşağıdaki bağlantıları kullanın:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

- macOS
  - https://aka.ms/sfx-macos

> [!NOTE]
> Service Fabric Explorer'ın Masaüstü sürümünü küme desteği daha az veya daha fazla özellik olabilir. Tam özellik uyumluluğu sağlamak için kümeye dağıtılmış Service Fabric Explorer sürümüne geri dönebilirsiniz.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Kümedeki çalışan Service Fabric Explorer

Service Fabric Explorer, bir Service Fabric küme HTTP yönetim uç noktası da barındırılır. Bir web tarayıcısında SFX başlatmak için kümenin HTTP yönetim uç noktası için herhangi bir tarayıcıdan - örneğin göz https://clusterFQDN:19080.

Geliştirici iş istasyonu kurulumu için Service Fabric Explorer kullanarak yerel kümenizde giderek başlatabilirsiniz https://localhost:19080/Explorer. Bu makaleye bakın [geliştirme ortamınızı hazırlama](service-fabric-get-started.md).

## <a name="connect-to-a-service-fabric-cluster"></a>Bir Service Fabric kümesine bağlanın
Bir Service Fabric kümesine bağlanmak için küme yönetim uç noktası'nı (FQDN/IP) ve HTTP yönetim uç noktası 19080 bağlantı noktasına (varsayılan) gerekir. Örneğin, https://mysfcluster.westus.cloudapp.azure.com:19080. İş istasyonunuzda yerel kümeye bağlanmak için "Localhost bağlanma" onay kutusunu kullanın.

### <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma
Service Fabric kümenize sertifikalar veya Azure Active Directory (AAD) kullanarak istemci erişimi denetleyebilirsiniz.

Güvenli bir kümeye bağlanmayı denerseniz, ardından küme yapılandırmasına bağlı olarak, bir istemci sertifikası sunmak veya AAD kullanarak oturum gerekecektir.

## <a name="video-tutorial"></a>Video öğretici

Service Fabric Explorer'ı kullanmayı öğrenmek için aşağıdaki Microsoft Virtual Academy video izleyin:

> [!NOTE]
> Bu video, Service Fabric Explorer barındırılan Service Fabric kümesinde değil Masaüstü sürümünü gösterir.
>
>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="understand-the-service-fabric-explorer-layout"></a>Service Fabric Explorer düzenini anlama
Soldaki ağaç kullanarak Service Fabric Explorer gidebilirsiniz. Ağaç kökünde, küme Panosu, kümenize uygulama ve düğüm durumunun özetini de dahil olmak üzere, genel bir bakış sağlar.

![Service Fabric Explorer küme Panosu][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Kümenin Düzen görüntüleyin
Service Fabric kümesi, hata etki alanı arasında iki boyutlu bir kılavuz yerleştirilir ve yükseltme etki alanlarında. Bu yerleştirme uygulamalarınızı donanım arızalarında ve uygulama yükseltmeleri saklanacaktır kullanılabilir kalmasını sağlar. Nasıl geçerli Küme Küme eşlemesi kullanarak düzenlendiğini görüntüleyebilirsiniz.

![Service Fabric Explorer küme eşleme][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Görünüm uygulamaları ve Hizmetleri
İki alt kümeyi içerir: biri uygulamalar, diğer düğümler için.

Service Fabric'in mantıksal hiyerarşisi aracılığıyla gitmek için uygulama görünümü kullanabilirsiniz: uygulamaları, hizmetleri, bölümler ve çoğaltmalar.

Uygulama aşağıdaki örnekte **MyApp** iki hizmetten oluşur **MyStatefulService** ve **WebService**. Bu yana **MyStatefulService** durum bilgisi olan bir birincil ve iki ikincil çoğaltma ile bir bölüm içerir. Bunun aksine, WebSvcService durum bilgisiz olduğundan ve tek bir örnek içerir.

![Service Fabric Explorer uygulama görünümü][sfx-application-tree]

Ana bölmede, her ağaç düzeyinde öğe hakkındaki bilgileri gösterir. Örneğin, belirli bir hizmet için sürümü ve sistem durumunu görebilirsiniz.

![Service Fabric Explorer temel bileşenler bölmesi][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Küme düğümleri görüntüleyin
Düğüm görünümü, kümenin fiziksel düzenini gösterir. Belirli bir düğümde, hangi uygulamalara kod dağıtıldığını denetleyebilirsiniz. Daha açık belirtmek gerekirse çoğaltmalar var. çalışmakta olduğunu görebilirsiniz.

## <a name="actions"></a>Eylemler
Service Fabric Explorer, düğümler, uygulamaları ve Hizmetleri kümenizdeki eylemleri çağırmak için hızlı bir yol sunar.

Örneğin, bir uygulama örneğini silmek için soldaki ağaç oluşturulacak uygulamayı seçin ve ardından **eylemleri** > **uygulama Sil**.

![Service Fabric Explorer'da uygulamanın siliniyor][sfx-delete-application]

> [!TIP]
> Her öğenin yanındaki üç noktaya tıklayarak aynı eylemleri gerçekleştirebilirsiniz.
>
> Service Fabric Explorer gerçekleştirilen her eylemi, Otomasyon etkinleştirmek için PowerShell veya bir REST API'si gerçekleştirilebilir.
>
>

Service Fabric Explorer, belirli uygulama türü ve sürümü için uygulama örnekleri oluşturmak için de kullanabilirsiniz. Ağaç görünümünde uygulama türünü seçin ve ardından tıklayın **uygulama örneği oluştur** sağ bölmede istediğiniz sürüm yanındaki bağlantı.

![Service Fabric Explorer'da uygulama örneğini oluşturma][sfx-create-app-instance]

> [!NOTE]
> Service Fabric Explorer, uygulama örnekleri oluştururken, parametreleri desteklemiyor. Uygulama örnekleri, varsayılan parametre değerlerini kullanın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Visual Studio'da Service Fabric uygulamalarınızı yönetme](service-fabric-manage-application-in-visual-studio.md)
* [PowerShell kullanarak Service Fabric uygulama dağıtımı](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
