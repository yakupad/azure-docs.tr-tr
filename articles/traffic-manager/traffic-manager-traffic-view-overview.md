---
title: Trafiği Azure trafik Yöneticisi görünümü | Microsoft Docs
description: Trafik Yöneticisi trafiği görünüm giriş
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 7ce51017fdee92e5589c06b398c9650930d5436d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30179846"
---
# <a name="traffic-manager-traffic-view"></a>Trafik Yöneticisi trafiği görünümü

Trafik Yöneticisi, böylece son kullanıcılarınızın yönlendirme yöntemini esas alarak sağlıklı uç noktalar yönlendirilir DNS düzeyi yönlendirme profili oluşturduğunuzda belirttiğiniz sağlar. Trafik görünümü trafik Yöneticisi, kullanıcı tabanları (düzeyinde bir DNS Çözümleyicisi ayrıntı düzeyi) ve bunların trafiğini desen bir görünüm sağlar. Trafik görünüm etkinleştirdiğinizde, bu bilgileri ile eyleme dönüştürülebilir Öngörüler sağlamak için işlenir. 

Trafik görünümünü kullanarak şunları yapabilirsiniz:
- (bir yerel DNS Çözümleyicisi düzeyi ayrıntı düzeyi kadar), kullanıcı temellerine bulunduğu anlayın.
- (Azure Traffic Manager tarafından işlenen DNS sorgularını olarak gözlenen) trafiği hacmi görüntülemek bu bölgelerden kaynaklanan.
- Bu kullanıcılar tarafından yaşadı temsilcisi gecikme süresi nedir içine Öngörüler alın.
- Azure bölgeleri uç noktaları sahip olduğu için bu kullanıcı temellerine her derin Dalış belirli bir trafik düzenleri içine. 

Örneğin, hangi bölgelerin çok sayıda trafiği sahip ancak daha yüksek gecikme yaşar anlamak için trafiği görünümü kullanabilirsiniz. Ardından, bu kullanıcıların daha düşük gecikme süresi deneyimi böylece, yeni Azure bölgeleri ayak izini genişletmeye planlamak için bu bilgileri kullanabilirsiniz.

## <a name="how-traffic-view-works"></a>Trafik görünümü nasıl çalışır?

Trafik Yöneticisi bu özelliği etkinleştirilmiş bir profili karşı son yedi gün içinde alınan gelen sorguları bakmak sağlayarak trafiği görünüm çalışır. Gelen sorguları bilgileri, kullanıcıların konumunu bir gösterimi kullanılan DNS Çözümleyicisi kaynak IP trafiği görünüm ayıklar. Bunlar ardından birlikte trafik Yöneticisi tarafından tutulan IP adreslerinin coğrafi bilgileri kullanarak kullanıcı temel bölgeler oluşturmak için bir DNS Çözümleyicisi düzeyi ayrıntı düzeyi adresindeki gruplandırılır. Trafik Yöneticisi ardından sorgu yönlendirildi ve bu bölgelerdeki kullanıcılar için bir trafik akış eşleme oluşturur Azure bölgeleri bakar.  
Sonraki adımda, trafik Yöneticisi Azure bölgesi eşleme bu bölgelerdeki kullanıcılar tarafından yaşadı ortalama gecikme süresi anlamak farklı son kullanıcı ağlar için tutar ağ Intelligence gecikme tablolar ile kullanıcı temel bölgesine karşılık gelen zaman Azure bölgeleri bağlanılıyor. Bu hesaplamalar adresindeki birleştirilir bir gerçekleştiriliyordu size görüntülenmeden önce yerel DNS Çözümleyicisi IP. Çeşitli şekillerde bilgileri kullanabilir.

>[!NOTE]
>Trafik görünümünde açıklanan gecikme son kullanıcı ve kendisine bağlı Azure bölgeleri arasındaki temsili bir gecikme süresi ve DNS Arama gecikme değil. Yerel DNS Çözümleyicisi ve kullanılabilir yeterli veri ise sorgu için yönlendirildi Azure bölgesi arasındaki gecikme süresi daha sonra gecikme süresini en iyi çaba tahmin döndürülen trafiği görünüm yapar null olur. 

## <a name="visual-overview"></a>Görsel genel bakış

Ne zaman gezinmek için **trafiği Görünüm** bölüm trafik Yöneticisi sayfanızda bir coğrafi harita trafiği görünüm Öngörüler kaplamasını ile birlikte sunulur. Harita kullanıcı tabanı ve trafik Yöneticisi profil için uç noktalar hakkında bilgi sağlar.

### <a name="user-base-information"></a>Kullanıcı temel bilgileri

Hangi konum için bilgi mevcut değil Bu yerel DNS Çözümleyicileri için bunlar eşlemesinde gösterilir. DNS Çözümleyicisi rengini trafik Yöneticisi sorgularını için o DNS Çözümleyicisi kullanan son kullanıcılar tarafından deneyimli ortalama gecikme süresini gösterir.

Harita DNS Çözümleyicisi konumda üzerine getirirseniz gösterir:
- DNS Çözümleyicisi IP adresi
- Trafik Yöneticisi tarafından buradan görülen DNS sorgu trafiğinin hacmini
- hangi trafiğe DNS'den Çözümleyici, uç nokta ve DNS Çözümleyicisi arasında bir çizgi olarak yönlendirildi uç noktaları 
- Bu konumdan ortalama gecikme süresi bunları bağlayan bir çizgi rengi temsil uç noktası

### <a name="endpoint-information"></a>Uç nokta bilgileri

Uç noktaları bulunduğu Azure bölgeleri eşlemesindeki mavi noktalar olarak gösterilir. Uç noktanız dış ve eşlenmiş bir Azure bölgesi yoksa harita üstünde gösterilir. (Kullanılan DNS Çözümleyicisi göre) farklı konumlara görmek için herhangi bir uç nokta gelen trafik Bu uç noktasına burada yönlendirilmiş olan tıklayın. Bağlantı uç nokta ve DNS Çözümleyicisi konum arasında bir çizgi olarak gösterilir ve bu çifti arasındaki temsilcisi gecikme süresi göre renkli. Buna ek olarak, uç nokta, içinde çalıştığı Azure bölgesi ve bu trafik Yöneticisi profili tarafından yönlendirilen isteklerini kendisine toplam hacmini adını görebilirsiniz.


## <a name="tabular-listing-and-raw-data-download"></a>Tablo listesi ve ham verileri yükleme

Tablo biçiminde Azure portalında trafik görünüm verilerini görüntüleyebilir. Her DNS Çözümleyicisi IP için bir giriş veya uç nokta eşleştirin IP adresini ve DNS Çözümleyicisi, adı Azure bölgesinde coğrafi konumunu gösterir, uç nokta (varsa) bulunur, bu DNS Çözümleyicisi ile ilişkili istek hacmini Bu uç ve son kullanıcılara bu DNS (kullanılabiliyorsa) kullanarak ilgili temsili gecikmeyi. Tercih ettiğiniz bir analytics iş akışının bir parçası olarak kullanılabilir bir CSV dosyası olarak trafik görünüm verileri de yükleyebilirsiniz.

## <a name="billing"></a>Faturalandırma

Trafik görünümü kullandığınızda, sunulan öngörüleri oluşturmak için kullanılan veri noktalarının sayısına göre faturalandırılır. Şu anda kullanılan tek veri noktası Traffic Manager profilinize karşı alınan sorguları türüdür. Fiyatlandırma hakkında daha fazla ayrıntı için ziyaret [trafik Yöneticisi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [trafik Yöneticisi nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinmek [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

