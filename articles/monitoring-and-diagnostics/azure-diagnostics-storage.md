---
title: Azure Depolama’da Tanılama Verilerini Depolama ve Görüntüleme
description: Azure Tanılama verileri Azure depolama alanına almak ve görüntüleme
services: azure-monitor
author: thraka
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/01/2016
ms.author: adegeo
ms.component: diagnostic-extension
ms.openlocfilehash: 6590e6991f07b7315c09a995152879c991fafcef
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267671"
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Azure Storage deposu ve görünüm Tanılama verileri
Microsoft Azure storage öykünücüsü veya Azure depolama transfer sürece Tanılama verileri kalıcı olarak depolanmaz. Bir kez depolama biriminde, onu birkaç kullanılabilen araçlar biriyle görüntülenebilir.

## <a name="specify-a-storage-account"></a>Bir depolama hesabı belirtin
ServiceConfiguration.cscfg dosyasında kullanmak istediğiniz depolama hesabı belirtin. Hesap bilgileri, bir bağlantı dizesi bir yapılandırma ayarı olarak tanımlanır. Aşağıdaki örnek, Visual Studio'da yeni bir bulut hizmeti projesi için oluşturulan varsayılan bağlantı dizesini gösterir:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Bir Azure depolama hesabı için hesap bilgilerini sağlamak için bu bağlantı dizesi değiştirebilirsiniz.

Toplanacak tanılama veri türüne bağlı olarak, Azure tanılama Blob hizmeti veya tablo hizmeti kullanır. Aşağıdaki tabloda, kalıcı veri kaynaklarını ve bunların biçimi gösterir.

| Veri kaynağı | Depolama biçimi |
| --- | --- |
| Azure günlükleri |Tablo |
| IIS 7.0 günlükleri |Blob |
| Azure Tanılama altyapısı günlükleri |Tablo |
| Başarısız istek izleme günlükleri |Blob |
| Windows Olay günlükleri |Tablo |
| Performans sayaçları |Tablo |
| Kilitlenme bilgi dökümleri |Blob |
| Özel hata günlükleri |Blob |

## <a name="transfer-diagnostic-data"></a>Tanılama veri aktarımı girişi
Tanılama veri aktarım isteği, SDK 2.5 ve daha sonra yapılandırma dosyası aracılığıyla ortaya çıkabilir. Yapılandırmada belirtilen zamanlanan aralıklarla tanılama veri aktarın.

SDK 2.4 ve önceki Tanılama verileri de yapılandırma dosyası aracılığıyla programlı olarak aktarmak isteyebilir. Programsal yaklaşım isteğe bağlı aktarımları yapmanıza olanak sağlar.

> [!IMPORTANT]
> Bir Azure depolama hesabına tanılama veri aktarırken tanılama verilerini kullanan depolama alanı kaynakları için ücrete neden.
> 
> 

## <a name="store-diagnostic-data"></a>Tanılama verileri depola
Günlük verileri Blob veya tablo depolama aşağıdaki adlarla depolanır:

**tabloları**

* **WadLogsTable** - İzleme dinleyicisi kullanarak kod içinde yazılmış günlükleri.
* **WADDiagnosticInfrastructureLogsTable** -Tanılama izleme ve yapılandırma değişikliklerini.
* **WADDirectoriesTable** – tanı İzleyicisi İzleme dizinleri.  Bu, IIS günlüklerini içerir, IIS istek günlüklerini ve özel dizinleri başarısız oldu.  Blob günlük dosyasının konumunu kapsayıcı alanında belirtilen ve blob adını RelativePath alanıdır.  Azure sanal makinede var gibi AbsolutePath alan dosyasının adını ve konumunu gösterir.
* **WADPerformanceCountersTable** – performans sayaçları.
* **WADWindowsEventLogsTable** – Windows olayı günlüğe kaydeder.

**Bloblar**

* **wad denetimi kapsayıcısı** – (yalnızca SDK 2.4 ve önceki) Azure tanılama denetimleri XML yapılandırma dosyalarını içerir.
* **wad IIS failedreqlogfiles** – IIS isteği başarısız oldu günlüklerdeki bilgiler içerir.
* **wad IIS logfiles** – IIS günlüklerini hakkında bilgiler içerir.
* **"özel"** – özel bir kapsayıcı dayalı tanı İzleyicisi tarafından izlenen dizinleri yapılandırma.  Bu blob kapsayıcısının adı WADDirectoriesTable belirtilir.

## <a name="tools-to-view-diagnostic-data"></a>Tanılama verileri görüntülemek için Araçlar
Depolama birimine aktarıldıktan sonra verileri görüntülemek birkaç araç bulunmaktadır. Örneğin:

* Microsoft Visual Studio için Azure araçlarını yüklediyseniz, Visual Studio - sunucu Gezgini'nde, Azure Depolama düğümü sunucu Gezgini'nde salt okunur blob ve tablo verileri Azure depolama hesaplarından görüntülemek için kullanabilirsiniz. Yerel depolama öykünücüsü hesabınızdan verileri görüntüleyebilir ve ayrıca depolama hesaplarından Azure için oluşturduğunuz. Daha fazla bilgi için bkz: [gözatma ve Sunucu Gezgini ile depolama kaynaklarını yönetme](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).
* [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) , Windows, OSX ve Linux Azure Storage ile kolayca çalışmanızı sağlayan bir tek başına uygulamadır.
* [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) , görüntülemek, indirin ve Azure üzerinde çalışan uygulamalar tarafından toplanan tanılama verilerini yönetmenize olanak sağlayan Azure tanılama Manager içerir.

## <a name="next-steps"></a>Sonraki Adımlar
[Bulut Hizmetleri uygulaması Azure Tanılama ile akışında izleme](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)

