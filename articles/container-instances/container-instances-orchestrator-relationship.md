---
title: Azure kapsayıcı örnekleri ve kapsayıcı düzenleme
description: Örnekleri etkileşim nasıl Azure kapsayıcısı ile kapsayıcı orchestrators anlayın.
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/23/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: e1455cba004facfa03dca21544eec754f5dc60be
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32165570"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure kapsayıcı örnekleri ve kapsayıcı orchestrators

Kendi küçük boyutu ve uygulama yönlendirme nedeniyle kapsayıcıları Çevik teslim ortamları ve mikro hizmet tabanlı mimariler için uygundur. Otomatikleştirme ve çok sayıda kapsayıcıları ve nasıl etkileşim kurduklarını yönetme görevini olarak bilinen *orchestration*. Popüler kapsayıcı orchestrators Kubernetes, DC/OS ve Docker Swarm içerir.

Azure kapsayıcı örnekleri bazı orchestration platformları temel zamanlama özelliklerini sağlar. Ve bu platformları sağlayan bir yüksek değerli hizmetler kapsamaz olsa da, Azure kapsayıcı örnekleri için tamamlayıcı olabilir. Bu makalede, Azure kapsayıcı örnekleri ne işler ve kapsayıcı orchestrators ile nasıl tam etkileşebilir kapsamını açıklanmaktadır.

## <a name="traditional-orchestration"></a>Geleneksel düzenleme

Orchestration standart tanımını aşağıdaki görevleri içerir:

- **Zamanlama**: verilen bir kapsayıcı görüntüsü ve bir kaynak isteği, kapsayıcı çalıştırmak için uygun bir makine bulunamadı.
- **Benzeşim/Anti-affinity**: kapsayıcıları bir dizi diğer (performans için) yakındaki veya yeterince kadar parçalayın (kullanılabilirlik için) çalışması gerektiğini belirtmek.
- **Sistem durumu izleme**: kapsayıcı hatalarını ve otomatik olarak izlemek yeniden zamanlayabilirsiniz.
- **Yük devretme**: her makinede çalışan izlemek ve başarısız makineler kapsayıcılardan sağlıklı düğümlere yeniden zamanlayın.
- **Ölçeklendirme**: isteğe bağlı, el ile veya otomatik olarak eşleştirmek için kapsayıcı örnekler ekleyip kaldıracaktır.
- **Ağ**: birden çok ana makine arasında iletişim kurmak için kapsayıcıları düzenlemekten bir katman ağ sağlar.
- **Hizmet bulma**: ana makineler arasında taşımak ve IP adreslerini değiştirmek gibi bile birbirlerine otomatik olarak bulmak kapsayıcıları etkinleştirin.
- **Uygulama yükseltme Eşgüdümlü**: uygulama kapalı kalma süresi önlemek için kapsayıcı yükseltmeler yönetebilir ve bir sorun yaşanırsa geri alma etkinleştirebilir.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Azure kapsayıcı örnekleri düzenlemesini: bir katmanlı yaklaşımın

Azure kapsayıcı örnekleri, tüm orchestrator platformlar, üzerinde birden çok kapsayıcı görevleri yönetmek üzere izin verirken tek bir kapsayıcı çalıştırmak için gerekli planlama ve yönetim özellikleri sağlayarak orchestration, katmanlı bir yaklaşım sağlar.

Altyapının kapsayıcı örnekleri için Azure tarafından yönetildiğinden, bu orchestrator platform kendisini bir uygun konak makinesi üzerinde tek bir kapsayıcı çalıştırmak için bulma ile ilgili gerekmez. Bir her zaman kullanılabilir bulut esneklik sağlar. Bunun yerine, orchestrator ölçeklendirme dahil olmak üzere birden çok kapsayıcı mimarileri ve Eşgüdümlü yükseltmeleri geliştirilmesini basitleştirmek görevlerde odaklanabilirsiniz.

## <a name="potential-scenarios"></a>Olası senaryolar

Azure kapsayıcı örnekleri ile orchestrator tümleştirme hala nascent olsa da, birkaç farklı ortamlarda ortaya çıkan düşündüğünüz:

### <a name="orchestration-of-container-instances-exclusively"></a>Orchestration kapsayıcısının özel olarak örnekleri

Hızlı Başlat ve ikinciye faturalandırmak çünkü özel olarak Azure kapsayıcı örneklerinde bağlı bir ortam başlamak ve yüksek oranda değişken iş yükleri ile mücadele etmek için en hızlı yolu sunar.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Kapsayıcı örnekleri ve sanal makineleri kapsayıcılarında birleşimi

Uzun süre çalışan, kararlı iş yükleri için genellikle Azure kapsayıcı örnekleriyle aynı kapsayıcıları çalıştıran daha ucuz ayrılmış sanal makine bir kümede kapsayıcıları yönetme. Ancak, kapsayıcı örnekleri hızlı bir şekilde genişletme ve beklenmeyen veya kısa süreli ani kullanımı uğraşmanız genel kapasitenizi daraltılırken için harika bir çözüm sunar.

Sanal makine kümenizdeki sayısı ölçeğini, yerine daha sonra bu makinelere ek kapsayıcıları dağıtma, orchestrator yalnızca Azure kapsayıcı örnekleri ek kapsayıcılarında zamanlayabilir ve bunlar artık olduğunuzda silin gerekli.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Örnek uygulama: Kubernetes için Azure kapsayıcı örnekleri Bağlayıcısı

Kapsayıcı orchestration platformları Azure kapsayıcı örnekleri ile nasıl tümleşebilir göstermek için biz yapı başlattığınız bir [Kubernetes için örnek Bağlayıcısı][aci-connector-k8s].

Bağlayıcı Kubernetes için taklit eder [kubelet] [ kubelet-doc] sınırsız kapasiteye sahip bir düğüm olarak kaydetme ve oluşturulmasını göndermeyi [pod'ları] [ pod-doc] Azure kapsayıcı durumlarda kapsayıcı grupları olarak.

Diğer orchestrators bağlayıcılarının API orchestrator gücünü hızı ve Azure kapsayıcı örnekleri kapsayıcılarında yönetme Basitlik birleştirip platform temelleri benzer şekilde entegre oluşturulabilir.

> [!WARNING]
> Kubernetes ACI Bağlayıcısı *Deneysel* ve üretimde kullanılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı örneği kullanarak, ilk kapsayıcı oluşturmak [Hızlı Başlangıç Kılavuzu](container-instances-quickstart.md).

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/