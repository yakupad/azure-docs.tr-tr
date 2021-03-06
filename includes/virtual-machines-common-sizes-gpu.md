---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 07/06/2018
ms.author: danlep;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 268da0e5d078f7b6b4b36929dbf6755068adb444
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37907183"
---
GPU için iyileştirilmiş sanal makine boyutları olan özel sanal makineler tek veya birden çok NVIDIA GPU'ları ile kullanılabilir. Bu boyutları görselleştirme yoğun işlem gücü kullanımlı ve grafik kullanımlı iş yükleri için tasarlanmıştır. Bu makalede sayısı ve gpu'ları, Vcpu, veri diskleri ve NIC türü hakkında bilgi sağlar. Depolama aktarım hızı ve ağ bant genişliği de bu gruplandırmaki her boyut için dahil edilir. 

* **NC, NCv2 NCv3 ve ND** boyutları, yoğun işlem ve ağ kullanımlı uygulamalar ve algoritmalar için iyileştirilmiştir. CUDA ve OpenCL tabanlı uygulama ve simülasyonlar, yapay ZEKA ve derin öğrenme örnek verilebilir. 
* **NV** boyutları en iyi duruma getirilmiş ve Uzaktan görselleştirme, akışı, oyun, kodlama ve OpenGL ve DirectX gibi çerçeveleri kullanan VDI senaryoları için tasarlanmıştır.  


## <a name="nc-series"></a>NC serisi

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

NC serisi VM'ler ile desteklenen [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) kart. Kullanıcılar verileri daha hızlı enerji keşif uygulamaları CUDA yararlanarak işleyin, benzetimleri kilitlenme, izlenen işleme, derin öğrenme ve daha fazlasını ışın. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24r yapılandırması sunar.


| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 340 | 1 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 | 4 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

## <a name="ncv2-series"></a>NCv2 serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

NCv2 serisi VM'ler ile desteklenen [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU'ları. Bu GPU'ları NC serisi işlem performansı x 2'den sağlayabilir. Müşteriler, modellemesi, DNA sıralama, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yüklerinde bu güncelleştirilmiş GPU avantajlarından yararlanabilirsiniz. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24rs v2 yapılandırmasını sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizdeki vCPU (çekirdek) kotasını başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | ---  |
| Standard_NC6s_v2 |6 |112 | 736 | 1 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 1474 | 2 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 2948 | 4 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 2948 | 4 | 32 | 8 |

1 GPU = bir P100 kart.

*RDMA özellikli

## <a name="ncv3-series"></a>NCv3 serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

NCv3 serisi VM'ler ile desteklenen [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU'ları. Bu GPU'ları 1,5 kat NCv2 serisi işlem performansı sağlayabilir. Müşteriler, modellemesi, DNA sıralama, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yüklerinde bu güncelleştirilmiş GPU avantajlarından yararlanabilirsiniz. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24rs v3 yapılandırmasını sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizdeki vCPU (çekirdek) kotasını başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 32 | 8 |

1 GPU = bir V100 kart.

*RDMA özellikli

## <a name="nd-series"></a>ND serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

ND serisi sanal makineler, yapay ZEKA ve derin öğrenme iş yükleri için tasarlandı GPU ailesine yeni eklenen olur. Bunlar, eğitim ve çıkarım için mükemmel performans sunar. ND örnekleri ile desteklenen [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU'ları. Bu örnekler tek duyarlıklı kayan nokta işlemleri, Microsoft Bilişsel araç seti, TensorFlow, Caffe ve diğer çerçeveleri kullanan AI iş yükleri için mükemmel bir performans sağlar. Ayrıca, ND serisi çok daha büyük bir GPU bellek boyutu (24 GB) sunarak daha büyük sinir ağı modellerinin sığdırılmasına imkan tanır. NC serisinde olduğu gibi ND serisi RDMA aracılığıyla ikincil bir düşük gecikme süreli, yüksek performanslı ağ yapılandırmasıyla sunar ve Infiniband bağlantısı birçok GPU'yu kapsayan büyük ölçekli eğitim işleri çalıştırabilirsiniz.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizi bölge başına vCPU (çekirdek) kotasını başlangıçta 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 736 | 1 | 12 | 4 |
| Standard_ND12s |12 |224 | 1474 | 2 | 24 | 8 | 
| Standard_ND24s |24 |448 | 2948 | 4 | 32 | 8 |
| Standard_ND24rs * |24 |448 | 2948 | 4 | 32 | 8 |

1 GPU = bir P40 kart.

*RDMA özellikli

## <a name="nv-series"></a>NV serisi

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

NV serisi sanal makineler tarafından desteklenen [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID teknoloji Masaüstü için hızlandırılmış uygulamaları ve sanal masaüstü müşterilerin verilerini veya simülasyonlarını görselleştirmek mümkün olduğu. Kullanıcılar üst grafik özelliğine sahip olursunuz ve buna ek olarak kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için NV örnekleri üzerinde grafik açısından yoğun kaynak gerektiren iş akışlarını görselleştirin olanağına sahip olursunuz. 

NV örnekleri, her bir GPU kılavuz lisansı ile birlikte gelir. Bu lisans size NV örneği, tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik veya 25 eş zamanlı kullanıcı bir sanal uygulama senaryosu için VM'ye bağlanabilirsiniz.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC | Sanal çalışma İstasyonlarınızı | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |340 | 1 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 | 4 | 4 | 100 |

1 GPU = M60 kartın yarısı.

 
