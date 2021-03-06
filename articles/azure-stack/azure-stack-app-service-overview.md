---
title: 'Uygulama hizmeti genel bakış: Azure yığın | Microsoft Docs'
description: Azure yığın uygulama hizmeti genel bakış
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: brenduns
ms.reviewer: anwestg
ms.openlocfilehash: 42258a352b05d97341f20c13bf837e1398dd8979
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099971"
---
# <a name="app-service-on-azure-stack-overview"></a>Azure Stack’te App Service’e genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure uygulama hizmeti Azure yığında Azure yığınına kullanılabilir Microsoft Azure platform olarak-hizmet (PaaS) teklifi ' dir. Hizmet müşterilerinizin - iç veya dış sağlar - web, API ve Azure işlevleri oluşturma tüm platform veya cihazlar için uygulamalar. Bunlar, uygulamalarınızı şirket içi uygulamaları ile tümleştirme ve kendi iş süreçlerini otomatikleştirmek. Azure yığın bulut operatörleri müşteri uygulamaları kendi seçtikleri paylaşılan VM kaynakları veya özel VM'ler ile tam olarak yönetilen sanal makinelerde (VM'ler) çalıştırabilirsiniz.

Azure uygulama hizmeti iş süreçlerini ve barındırma Bulutu API'leri otomatikleştirmenizi sağlar. Tek bir tümleşik hizmet olarak Azure App Service, çeşitli bileşenler--Web siteleri, RESTful API'leri ve iş süreçlerini--birleştirmek tek bir çözümde olanak tanır.

## <a name="why-offer-azure-app-service-on-azure-stack"></a>Neden Azure uygulama hizmeti Azure yığında sunar?

App Service’in temel özelliklerinden bazıları şunlardır:

- **Birden çok dil ve çerçeve**: App Service ASP.NET, Node.js, Java, PHP ve Python için birinci sınıf destek vardır. Windows PowerShell ve diğer betikleri veya yürütülebilir dosyalar App Service sanal makineleri üzerinde de çalıştırabilirsiniz.
- **DevOps iyileştirmesi**: sürekli tümleştirme ve dağıtım GitHub, yerel Git veya BitBucket ile ayarlayın. Test ve hazırlık ortamları aracılığıyla güncelleştirmeleri yükseltin. Uygulama hizmeti uygulamalarınızda Azure PowerShell veya platformlar arası komut satırı arabirimi (CLI) kullanarak yönetin.
- **Visual Studio tümleştirmesi**: Visual Studio'daki ayrılmış Araçlar uygulamalar oluşturma ve dağıtma işlemlerini kolaylaştırır.

## <a name="app-types-in-app-service"></a>App Service’deki uygulama türleri

App Service her biri belirli bir iş yüküne barındırmaya yönelik birkaç uygulama türleri sunar:

- [Web uygulamaları](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) Web sitelerini ve web uygulamalarını barındırmak için.
- [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-apps-why-best-platform) RESTful API'lerini barındırmak için.
- Olay yönelimli, sunucusuz iş yüklerini barındırmak için azure işlevleri.

Word uygulaması Buraya bir iş yükünü çalıştırmaya ayrılmış barındırma kaynaklarını ifade eder. “Web uygulaması” örnek olarak alındığında, web uygulamasını tarayıcıya işlev kazandıran işlem kaynaklar ve uygulama kodu bütünü olarak düşünmeye büyük olasılıkla alışkınsınız. Ancak App Service'te bir web uygulaması, uygulama kodunuzun barındırılması için Azure yığını sağlar işlem kaynakları.

Uygulamanız farklı türlerde birden çok uygulama hizmeti uygulamaların birleştirilebilir. Örneğin, uygulamanız bir web ön ucu oluşur ve bir RESTful API'si geri bitiş yoksa, şunları yapabilirsiniz:

- İkisini de (ön uç ve api) tek bir web uygulamasına dağıtma
- Ön uç kodunuzu bir web uygulamasına, arka uç kodunuzu bir API uygulamasına dağıtma.

   ![İzleme verilerini içeren uygulama hizmeti genel bakış](media/azure-stack-app-service-overview/image01.png)

## <a name="what-is-an-app-service-plan"></a>App Service planı nedir?

Uygulama hizmeti kaynak sağlayıcısı Azure uygulama hizmeti kullanan aynı kodu kullanır. Sonuç olarak, bazı ortak kavramlar açıklayan değer var. App Service'te fiyatlandırma kapsayıcı uygulamalar için uygulama hizmeti plan adı verilir. Uygulamalarınızı tutmak için kullanılan ayrılmış sanal makineler kümesini temsil eder. Belirli bir aboneliğe içinde birden çok uygulama hizmeti planına sahip olabilir.

Azure'da, paylaşılan ve ayrılmış çalışanlar vardır. Paylaşılan çalışan yüksek yoğunluklu çok kiracılı uygulama barındırma destekler ve paylaşılan Çalışanlar yalnızca bir kümesi yok. Adanmış sunucular yalnızca bir kiracı tarafından kullanılan ve üç boyut halinde gelen: küçük, Orta ve büyük. Şirket içi müşterilerin ihtiyaçlarını her zaman bu koşulları'nı kullanarak açıklanan olamaz. Azure yığında App Service'te kullanılabilir hale getirmek istedikleri çalışan katmanları kaynak sağlayıcısı Yöneticiler tanımlayabilir. Benzersiz barındırma gereksinimlerinize bağlı olarak, birden çok paylaşılan çalışanlar kümesini veya adanmış çalışanların farklı kümelerini tanımlayabilirsiniz. Bu çalışan katmanı tanımlarını kullanarak, bunlar daha sonra kendi SKU'ları fiyatlandırması tanımlayabilirsiniz.

## <a name="portal-features"></a>Portal özellikleri

Azure uygulama hizmeti kullanan aynı kullanıcı arabirimini Azure yığın uygulama hizmeti kullanıyorsa, aynı arka uç ile geçerlidir. Ancak, bazı özellikler devre dışı bırakılır ve Azure yığınında işlevsel değildir. Azure özel beklentileri ya da bu özellikleri gerektiren hizmetleri Azure yığınında şu anda kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığın uygulama hizmeti ile çalışmaya başlamadan önce](azure-stack-app-service-before-you-get-started.md)
- [Uygulama hizmeti kaynak Sağlayıcısı'nı yüklemek](azure-stack-app-service-deploy.md)

Ayrıca diğer deneyebilirsiniz [platform olarak hizmet (PaaS) Hizmetleri](azure-stack-tools-paas-services.md), gibi [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md) ve [MySQL kaynak sağlayıcı](azure-stack-mysql-resource-provider-deploy.md).
