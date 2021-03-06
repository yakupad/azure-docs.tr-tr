---
title: Kapsayıcı iş yüklerinin Azure batch | Microsoft Docs
description: Uygulamaları, kapsayıcı görüntülerini Azure Batch'te çalıştırmayı öğrenin.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 06/04/2018
ms.author: danlep
ms.openlocfilehash: a85db0315a2ee8aa9fd34b8c18893f4cb1068528
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090971"
---
# <a name="run-container-applications-on-azure-batch"></a>Azure Batch'te kapsayıcı uygulamaları çalıştırma

Azure Batch, çalıştırmak ve toplu bilgi işlem işlerini azure'da çok sayıda ölçeklendirmek olanak tanır. Batch görevleri Batch havuzundaki sanal makinelerin (düğümler) üzerinde doğrudan çalıştırabilirsiniz, ancak Docker uyumlu kapsayıcılar görevleri düğümler üzerinde çalıştırılacak bir Batch havuz ölçeğini de ayarlayabilirsiniz. Bu makalede kapsayıcı çalışmakta olan görevlerin destekleyen ve ardından kapsayıcı görevleri havuzda çalışan işlem düğümleri havuzu oluşturma işlemini gösterir. 

Kapsayıcı kavramları ve bir Batch havuzu ve işini oluşturma konusunda bilgi sahibi olması gerekir. Kod örnekleri, Batch .NET ve Python SDK'ları kullanın. Ayrıca diğer Batch SDK'ları ve araçları, Azure portalında da dahil olmak üzere kapsayıcı özellikli Batch havuzları oluşturma ve kapsayıcı görevleri çalıştırmak için kullanabilirsiniz.

## <a name="why-use-containers"></a>Kapsayıcıları neden kullanmalısınız?

Kapsayıcıları kullanarak bir ortam ve uygulamaları çalıştırmak için bağımlılıkları yönetmek zorunda kalmadan, Batch görevleri çalıştırmak için kolay bir yol sağlar. Kapsayıcı uygulamaları birkaç farklı ortamlarda çalışabilir basit, taşınabilir, kendi kendine yeterli birimleri olarak dağıtın. Örneğin, yapı ve kapsayıcı yerel olarak test sonra Azure'da veya başka bir kayıt defterindeki kapsayıcı görüntüsünün yükleyin. Kapsayıcı dağıtım modelini, uygulamanızın çalışma zamanı ortamı her zaman doğru bir şekilde yüklendiğini ve uygulamayı barındıran her yerde yapılandırıldığını sağlar. Batch'teki görevleri kapsayıcı tabanlı özellikleri, uygulama paketleri ve kaynak dosyaları ve çıkış dosyalarının yönetimi dahil olmak üzere kapsayıcı olmayan görevleri de yararlanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

* **SDK sürümleri**: Batch SDK'ları destek kapsayıcı görüntülerini aşağıdaki sürümlerinden itibaren:
    * Batch REST API Sürüm 2017-09-01.6.0
    * Batch .NET SDK'sı sürüm 8.0.0
    * Batch Python SDK'sı sürüm 4.0
    * Batch Java SDK'sı sürüm 3.0
    * Batch Node.js SDK'sı sürüm 3.0

* **Hesapları**: Azure aboneliğinizde bir Batch hesabı ve isteğe bağlı olarak bir Azure depolama hesabı oluşturmanız gerekir.

* **Desteklenen bir VM görüntüsü**: sanal makine yapılandırmasıyla oluşturulan havuzlarda yalnızca kapsayıcılar desteklenir aşağıdaki bölümde ayrıntılı görüntülerden "sanal makine görüntüleri desteklenmiyor." Özel bir görüntü sağlarsanız, aşağıdaki bölümde önemli noktalar ve gereksinimler bkz [sanal makine havuzu oluşturmak için yönetilen bir özel görüntü kullanmak](batch-custom-images.md). 

### <a name="limitations"></a>Sınırlamalar

* Batch, yalnızca Linux havuzlarında çalışan kapsayıcılar için RDMA desteği sağlar.

* Windows kapsayıcı iş yükleri için havuzunuz için birden fazla çekirdekli bir VM boyutu seçme için önerilir

## <a name="supported-virtual-machine-images"></a>Desteklenen sanal makine görüntüleri

Şunlardan birini kullanın desteklenen Windows veya Linux görüntülerinden sanal makine havuzu oluşturmak için işlem düğümlerine kapsayıcı iş yükleri için. Batch ile uyumlu olan Market görüntüleri hakkında daha fazla bilgi için bkz. [sanal makine görüntülerinin listesi](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Windows görüntüleri

Windows kapsayıcı iş yükleri için Batch şu anda desteklediği **kapsayıcılar ile Windows Server 2016 Datacenter** Azure Marketi'nde görüntü. Docker kapsayıcı görüntüleri Windows üzerinde desteklenir.

Docker Windows üzerinde çalışan sanal makineler özel görüntülerinizi de oluşturabilirsiniz.

### <a name="linux-images"></a>Linux görüntüleri

Linux kapsayıcı iş yükleri için Batch şu anda Azure marketi'ndeki Microsoft Azure Batch tarafından yayımlanan aşağıdaki Linux görüntüleri destekler:

* **Azure Batch kapsayıcı havuzlar için centOS**

* **Azure Batch kapsayıcı havuzlar için centOS (ile RDMA sürücüleri)**

* **Azure Batch kapsayıcı havuzlar için ubuntu Server**

* **Azure Batch kapsayıcı havuzlar için ubuntu Server (ile RDMA sürücüleri)**

Bu görüntüler, yalnızca kullanılacak Azure Batch havuzlarında desteklenir. Bunlar özelliği:

* Önceden yüklenmiş [Moby](https://github.com/moby/moby) kapsayıcı çalışma zamanı 

* Azure N serisi vm'lerde dağıtımı kolaylaştırmak için önceden yüklenmiş NVIDIA GPU sürücüleri

* Görüntüler ile veya olmadan önceden yüklenmiş RDMA sürücüleri; Bu sürücüleri havuz düğümleri RDMA özellikli bir VM boyutlarına dağıtıldığında Azure RDMA ağ erişmesine izin ver  

Batch ile uyumlu Linux dağıtımları birinde Docker'ı çalıştıran VM'ler özel görüntülerinizi de oluşturabilirsiniz. Özel kendi Linux görüntünüzü sağlamayı tercih ederseniz yönergelere bakın [sanal makine havuzu oluşturmak için yönetilen bir özel görüntü kullanmak](batch-custom-images.md).

Özel bir görüntüdeki Docker desteğini yükleme [Docker Community Edition'ı (CE)](https://www.docker.com/community-edition) veya [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Özel bir Linux görüntüsü kullanmak için ek hususlar:

* Özel bir görüntü kullanırken Azure N serisi boyutları GPU performansını yararlanmak için önceden NVIDIA sürücüleri yükleyin. Ayrıca, NVIDIA GPU'ları için Docker altyapısı yardımcı programı'nı yüklemeniz gerekir [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* Azure RDMA ağ erişmek için bir RDMA özellikli bir VM boyutu kullanın. CentOS HPC ve Batch tarafından desteklenen bir Ubuntu görüntülerinde gerekli RDMA sürücüleri yüklenir. MPI iş yüklerini çalıştırmak için ek yapılandırma gerekli olabilir. Bkz: [Batch havuzunda kullanım RDMA özellikli veya GPU özellikli örnekler](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>Batch havuzu için kapsayıcı yapılandırma

Kapsayıcı iş yüklerini çalıştırmak bir Batch havuzu etkinleştirmek için belirtmelisiniz [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) havuzun ayarlarında [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) nesne. (Bu makale, Batch .NET API Başvurusu bağlantılar sağlar. Karşılık gelen ayarları bulunduğunuz [Batch Python](/python/api/azure.batch) API.)

Aşağıdaki örneklerde gösterildiği gibi ile veya olmadan önceden getirilmiş kapsayıcı görüntüleri, kapsayıcı özellikli bir havuz oluşturabilirsiniz. Çekme (veya önceden getirme) işlemi, Docker Hub veya Internet üzerindeki başka bir kapsayıcı kayıt defterinden kapsayıcı görüntüleri önceden yükleme sağlar. En iyi performans için kullanmak bir [Azure kapsayıcı kayıt defteri](../container-registry/container-registry-intro.md) Batch hesabıyla aynı bölgede.

Kapsayıcı görüntülerini önceden getiriliyor avantajı görevleri çalıştıran ilk kez başlattığınızda, bunlar indirmek kapsayıcı görüntüsü için beklenecek gerekmemesidir. Kapsayıcı yapılandırması, havuz oluşturulduğunda Vm'lere kapsayıcı görüntülerini çeker. Havuzda çalışan görevler, kapsayıcı görüntüleri listesi ardından başvurabilir ve kapsayıcı Çalıştırma Seçenekleri.


### <a name="pool-without-prefetched-container-images"></a>Olmadan önceden getirilmiş kapsayıcı görüntülerini havuz

Önceden getirilmiş kapsayıcı görüntüleri olmadan bir kapsayıcı özellikli havuzunu yapılandırmak için tanımladığınız `ContainerConfiguration` ve `VirtualMachineConfiguration` Python aşağıda gösterildiği gibi nesneleri. Bu örnekte, Ubuntu Server için Azure Batch havuzları görüntüyü Market kullanır.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Görüntüler kapsayıcı yapılandırması için hazırlık

Kapsayıcı görüntülerini havuz üzerinde önceden getirme, kapsayıcı görüntüleri listesi ekleyin (`container_image_names`, python'daki) için `ContainerConfiguration`. 

Aşağıdaki temel Python örnek, standart bir Ubuntu kapsayıcı görüntüden önceden getirme işlemi gösterilmektedir [Docker Hub](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


Aşağıdaki örnek C# örneği TensorFlow görüntüden önceden getirme istediğinizi varsayar [Docker Hub](https://hub.docker.com). Bu örnek, havuz düğümlerine sanal makine ana bilgisayarında çalışan bir başlangıç görevi içerir. Bir başlangıç görevi konak, örneğin, kapsayıcılardan erişilebilir bir dosya sunucusuna bağlamak için çalıştırabilirsiniz.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Bir özel kapsayıcı kayıt defterinden görüntüleri önceden getirme

Ayrıca, bir özel kapsayıcı kayıt defteri sunucusunda kimlik doğrulaması yaparak kapsayıcı görüntülerini önceden getirme. Aşağıdaki örnekte, `ContainerConfiguration` ve `VirtualMachineConfiguration` nesneleri önceden getirme özel bir TensorFlow görüntüsü özel Azure kapsayıcısı kayıt defterinden. Görüntü önceki örnektekiyle aynı başvurudur.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## <a name="container-settings-for-the-task"></a>Görev için kapsayıcı ayarları

Kapsayıcı görevleri işlem düğümlerinde çalıştırılacak görev seçenekleri, kullanılacak görüntüleri ve kayıt defteri çalıştırma gibi kapsayıcı özgü ayarları belirtmeniz gerekir.

Kullanım `ContainerSettings` kapsayıcı özgü ayarları yapılandırmak için görev sınıflarında özelliği. Bu ayarları tarafından tanımlanan [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) sınıfı.

Kapsayıcı görüntülerini üzerinde görevleri çalıştırırsanız [bulut görev](/dotnet/api/microsoft.azure.batch.cloudtask) ve [iş yöneticisi görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) kapsayıcı ayarları gerektirir. Ancak, [başlangıç görevi](/dotnet/api/microsoft.azure.batch.starttask), [iş hazırlama görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), ve [iş bırakma görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) kapsayıcı ayarları gerektirmeyen (diğer bir deyişle, bir kapsayıcı bağlam içinde veya doğrudan çalıştırabilirler düğümde).

Normal (kapsayıcı olmayan) bir görev için Batch ayarlayan ortam çok benzer kapsayıcı çalışma dizininde bir Azure Batch kapsayıcı görevi için komut satırı yürütür:

* Tüm dizinler yinelemeli olarak aşağıdaki `AZ_BATCH_NODE_ROOT_DIR` (kök düğümünde Azure Batch dizinlerin) kapsayıcıya eşlenir
* Tüm görev ortam değişkenlerini kapsayıcıya eşlenir.
* Uygulama paketleri ve kaynak dosyaları gibi özelliklerini kullanabilmeniz için uygulamanın çalışma dizini normal bir görev için olanla aynıdır ayarlanır

Batch kapsayıcınızı varsayılan çalışma dizinindeki değiştiğinden, tipik kapsayıcı giriş noktasından farklı bir konumda görev çalışır (örneğin, `c:\` varsayılan olarak bir Windows kapsayıcısı veya `/` Linux'ta). Zaten bu şekilde yapılandırılmazsa görev komut satırı veya kapsayıcı giriş noktanız mutlak bir yol belirttiğinden emin olun.

Docker Hub'ından çekilir bir Ubuntu kapsayıcısında çalışan temel bir komut satırı aşağıdaki Python kod parçacığı gösterir. Kapsayıcı çalıştırma seçenekleri için ek bağımsız değişkenler `docker create` görevi yürütecek komut. Burada, `--rm` seçeneği görev tamamlandıktan sonra kapsayıcıyı kaldırır.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/echo hello',
    container_settings=task_container_settings
)

```

Aşağıdaki C# örneği temel kapsayıcı ayarları bulut görev gösterilmektedir:

```csharp
// Simple container task command

string cmdLine = "c:\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Sonraki adımlar

* Ayrıca bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard) kapsayıcı yüklerinin Azure toplu işlem ile kolay dağıtım için Araç Seti [Shipyard tarifler](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Yükleme ve Linux'ta Docker CE kullanma hakkında daha fazla bilgi için bkz. [Docker](https://docs.docker.com/engine/installation/) belgeleri.

* Özel görüntüleri kullanma ile ilgili daha fazla bilgi için bkz: [sanal makine havuzu oluşturmak için yönetilen bir özel görüntü kullanmak ](batch-custom-images.md).

* Daha fazla bilgi edinin [Moby proje](https://mobyproject.org/), kapsayıcı tabanlı sistemler oluşturmaya yönelik bir çerçeve.