---
title: SendGrid e-posta hizmetini (Node.js) kullanma | Microsoft Docs
description: Bilgi e-posta SendGrid e-posta hizmeti ile Azure üzerinde nasıl gönderin. Node.js API'si kullanılarak yazılan kod örnekleri.
services: ''
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: ''
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: 327cea3a24cc47a9cc463b37cc2346ebc475ef7f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38701864"
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Node.js'den SendGrid kullanarak e-posta gönderme
Bu kılavuzda, Azure üzerinde SendGrid e-posta hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Örnek Node.js API'si kullanılarak yazılır. Senaryoları ele alınmaktadır **e-posta oluşturma**, **e-posta gönderme**, **ekler eklenirken**, **filtreleri kullanarak**ve **özelliklerini güncelleştirme**. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps) bölümü.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid e-posta hizmeti nedir?
SendGrid olduğu bir [bulut tabanlı e-posta hizmeti] sağlayan güvenilir [İşlem tabanlı e-posta teslimi], ölçeklenebilirlik ve gerçek zamanlı analiz, özel tümleştirmeyi kolaylaştıran esnek API'ler ile birlikte. SendGrid kullanım senaryoları şunları içerir:

* Otomatik olarak okundu bilgilerini müşterilere gönderme
* Müşteriler aylık e-ilanlara ve özel teklifler göndermek için dağıtım yönetme listeler
* Engellenen e-posta ve müşteri yanıt hızı gibi şeyler için gerçek zamanlı ölçümler toplama
* Eğilimleri belirlemenize yardımcı olması için rapor oluşturma
* İletme müşteri sorguları
* Uygulamanızdan e-posta bildirimleri

Daha fazla bilgi için bkz. [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>SendGrid Node.js modül başvurusu
Node.js için SendGrid Modülü aşağıdaki komutu kullanarak düğüm paketi yöneticisini (npm) yüklenebilir:

    npm install sendgrid

Yüklemeden sonra aşağıdaki kodu kullanarak uygulamanızda modülün gerektirebilir:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

SendGrid modülü aktarır **SendGrid** ve **e-posta** işlevleri.
**SendGrid** Web API'si aracılığıyla e-posta göndermekten sorumludur sırada **e-posta** bir e-posta iletisi kapsüller.

## <a name="how-to-create-an-email"></a>Nasıl yapılır: bir e-posta oluşturma
Modül SendGrid kullanarak e-posta iletisi oluşturmak, ilk e-posta işlevi kullanılarak ve SendGrid işlevi kullanarak göndermek için bir e-posta iletisi oluşturmayı içerir. E-posta işlevi kullanarak yeni bir ileti oluşturma örneği verilmiştir:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

Html özelliğini ayarlayarak destekleyen istemciler için bir HTML iletisi de belirtebilirsiniz. Örneğin:

    html: This is a sample <b>HTML<b> email message.

Metin içeriği için normal bir geri dönüş metin ve html özellikleri ayarlama HTML iletileri destekleyemiyorsa istemciler için sağlar.

E-posta işlevi tarafından desteklenen tüm özellikleri hakkında daha fazla bilgi için bkz. [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Nasıl yapılır: bir e-posta Gönder
E-posta işlevi kullanarak bir e-posta iletisi oluşturduktan sonra SendGrid tarafından sağlanan Web API'sini kullanarak gönderebilirsiniz. 

### <a name="web-api"></a>Web API
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> Yukarıdaki örnek bir e-posta nesne ve geri çağırma işlevi geçirme gösterirken, doğrudan e-posta özellikleri belirterek de doğrudan kullanılacak gönderme işlevi çağırabilirsiniz. Örneğin:  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: bir ek ekleyin
Ekleri eklenebilir bir ileti yollarda ve dosya adları belirterek **dosyaları** özelliği. Aşağıdaki örnek, bir ek gönderme gösterir:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> Kullanırken **dosyaları** özelliği, dosya olmalıdır üzerinden erişilebilir [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Eklemek istediğiniz dosyayı, Azure Depolama'da bir Blob kapsayıcısı gibi barındırılıyorsa kullanarak ek olarak gönderilmeden önce öncelikle dosyayı yerel depolama veya Azure bir sürücüye kopyalamanız gerekir **dosyaları** özelliği.
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a>Nasıl yapılır: Enable altbilgiler ve izleme için filtreleri kullanın
SendGrid filtreleri kullanarak ek e-posta işlevselliğini sağlar. Bu izleme'ye tıklayın, Google analytics, izleme aboneliği ve benzeri etkinleştirme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisi eklenebilir ayarlarıdır. Filtreler tam bir listesi için bkz. [Filtresi Ayarları][Filter Settings].

Filtre uygulanabilir bir iletiyi kullanarak **filtreleri** özelliği.
Her filtre filtre özgü ayarları içeren bir karma olarak belirtilir.
Aşağıdaki örnekler, alt bilgi göstermek ve filtreleri İzleme'yi tıklatın:

### <a name="footer"></a>Alt bilgi
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a>İzleme'ye tıklayın.
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a>Nasıl yapılır: e-posta özelliklerini güncelleştir
Bazı e-posta özellikleri kullanılarak üzerine yazılabilir **ayarlamak*özellik*** veya kullanarak eklenmiş **ekleme*özelliği***. Örneğin, kullanarak ek alıcılar ekleyebilir miyim

    email.addTo('jeff@contoso.com');

veya bir filtre kullanarak ayarlayın.

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

Daha fazla bilgi için [sendgrid nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl yapılır: ek SendGrid hizmetlerini kullanma
SendGrid, Azure uygulamanızı ek SendGrid işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm Ayrıntılar için bkz. [SendGrid API belgeleri][SendGrid API documentation].

## <a name="next-steps"></a>Sonraki Adımlar
SendGrid e-posta hizmeti ile ilgili temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* SendGrid Node.js modülü deposu: [sendgrid nodejs][sendgrid-nodejs]
* SendGrid API belgeleri: <https://sendgrid.com/docs>
* Azure müşterileri için SendGrid özel tekliftir: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[İşlem tabanlı e-posta teslimi]: https://sendgrid.com/transactional-email
