---
title: Twilio ses ve SMS (PHP) için kullanma | Microsoft Docs
description: Telefon araması yapın ve Azure'da Twilio API'si hizmeti içeren bir SMS mesaj gönderme hakkında bilgi edinin. PHP'de yazılan kod örneklerini.
documentationcenter: php
services: ''
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 007f22e3-ac75-4868-8315-da000c2e0dd0
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 27df7d306b55b7280c871d4638dc34c8fcd33acb
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903674"
---
# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Ses ve SMS özellikleri php'de için Twilio kullanma
Bu kılavuzda, Azure üzerinde Twilio API'si hizmeti ile genel programlama görevlerini gerçekleştirmek gösterilmiştir. Telefon görüşmesi yapma ve kısa mesaj servisi (SMS) ileti gönderme senaryoları ele alınmaktadır. Twilio ve ses ve SMS uygulamalarınızda kullanma hakkında daha fazla bilgi için bkz. [sonraki adımlar](#NextSteps) bölümü.

## <a id="WhatIs"></a>Twilio nedir?
Twilio ses, VoIP ve mesajlaşma uygulamaları gömmek geliştiricilerin iş iletişimleri, geleceğin güçlendiren. Bunlar, Twilio iletişimleri API platformu ile gösterme genel, bulut tabanlı bir ortamda gerekli tüm altyapı sanallaştırın. Uygulamaları oluşturmak basit ve ölçeklenebilir. Sizinle ödeme-olarak esnekliğinin fiyatlandırma gidin ve bulut güvenilirlik ' yararlanabilir.

**Twilio ses** yapıp telefon çağrılarını almak, uygulamaların sağlar. **Twilio SMS** uygulamanızın gönderin ve metin iletileri almasına olanak tanır. **Twilio istemci** WebRTC destekler ve herhangi bir telefon, tablet veya tarayıcı VoIP çağrı yapmak sağlar.

## <a id="Pricing"></a>Twilio fiyatlandırma ve özel teklifler
Azure müşterileri alır bir [özel teklif](http://www.twilio.com/azure): 10 ücretsiz Twilio Twilio hesabınızın yükselttiğinizde kredi. Twilio kredi herhangi bir Twilio kullanım (10 ABD Doları kredi 1.000 adede kadar SMS mesajları göndermek veya telefon numarası ve ileti veya çağrı hedef konumuna bağlı olarak en fazla 1000 gelen sesi dakika alma eşdeğerdir) uygulanabilir. Twilio kredi kullanma ve kullanmaya başlayın: [ http://ahoy.twilio.com/azure ](http://ahoy.twilio.com/azure).

Twilio, bir Kullandıkça Öde hizmetidir. Kurulum ücret yoktur ve hesabınızı dilediğiniz zaman kapatabilirsiniz. Daha fazla bilgi bulabilirsiniz [Twilio fiyatlandırma][twilio_pricing].

## <a id="Concepts"></a>Kavramları
Twilio ses ve SMS işlevselliğini uygulamaları için sağlayan bir RESTful API API'dir. Birden fazla dilde istemci kitaplıkları vardır; bir liste için bkz. [Twilio API kitaplıkları][twilio_libraries].

Twilio API'si önemli yönlerini Twilio fiilleri ve Twilio biçimlendirme dili (TwiML) var.

### <a id="Verbs"></a>Twilio fiiller
Twilio'yu kullanarak API yapar; fiiller Örneğin, **&lt;Say&gt;** fiil kullanımı bir çağrıda bir iletiyi teslim etmek için Twilio bildirir.

Twilio fiillerin listesi verilmiştir. Diğer fiilleri ve aracılığıyla özellikler hakkında bilgi edinin [Twilio işaretleme dili belge](http://www.twilio.com/docs/api/twiml).

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

### <a id="TwiML"></a>TwiML
TwiML çağrı işlemek nasıl Twilio veya SMS konusunda bilgilendiren Twilio fiilleri XML tabanlı yönergeleri kümesidir.

Örneğin, aşağıdaki TwiML metin dönüştürecektir **Hello World** konuşmaya.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Uygulamanız için Twilio API'sini çağırdığında, API parametrelerden biri TwiML yanıt veren URL'dir. Geliştirme amacıyla, Twilio tarafından sağlanan URL uygulamalarınız tarafından kullanılan TwiML yanıtlar sağlamak için kullanabilirsiniz. TwiML yanıtları üretmek için kendi URL'leri de barındırabilir ve başka bir seçenek kullanmaktır **TwiMLResponse** nesne.

Twilio fiilleri, öznitelikleri ve TwiML hakkında daha fazla bilgi için bkz: [TwiML][twiml]. Twilio API'si hakkında ek bilgi için bkz: [Twilio API'si][twilio_api].

## <a id="CreateAccount"></a>Twilio hesap oluşturma
Twilio hesap almak hazır olduğunuzda, adresinde kaydolun [deneyin Twilio][try_twilio]. Ücretsiz bir hesapla yeniden başlayın ve daha sonra hesabınızı yükseltin.

Twilio hesabınız için kaydolduğunuzda, bir hesap kimliği ve kimlik doğrulama belirteci alırsınız. Hem Twilio API çağrıları gerçekleştirmek için gerekli olacaktır. Hesabınıza yetkisiz erişimi önlemek için kimlik doğrulama belirtecinizi güvenli tutun. Hesap Kimliği ve kimlik doğrulama belirteci adresindeki görüntülenebilir [Twilio hesap sayfası][twilio_account], etiketli alanları **hesap SID'si** ve **AUTH TOKEN**sırasıyla.

## <a id="create_app"></a>PHP uygulaması oluşturma
Twilio hizmeti kullanan ve Azure'da çalışan bir PHP uygulaması, Twilio hizmeti kullanan tüm diğer PHP uygulamaları farklı değildir. Twilio Hizmetleri REST tabanlı ve PHP'den çeşitli yollarla çağrılabilir olsa da bu makalede Twilio Hizmetleri ile kullanma hakkında odaklanacaktır [github'dan PHP için Twilio Kitaplığı][twilio_php]. PHP için Twilio kitaplığını kullanma hakkında daha fazla bilgi için bkz. [ http://readthedocs.org/docs/twilio-php/en/latest/index.html ] [ twilio_lib_docs].

Oluşturma ve bir Twilio/PHP uygulamasını azure'a dağıtma hakkında ayrıntılı yönergeler şurada bulunabilir [nasıl bir telefon araması Twilio kullanarak azure'da bir PHP uygulaması görünebileceğini][howto_phonecall_php].

## <a id="configure_app"></a>Twilio kitaplıkları kullanmak için uygulamanızı yapılandırma
Uygulamanızı Twilio kitaplığı için PHP iki yolla kullanacak şekilde yapılandırabilirsiniz:

1. Twilio kitaplığı PHP github'dan indirin ([https://github.com/twilio/twilio-php][twilio_php]) ve ekleme **Hizmetleri** uygulamanıza dizin.
   
    -VEYA-
2. Twilio kitaplığı için PHP ARMUTLU paket olarak yükleyin. Aşağıdaki komutlarla yüklenebilir:
   
        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

PHP için Twilio Kitaplığı'nı yükledikten sonra daha sonra ekleyebilirsiniz bir **require_once** kitaplığı başvurmak için PHP dosyalarınızı üst kısmındaki deyimi:

        require_once 'Services/Twilio.php';

Daha fazla bilgi için [ https://github.com/twilio/twilio-php/blob/master/README.md ] [ twilio_github_readme].

## <a id="howto_make_call"></a>Nasıl yapılır: giden bir çağrı yapın
Aşağıdaki çağrıda giden hale getirmeyi açıklayan **Services_Twilio** sınıfı. Bu kod, Twilio tarafından sağlanan bir site ayrıca Twilio biçimlendirme dili (TwiML) yanıt için kullanır. Kendi değerlerinizi yerleştirin **gelen** ve **için** telefon numaraları ve doğrulamanız olun **gelen** kodu çalıştırmadan önce Twilio hesabı için telefon numarası.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Belirtildiği gibi bu kod bir Twilio tarafından sağlanan site TwiML yanıt döndürmek için kullanır. Bunun yerine, kendi site TwiML yanıt sağlamak için de kullanabilirsiniz; Daha fazla bilgi için [nasıl bilgisayarınızı kendi Web sitesinden sağlamak TwiML yanıtları](#howto_provide_twiml_responses).

* **Not**: SSL sertifika doğrulama hatalarını gidermek için bkz. [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 

## <a id="howto_send_sms"></a>Nasıl yapılır: bir SMS iletisi gönderin
Aşağıdakileri kullanarak bir SMS iletisi göndermek nasıl gösterir **Services_Twilio** sınıfı. **Gelen** numarası, SMS mesajları gönderebilir tarafından deneme hesapları için Twilio sağlanır. **İçin** numarası doğrulandı, kodu çalıştırmadan önce Twilio hesabınız için.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Nasıl yapılır: kendi Web sitesinden TwiML yanıtları sağlayın
Uygulamanızı Twilio API'sine çağrıda başlattığında, Twilio isteğiniz TwiML yanıt beklenen bir URL'ye gönderirsiniz. Yukarıdaki örnekte, Twilio tarafından sağlanan URL'yi kullanır [ http://twimlets.com/message ] [ twimlet_message_url]. (TwiML Twilio tarafından kullanılmak üzere tasarlandığından, BT tarayıcınızda görüntüleyebilirsiniz. Örneğin, [ http://twimlets.com/message ] [ twimlet_message_url] boş görmek için `<Response>` öğesi; başka bir örnek olarak, tıklayın [ http://twimlets.com/message?Message%5B0%5D=Hello%20World ] [ twimlet_message_url_hello_world]görmek için bir `<Response>` öğesini içeren bir `<Say>` öğesi.)

Twilio tarafından sağlanan URL üzerinde işlemine güvenmek yerine, HTTP yanıtlarını döndürür, kendi site oluşturabilirsiniz. XML yanıtlar döndüren herhangi bir dilde site oluşturabilirsiniz; Bu konuda, PHP TwiML oluşturmak için kullanacaksınız varsayılır.

Şu PHP sayfaya sonuçlarını bildiren bir TwiML yanıtına **Hello World** çağrısında.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Yukarıdaki örnekte görebileceğiniz gibi yalnızca bir XML belgesi TwiML yanıt. PHP için Twilio kitaplığı TwiML oluşturacaktır sınıflar içerir. Aşağıdaki örnekte, yukarıda gösterildiği gibi eşdeğer yanıt verir, ancak kullanır **Hizmetleri\_Twilio\_Twiml** PHP Twilio Kitaplığı'nda sınıfı:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML hakkında daha fazla bilgi için bkz: [ https://www.twilio.com/docs/api/twiml ] [ twiml_reference]. 

URL içine geçirilen olarak TwiML yanıt vermek üzere kurulan PHP sayfanızı aldıktan sonra PHP sayfasının URL'sini kullanın `Services_Twilio->account->calls->create` yöntemi. Örneğin, adlı bir Web uygulaması varsa **MyTwiML** dağıtılan Azure barındırılan hizmet ve PHP sayfanın adıdır **mytwiml.php**, URL geçirilebilir **Services_Twilio -> Hesap çağrıları -> -> oluşturma** aşağıdaki örnekte gösterildiği gibi:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
              $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Twilio azure'da PHP ile kullanma hakkında ek bilgi için bkz: [nasıl bir telefon araması Twilio kullanarak azure'da bir PHP uygulaması görünebileceğini][howto_phonecall_php].

## <a id="AdditionalServices"></a>Nasıl yapılır: ek Twilio hizmetlerini kullanma
Burada gösterilen örneklerden yanı sıra, Twilio, Azure uygulamanızı ek Twilio işlevinden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm Ayrıntılar için bkz. [Twilio API'si belgeleri][twilio_api_documentation].

## <a id="NextSteps"></a>Sonraki adımlar
Twilio hizmeti temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin:

* [Twilio güvenlik yönergeleri][twilio_security_guidelines]
* [Twilio nasıl yapılır kılavuzları ve örnek kod][twilio_howtos]
* [Twilio hızlı başlangıç öğreticileri][twilio_quickstarts] 
* [Twilio github'da][twilio_on_github]
* [Twilio desteği için konuşma][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: https://www.twilio.com/docs/api/errors
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
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
