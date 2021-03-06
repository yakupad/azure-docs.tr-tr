---
title: Twilio ses ve SMS (Ruby) için kullanma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. Ruby'de yazılan kod örneklerini içerir.
services: ''
documentationcenter: ruby
author: devinrader
manager: twilio
editor: ''
ms.assetid: 60e512f6-fa47-47c0-aedc-f19bb72a1158
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 11/25/2014
ms.author: MicrosoftHelp@twilio.com
ms.openlocfilehash: 41b5383dd319f2cb6fad4316e963f86dd7a4bc61
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036617"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Ses ve SMS özellikleri, Ruby için Twilio kullanma
Bu kılavuzda, Azure üzerinde Twilio API'si hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Telefon görüşmesi yapma ve kısa mesaj servisi (SMS) ileti gönderme senaryoları ele alınmaktadır. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz. [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses ve SMS uygulamaları oluşturmak için mevcut web dilleri ve becerilerinizi kullanmanıza olanak sağlayan bir telefon web hizmeti API'dir. Twilio, (bir Azure özelliğidir ve Microsoft Ürün değil) bir üçüncü taraf hizmetidir.

**Twilio ses** yapıp telefon çağrılarını almak, uygulamaların sağlar. **Twilio SMS** yapmak ve SMS mesajları uygulamalarınızı sağlar. **Twilio istemci** uygulamalarınızı mobil bağlantılar da dahil olmak üzere var olan Internet bağlantılarını kullanarak ses iletişimine olanak tanır.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Twilio fiyatlandırması hakkında bilgi şu adreste [Twilio fiyatlandırma][twilio_pricing]. Azure müşterileri alır bir [özel teklif][special_offer]: ücretsiz kredi 1000 metinlerinin veya 1000 dakika sayısı. Bu teklif için kaydolun veya daha fazla bilgi edinmek için lütfen [ http://ahoy.twilio.com/azure ] [ special_offer].  

## <a id="Concepts"></a>Kavramları
Twilio ses ve SMS işlevselliğini uygulamaları için sağlayan bir RESTful API API'dir. Birden fazla dilde istemci kitaplıkları vardır; bir liste için bkz. [Twilio API kitaplıkları][twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML çağrı işlemek nasıl Twilio veya SMS bildiren XML tabanlı yönergeleri kümesidir.

Örneğin, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşmaya.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Tüm TwiML belgeleriniz `<Response>` kendi kök öğesi olarak. Burada, uygulamanızın davranışını tanımlamak için Twilio fiilleri kullanın.

### <a id="Verbs"></a>TwiML fiiller
Twilio fiilleri Twilio ne bildiren XML etiketleri olan **yapmak**. Örneğin, **&lt;Say&gt;** fiil kullanımı bir çağrıda bir iletiyi teslim etmek için Twilio bildirir. 

Twilio fiillerin listesi verilmiştir.

* **&lt;Arama&gt;**: çağıran başka bir telefonu bağlanır.
* **&lt;Toplama&gt;**: telefon tuş takımında girilen sayı toplar.
* **&lt;Kapat&gt;**: bir aramasını sonlandırır.
* **&lt;Play&gt;**: bir ses dosyası çalar.
* **&lt;Duraklatma&gt;**: sessiz bir şekilde belirtilen sayıda saniye bekler.
* **&lt;Kayıt&gt;**: arayanın ses kayıtlarını ve kayıt içeren dosyanın URL'sini döndürür.
* **&lt;Yeniden yönlendirme&gt;**: farklı bir URL'de TwiML çağrısı veya SMS denetim aktarır.
* **&lt;Reddetme&gt;**: faturalama olmadan bir Twilio numaranızı gelen çağrı reddeder.
* **&lt;Söyleyin&gt;**: dönüştürür metin üzerindeki bir çağrı yapan okuma.
* **&lt;SMS&gt;**: SMS iletisi gönderir.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API'si][twilio_api].

## <a id="CreateAccount"></a>Twilio hesap oluşturma
Twilio hesap almak hazır olduğunuzda, adresinde kaydolun [deneyin Twilio][try_twilio]. Ücretsiz bir hesapla yeniden başlayın ve daha sonra hesabınızı yükseltin.

Twilio hesabınız için kaydolduğunuzda, uygulamanız için bir boş bir telefon numarası elde edersiniz. Ayrıca, bir hesap SID'si ve kimlik doğrulama belirteci alırsınız. Hem Twilio API çağrıları gerçekleştirmek için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvenli tutun. Hesap SID'si ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio hesap sayfası][twilio_account], etiketli alanları **hesap SID'si** ve **AUTH TOKEN**, sırasıyla.

### <a id="VerifyPhoneNumbers"></a>Telefon numaralarını doğrulayın
Ek olarak sayı, Twilio tarafından sunulur da kontrol edebilirsiniz, uygulamalarınızda kullanılmak sayılar, denetimin (yani, cep telefonu veya ev telefon numarası). 

Bir telefon numarası doğrulama hakkında daha fazla bilgi için bkz: [yönetme numaraları][verify_phone].

## <a id="create_app"></a>Ruby uygulaması oluşturma
Twilio hizmeti kullanan ve Azure'da çalışan bir Ruby uygulaması, Twilio hizmeti kullanan tüm diğer Ruby uygulamaları farklı değildir. Twilio Hizmetleri RESTful ve çeşitli yollarla Ruby çağrılabilir olsa da bu makalede Twilio Hizmetleri ile kullanma hakkında odaklanacaktır [Ruby için Twilio yardımcı kitaplık][twilio_ruby].

İlk olarak, [Kurulum yeni bir Azure Linux VM] [ azure_vm_setup] yeni Ruby web uygulamanız için bir konak olarak görev yapacak. Rails uygulamasının, yalnızca Kurulum VM oluşturma ile ilgili adımları yok sayın. Bir dış bağlantı noktası 80 ve 5000 dahili bir bağlantı ile bir uç nokta oluşturduğunuzdan emin olun.

Aşağıdaki örneklerde, biz kullanacaklardır [Sinatra][sinatra], Ruby için bir çok basit bir web çerçevesidir. Ancak, verilerle Twilio yardımcı kitaplık Ruby için Ruby on Rails dahil olmak üzere tüm diğer web çerçevesi ile kullanabilirsiniz.

Vm'nize yeni SSH ve yeni uygulamanız için bir dizin oluşturun. Bu dizin içinde Gemfile adlı bir dosya oluşturun ve içine aşağıdaki kodu kopyalayın:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Komut satırında çalıştırın `bundle install`. Bu, yukarıdaki bağımlılıkları yükler. Sonraki adlı bir dosya oluşturun `web.rb`. Bu, web uygulamanız için kod nerede yaşıyor olacaktır. Aşağıdaki kodu yapıştırın:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Bu noktada erişebileceğinizi komutunu çalıştırın `ruby web.rb -p 5000`. Bu dönüş 5000 numaralı bağlantı noktasında bir küçük web sunucusu yukarı. Tarayıcınızda bu uygulama için URL'yi ziyaret ederek Kurulum, Azure VM için göz olması gerekir. Web uygulamanızı tarayıcıda ulaşabilir sonra bir Twilio uygulama oluşturmaya başlamak hazır olursunuz.

## <a id="configure_app"></a>Twilio kullanmak için uygulamanızı yapılandırma
Web uygulamanızı güncelleştirerek Twilio kitaplığını kullanacak şekilde yapılandırabilirsiniz, `Gemfile` bu satırı içerecek şekilde:

    gem 'twilio-ruby'

Komut satırında çalıştırın `bundle install`. Artık `web.rb` ve üst bu çizgi de dahil olmak üzere:

    require 'twilio-ruby'

Şimdi Twilio yardımcı kitaplık için Ruby kullanarak web uygulamanızda kullanılacak işiniz tamamlandı.

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Giden bir çağrı yapmak nasıl gösterir. Temel kavramları, REST API çağrıları gerçekleştirmek için Ruby için Twilio yardımcı kitaplık kullanma ve TwiML işleme içerir. Kendi değerlerinizi yerleştirin **gelen** ve **için** telefon numaraları ve doğrulamanız olun **gelen** kodu çalıştırmadan önce Twilio hesabı için telefon numarası.

Bu işleve ekleyin `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";

    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);

      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

Açık yukarı, `http://yourdomain.cloudapp.net/make_call` bir tarayıcıda, tetikleme telefon araması yapmak için Twilio API'si çağrısı. İlk iki parametre `client.account.calls.create` okunarak da anlaşılabilir: sayı çağrıdır `from` ve sayı çağrıdır `to`. 

Üçüncü parametre (`url`), Twilio arama bağlandıktan sonra yapmanız gerekenler üzerinde yönergeleri almak için istekleri URL'dir. Bu örnekte biz Kurulum bir URL (`http://yourdomain.cloudapp.net`) kullanır ve basit bir TwiML belge döndürür `<Say>` fiili bazı metin okuma yapmak ve çağrı alma kişiye "Hello Monkey" deyin.

## <a id="howto_recieve_sms"></a>Nasıl yapılır: bir SMS mesajı alma
Önceki örnekte biz başlatılan bir **giden** telefon araması. Bu kez, Twilio sırasında bize bildiren bir telefon numarası kullanalım kaydolma işlemine bir **gelen** SMS iletisi.

İlk, oturum açma için sizin [Twilio Panosu][twilio_account]. Üst gezinti çubuğundaki "Numaralarında"'a tıklayın ve ardından, sağlanan Twilio sayısına tıklayın. Yapılandırabileceğiniz iki URL görürsünüz. Bir ses isteği URL'si belirtir ve bir SMS istek URL'si. Twilio telefon görüşmesi yapılır ya da SMS numaranızı gönderilen her çağıran URL'ler şunlardır. URL'leri de "web kancası" olarak bilinir.

İstiyoruz gelen SMS iletileri, Haydi URL'sini güncelleştirme `http://yourdomain.cloudapp.net/sms_url`. Devam edin ve sayfanın alt kısmındaki Değişiklikleri Kaydet'a tıklayın. Şimdi Geri `web.rb` şimdi uygulamamız bu durumu çözmek için program:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Değişikliği yaptıktan sonra web uygulamanızı yeniden başlatmak istediğinizden emin olun. Şimdi, telefonunuzu atın ve SMS için Twilio numaranızı gönderin. En kısa sürede "Hey, ping için teşekkür ederiz! ifadesini içeren bir SMS yanıt almanız gerekir Twilio ve Azure rock! ".

## <a id="additional_services"></a>Nasıl yapılır: ek Twilio hizmetlerini kullanma
Burada gösterilen örneklerden yanı sıra, Twilio, Azure uygulamanızı ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm Ayrıntılar için bkz. [Twilio API'si belgeleri][twilio_api_documentation].

### <a id="NextSteps"></a>Sonraki adımlar
Twilio hizmeti temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio HowTos ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts] 
* [Twilio github'da][twilio_on_github]
* [Twilio desteği için konuşma][twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: https://docs.microsoft.com/azure/virtual-machines/linux/classic/ruby-rails-web-app
