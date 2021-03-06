---
title: Azure Service Fabric Mesh için sık sorulan sorular | Microsoft Docs
description: Azure Service Fabric Mesh için sık sorulan sorular ve yanıtlar hakkında bilgi edinin.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 07c0347793f6541a3e047f3f357d0d1b05dc3bca
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136190"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Sık sorulan sorular Service Fabric Mesh
Azure Service Fabric Mesh geliştiricilerin mikro hizmet uygulamaları sanal makineler, depolama, yönetme veya ağ dağıtmanıza olanak sağlayan tam olarak yönetilen bir hizmettir. Bu makalede sık sorulan soruların yanıtları bulunur.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Nasıl bir sorun bildirin veya soru sormak?

Soru sorun, Microsoft mühendisinin cevaplar ve raporlayın [service-fabric-kafes-preview GitHub deposunu](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Kota ve maliyet

**Önizlemede katılım maliyeti nedir?**

Kafes Önizleme uygulamalar veya kapsayıcıları dağıtmak için ücretlendirme yoktur. Etkin bir şekilde sınamakta olduğunuz sürece ancak dağıtın ve bunları bırakmamaya kaynakları silmeniz önerilir, çalışıyor.

**Çekirdek sayısını ve RAM'i kota sınırı var mı?**

Evet, her abonelik için kotalar şunlardır:

- Uygulamalar - 5 sayısı 
- Uygulama – 12 başına çekirdek sayısı 
- Uygulama - 48 GB başına toplam RAM 
- Ağ ve giriş uç noktalarının sayısı – 5  
- Azure ekleyebileceğiniz - birim sayısı 10 
- Hizmet çoğaltma – 3 sayısı 
- 4 çekirdek, 16 GB RAM dağıtabileceğiniz en büyük kapsayıcı sınırlıdır.
- En çok 6 çekirdek 0,5 çekirdek artışlarla kapsayıcılarınızı kısmi çekirdek ayırabilirsiniz.

**Uygulamamın gece çalıştıran bildirebilirim?**

Evet, kullanabilirsiniz, ancak dağıtın ve bunları bırakmamaya kaynakları silmek için kullanmaları çalışan etkin bir şekilde sınamakta olduğunuz sürece. Bu ilke gelecekte değişebilir ve bunlar yanlış kaynakları silinmiş.

## <a name="supported-container-os-images"></a>Desteklenen kapsayıcı işletim sistemi görüntüleri
Aşağıdaki kapsayıcı işletim sistemi görüntüleri, Hizmetleri dağıtım yaparken kullanılabilir.

- Windows - windowsservercore ve nanoserver
    - Windows Server 2016
    - Windows Server 1709 sürümü
- Linux
    - Hiçbir bilinen sınırlamalar

## <a name="features-gaps-and-known-issues"></a>Özellikleri boşlukları ve bilinen sorunlar

**Uygulamamın dağıttıktan sonra kendisiyle ilişkilendirilmiş bir ağ kaynağına bir IP adresi olarak görünmüyor**

Bugün bir gecikmeden sonra geldiğini IP adresi ile ilgili bilinen bir sorun yoktur. İlişkili IP adresi görmek için birkaç dakika içinde ağ kaynak durumunu denetleyin.

**Uygulamamın doğru ağ/birim kaynağına erişemiyor**

Uygulama modelinizde ilişkili kaynak erişebilmesi için ağları ve birimler için tam kaynak Kimliğini kullanmanız gerekir. Hızlı Başlangıç örnek göründüğüne aşağıda verilmiştir:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**My parolaları şifrelemek için bir yol destekleyen geçerli uygulama modeli görmez**

Evet, gizli dizileri şifrelemek geçerli özel Önizleme sürümünde desteklenmiyor. 

**DNS kafes ve Service Fabric geliştirme kümem de aynı şekilde çalışmaz**

Burada, yerel geliştirme kümenizin farklı şekillerde ve Azure Ağ Hizmetleri başvuru yapması gerekli olabilir bilinen bir sorun yoktur. {ServiceName}, yerel geliştirme kümenizin kullanın. {applicationName}. Azure Service Fabric Mesh, {servicename} kullanın. Azure ağı uygulamalarda dns çözümlemesi şu anda desteklemiyor.

Burada bir Service Fabric geliştirme kümesi üzerinde Windows 10 çalıştıran ile ilgili bilinen diğer DNS sorunlar için bkz: [hata ayıklama Windows kapsayıcıları](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**Bu hata CLI modülü _ImportError kullanırken Al: adı 'sdk_no_wait' içeri aktarılamıyor**

2.0.30 daha eski CLI sürümünü kullanıyorsanız, bu hatayı alabilirler-

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**Bir türde bir eşleşmeme dağıtım adı hatası CLI uzantı paketi yüklerken Al**

Bu uzantı yüklenmediğini anlamına gelmez. Hala sorun yaşamadan CLI komutları kullanmanız mümkün olması gerekir.

**Çıkış ölçeklediğinizde my tüm kapsayıcıları, my çalışan olanlar da dahil olmak üzere etkilenen olduğunu görüyorum**

Bu bir hatadır ve sonraki çalışma zamanı yenileme düzeltilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh hakkında daha fazla bilgi edinmek için [genel bakış](service-fabric-mesh-overview.md).
