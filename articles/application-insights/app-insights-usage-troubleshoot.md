---
title: Kullanıcı davranış analizi araçları Azure Application ınsights sorunlarını giderme
description: Sorun giderme kılavuzu - Application Insights ile site ve uygulama kullanımını analiz etme.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 07/11/2018
ms.reviewer: daviste
ms.author: mbullwin
ms.openlocfilehash: 725f67af8178c6c851999d18c771ebdd360d6d01
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991447"
---
# <a name="troubleshoot-user-behavior-analytics-tools-in-application-insights"></a>Kullanıcı davranış analizi araçları Application ınsights sorunlarını giderme
Hakkında sorularınız [kullanıcı davranış analizi araçları Application ınsights'ta](app-insights-usage-overview.md): [kullanıcılar, oturumlar, etkinlikler](app-insights-usage-segmentation.md), [Huniler](usage-funnels.md), [kullanıcı akışları](app-insights-usage-flows.md), [Bekletme](app-insights-usage-retention.md), veya Kohortlar? Bazı soruların yanıtları aşağıdadır.

## <a name="counting-users"></a>Kullanıcı sayımı
**Kullanıcı davranış analizi araçları, bir kullanıcı oturumunu Uygulamam var, ancak birçok kullanıcı oturumları uygulamamı sahip biliyorum gösterir. Bu yanlış sayım nasıl düzeltebilirim?**

Application ınsights tüm telemetri olaylarını sahip bir [anonim kullanıcı kimliği](application-insights-data-model-context.md) ve [oturum kimliği](application-insights-data-model-context.md) iki standart özellikleri olarak. Varsayılan olarak, tüm kullanım analizi araçları, kullanıcılar ve bu kimliklerine göre çıkarak sayısı. Bu standart özellikler her kullanıcı ve uygulama oturumu için benzersiz kimlikler ile doldurulmasını olmayan kullanıcılar ve kullanım analizi araçları oturumlarda sayımının yanlış görürsünüz.

Bir web uygulaması izleme yapıyorsanız, kolay çözümü eklemek için ise [Application Insights JavaScript SDK'sı](app-insights-javascript.md) , uygulama ve emin için kod parçacığı izlemek istediğiniz her sayfada yüklenir. JavaScript SDK'yı otomatik olarak anonim kullanıcı ve oturum kimliklerini oluşturur ve ardından uygulamanızdan gönderilen gibi telemetri olayları bu kimlikleri ile doldurur.

Bir web hizmeti (hiçbir kullanıcı arabirimi) izliyorsanız [anonim kullanıcı kimliği ve oturum kimliği özellikleri dolduran bir telemetri Başlatıcısı oluşturma](app-insights-usage-send-user-context.md) göre benzersiz kullanıcı ve oturum hizmetinizin kavramları.

Uygulamanızı gönderiyorsa [kimliği doğrulanmış kullanıcı kimlikleri](app-insights-api-custom-events-metrics.md#authenticated-users), bağlı olarak kimliği doğrulanmış kullanıcı kimlikleri kullanıcılar aracında güvenebilirsiniz. "Show" açılır menüden "Kimliği doğrulanan kullanıcılar" seçin

Kullanıcı davranış analizi araçları sayım kullanıcılar veya anonim kullanıcı kimliği, kimliği doğrulanmış kullanıcı kimliği veya oturum kimliği dışındaki özellikleri göre oturumları şu anda desteklenmiyor

## <a name="naming-events"></a>Adlandırma olayları
**Binlerce farklı sayfa görünümü ve özel olay adlarının Uygulamam var. Bunlar arasında ayrım yapmak zordur ve kullanıcı davranış analizi araçları genellikle yanıt veremez duruma gelebilir. Bu adlandırma sorunları nasıl düzeltebilirim?**

Sayfa görünümü ve özel olay adlarının kullanıcı davranış analizi araçları kullanılır. Olayları da adlandırma değeri bu Araçları'ndan almak için önemlidir. Hedef arasında bir denge ise çok az sayıda, aşırı genel adlar ("tıklanan Button") sahip ve çok fazla aşırı belirli adları olan ("Düzenle düğmesini tıkladığınız http://www.contoso.com/index").

Sayfa görünümü ve özel olay adlarının uygulamanızı göndermek için değişiklik yapmak için uygulamanızın kaynak kodu ve yeniden dağıtma değiştirmeniz gerekir. **Application Insights'ta verileri 90 gün boyunca saklanır ve silinemez tüm telemetri**, olay adları yaptığınız değişikliklerin tam listesi için 90 gün sürer. 90 ad değişiklikleri yaptıktan sonra gün için eski ve yeni olay adları telemetrinizi gösterilir, böylece sorguları ayarlamak ve takımlarınızın içinde uygun şekilde iletişim.

Uygulamanız çok fazla sayfa görünümü adları gönderiyorsa, bu sayfanın görünüm adları kodda el ile belirtilen olup olmadığını veya bunlar otomatik olarak Application Insights JavaScript SDK tarafından gönderilen, kontrol edin:

* Sayfa görünümü adları el ile kod kullanarak belirtilmezse [ `trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md), daha az belirgin olması için adı değiştirin. Sayfa görünümü adı URL koyarak gibi yaygın hataları kaçının. Bunun yerine, URL parametresi kullanın `trackPageView` API. Diğer ayrıntılar sayfa görünümü adından özel özelliklerini taşıyın.

* Application Insights JavaScript SDK'sını otomatik olarak sayfa görünümü adları gönderiyorsa, sayfaların başlıkları değiştirebilir veya el ile sayfa görünümü adları nasıl geçiş yapabilirim. SDK gönderir [başlık](https://developer.mozilla.org/docs/Web/HTML/Element/title) sayfa görünümü adı varsayılan olarak her sayfanın. Daha fazla genel, ancak bu değişiklik sahip diğer etkiler ve SEO ve oluşturduğunu, başlıklar değiştirebilir. Sayfa görünümü el ile belirtme adları ile `trackPageView` API sayfa başlıklarının değiştirmeden daha genel adlar telemetri gönderebilir şekilde otomatik olarak toplanan adları geçersiz kılar.   

Uygulamanız çok sayıda özel olay adlarının gönderiyorsa, kodda daha az belirgin olacak şekilde değiştirin. Yeniden URL'ler ve diğer sayfa başına veya dinamik bilgi özel olay adları doğrudan eklemekten kaçının. Bunun yerine, özel olay ile özel özellikleri içine bu ayrıntıları Taşı `trackEvent` API. Örneğin, yerine, `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`, aşağıdakine benzer öneririz `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })`.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı davranış analizi araçları genel bakış](app-insights-usage-overview.md)

## <a name="get-help"></a>Yardım alın
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

