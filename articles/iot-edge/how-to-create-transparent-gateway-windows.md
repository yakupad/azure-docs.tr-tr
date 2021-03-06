---
title: Azure IOT Edge - Windows ile saydam bir ağ geçidi oluşturma | Microsoft Docs
description: Birden çok cihaza yönelik bilgi işleme saydam bir ağ geçidi oluşturmak için Azure IOT Edge'i kullanma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5ffb1b5c9889e2325eab32306b61899b37d22488
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187477"
---
# <a name="create-a-windows-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Saydam bir ağ geçidi olarak davranır bir Windows IOT Edge cihazı oluşturma

Bu makalede, saydam bir ağ geçidi olarak IOT Edge cihazı kullanmaya yönelik ayrıntılı yönergeler sağlar. Bu makalenin geri kalanında terimi *IOT Edge ağ geçidi* saydam bir ağ geçidi olarak kullanılan bir IOT Edge cihazı gösterir. Daha ayrıntılı bilgi için bkz. [nasıl bir IOT Edge cihazı ağ geçidi olarak kullanılabilir][lnk-edge-as-gateway], kavramsal bir genel bakış sağlar.

>[!NOTE]
>Şu anda:
> * Ağ geçidi, IOT Hub'ından kesilirse, aşağı akış cihazların ağ geçidi ile kimlik doğrulaması yapamaz.
> * Edge özellikli cihazlar IOT Edge ağ geçidi için bağlantı kurulamıyor. 
> * Aşağı Akış cihazları karşıya dosya yükleme kullanamazsınız.

Saydam bir ağ geçidi oluşturma hakkında daha fazla sabit bölümü güvenli bir aşağı akış cihazları ağ geçidine bağlanıyor. Azure IOT Edge bu cihazları arasında güvenli TLS bağlantıları kurmak için PKI altyapısını kullanmanıza olanak tanır. Bu durumda, biz saydam bir ağ geçidi olarak görev yapan bir IOT Edge cihazına bağlamak için bir aşağı akış cihazı vermiş olursunuz.  Makul güvenliğini sağlamak için aşağı akış cihaz, ağ geçitleri ve değil kötü amaçlı olabilecek bir ağ geçidi'ne bağlama cihazlarınızı yalnızca istediğinden sınır cihazı kimliğini onaylamalıdır.

Ağ geçidi cihazı topolojiniz için gerekli güven sağlayan herhangi bir sertifika altyapısı oluşturabilirsiniz. Bu makalede, etkinleştirmek için kullanacağınız aynı sertifika Kurulumu varsayıyoruz [X.509 CA güvenlik] [ lnk-iothub-x509] IOT Hub'ında, belirli IOT hub'a (IOT hub'ı sahibi CA ilişkili bir X.509 CA sertifikası kapsamaktadır ) ve bu CA ve bir CA için sınır cihazı imzalanmış sertifikalar, bir dizi.

![Ağ geçidi][1]

Ağ geçidi bağlantı başlatma sırasında aşağı akış cihaza Edge cihaz CA sertifikasını sunar. Edge cihaz CA sertifika sahibi CA sertifikası tarafından imzalanmış emin olmak için aşağı akış cihaz denetler. Bu işlem, ağ geçidi güvenilir bir kaynaktan gelen onaylamak aşağı akış cihaz sağlar.

Aşağıdaki adımlarda sertifikaları oluşturma ve bunları doğru yerlerde yükleme işleminde size yol.

## <a name="prerequisites"></a>Önkoşullar
1.  [Azure IOT Edge çalışma zamanı yükleme] [ lnk-install-windows-x64] saydam bir ağ geçidi olarak kullanmak istediğiniz bir Windows cihazda.

1. Windows için OpenSSL alın. OpenSSL yükleyebilmek için birçok yolu vardır:

   >[!NOTE]
   >Windows Cihazınızda yüklü OpenSSL zaten varsa, bu adımı atlayın ancak lütfen emin olun `openssl.exe` kullanılabilir, `%PATH%` ortam değişkeni.

   * indirin ve yükleyin [üçüncü taraf OpenSSL ikili](https://wiki.openssl.org/index.php/Binaries), örneğin, gelen [SourceForge bu projede](https://sourceforge.net/projects/openssl/).
   
   * OpenSSL kaynak kodunu indirebilir ve ikili dosyaları makinenizde kendiniz oluşturmak ya da bunu aracılığıyla [vcpkg](https://github.com/Microsoft/vcpkg). Aşağıda listelenen yönergeleri vcpkg kaynak kodunu indirebilir, derlemek ve tüm kullanımı çok kolay adımda Windows makinenizde OpenSSL yüklemek için kullanın.

      1. Vcpkg yüklemek istediğiniz bir dizine gidin. Üzerinde buradan için $VCPKGDIR diyoruz. İndirme ve yükleme yönergelerini [vcpkg](https://github.com/Microsoft/vcpkg).
   
      1. Windows için x64 OpenSSL paketi yüklemek için aşağıdaki komutu vcpkg yüklendiğinde bir powershell isteminden çalıştırın. Bu, genellikle tamamlanması yaklaşık 5 dakika sürer.

         ```PowerShell
         .\vcpkg install openssl:x64-windows
         ```
      1. Ekleme `$VCPKGDIR\installed\x64-windows\tools\openssl` için `PATH` ortam değişkeni böylece `openssl.exe` dosya çağırma için kullanılabilir.

1. Çalışmak istediğiniz dizine gidin. Üzerinde buradan için $WRKDIR diyoruz.  Bu dizindeki tüm dosyaları oluşturulur.
   
   CD $WRKDIR

1.  Aşağıdaki komutla gerekli üretim dışı sertifikalarını oluşturmak için komut dosyalarını alın. Bu betikler, saydam bir ağ geçidini ayarlamak için gerekli sertifikaları oluşturmanıza yardımcı olur.

      ```PowerShell
      git clone https://github.com/Azure/azure-iot-sdk-c.git
      ```

1. Yapılandırma ve komut dosyaları, çalışma dizinine kopyalayın. Ayrıca, env değişkeni OPENSSL_CONF openssl_root_ca.cnf yapılandırma dosyası kullanmak için ayarlayın.

   ```PowerShell
   copy azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
   ```

1. Aşağıdaki komutu çalıştırarak betikleri çalıştırmak PowerShell'i etkinleştirme

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted
   ```

1. PowerShell'in genel ad alanında nokta kaynak belirleme aşağıdaki komutla tarafından betikler tarafından kullanılan işlevler, Getir
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

1. OpenSSL doğru şekilde yüklendiğini doğrulayın ve aşağıdaki komutu çalıştırarak var olan sertifikalar ile ad çakışmalarını olmayacak emin olun. Sorun varsa, betik bu sisteminize nasıl açıklamalıdır.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

## <a name="certificate-creation"></a>Sertifika oluşturma
1.  Sahibi CA sertifikası ve bir ara sertifika oluşturun. Bu tüm yerleştirilir `$WRKDIR`.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

1.  Aşağıdaki komutla, Edge cihaz CA sertifikasını ve özel anahtar oluşturun.

   >[!NOTE]
   > **DO NOT** ağ geçidinin DNS ana bilgisayar adıyla aynı adı kullanın. Bunun yapılması, bu sertifikaların başarısız olmasına karşı istemci sertifikası neden olur.

   ```PowerShell
   New-CACertsEdgeDevice "<gateway device name>"
   ```

## <a name="certificate-chain-creation"></a>Sertifika zinciri oluşturma
Sahibi CA sertifikası ara sertifika ve aşağıdaki komutla Edge cihaz CA sertifikasını bir sertifika zinciri oluşturun. Zincir dosyasında yerleştirme, kolayca saydam bir ağ geçidi olarak görev yapan Edge Cihazınızda yüklemenize olanak tanır.

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway device name>"
   ```

   Betik yürütme çıktısı, aşağıdaki sertifika ve anahtar şunlardır:
   * `$WRKDIR\certs\new-edge-device.*`
   * `$WRKDIR\private\new-edge-device.key.pem`
   * `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="installation-on-the-gateway"></a>Ağ geçidi yükleme
1.  Aşağıdaki dosyalar $WRKDIR Edge Cihazınızda herhangi bir yere kopyalayın, biz için $CERTDIR başvuracağınız. Edge Cihazınızda sertifikaları oluşturduysanız bu adımı atlayın.

   * Cihaz CA sertifikası-  `$WRKDIR\certs\new-edge-device-full-chain.cert.pem`
   * Cihaz CA özel anahtarı- `$WRKDIR\private\new-edge-device.key.pem`
   * Sahibi CA- `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

2.  Ayarlama `certificate` özellikleri sertifika ve anahtar dosyaları yerleştirdiğiniz güvenlik Daemon yapılandırma dosyasında yaml yolu.

```yaml
certificates:
  device_ca_cert: "$CERTDIR\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR\\certs\\azure-iot-test-only.root.ca.cert.pem"
```
## <a name="deploy-edgehub-to-the-gateway"></a>Ağ geçidine EdgeHub dağıtma
Azure IoT Edge'in önemli özelliklerinden biri buluttan IoT Edge cihazlarınıza modüller dağıtabilmektir. Bu bölümde, görünüşte boş bir dağıtımını oluşturun bulunur; Edge hub'ı ancak hiçbir diğer modüller mevcut olsa bile tüm dağıtımlara eklenen automatcially olur. Edge hub'ı saydam bir ağ geçidi olarak boş bir dağıtım oluşturma yeterli, bu nedenle işlem için bir Edge cihazında ihtiyacınız yalnızca modülüdür. 
1. Azure portalında IoT Hub'ınıza gidin.
2. Git **IOT Edge** ve ağ geçidi olarak kullanacağınız IOT Edge Cihazınızı seçin.
3. **Modülleri Ayarlama**'yı seçin.
4. **İleri**’yi seçin.
5. **Rotaları belirtin** adımında tüm modüllerden gelen tüm iletileri IoT Hub'a gönderen varsayılan rotaya sahip olmanız gerekir. Yoksa aşağıdaki kodu ekleyip **İleri**'yi seçin.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. Gözden geçirme şablon adımda seçin **Gönder**.

## <a name="installation-on-the-downstream-device"></a>Aşağı Akış cihaza yükleme
Bir aşağı akış cihaz herhangi bir uygulama olabilir kullanarak [Azure IOT cihaz SDK'sını][lnk-devicesdk]basit bir açıklandığı gibi [.NET kullanarak IOT hub'ınıza Cihazınızı bağlama] [ lnk-iothub-getstarted]. Güven bir aşağı akış cihaz uygulaması olan **sahibi CA** ağ geçidi cihazları TLS bağlantılarını doğrulamak için sertifika. Bu adım genellikle iki şekilde gerçekleştirilebilir: işletim sistemi düzeyinde ya da (için belirli bir dil) uygulama düzeyinde.

### <a name="os-level"></a>İşletim sistemi düzeyi
Bu sertifika işletim sistemi sertifika deposunda yükleme sahibi kullanmak için tüm uygulamaları CA sertifikası güvenilen bir sertifika izin verir.

* Ubuntu - bir Ubuntu konakta bir CA sertifikası yüklemek nasıl bir örnek aşağıda verilmiştir.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    "Güncelleştirme sertifikaları /etc/ssl/certs... belirten bir ileti görürsünüz. 1, 0 kaldırıldı eklendi; bitti."

* Windows - bir Windows konağında bir CA sertifikası yüklemek nasıl bir örnek aşağıda verilmiştir.
  * Başlat menüsünde "Bilgisayar sertifikalarını Yönet" türüne. Bu adlı bir yardımcı program getirmelisiniz `certlm`.
  * Gidin sertifikaların yerel bilgisayar güvenilen kök sertifikalar-->--> sertifika--> sağ tıklatın, tüm görevler-->--> Sertifika Alma Sihirbazı'nı başlatmak için içeri aktarma.
  * Yönergelerine uygun olarak aşağıdaki adımları uygulayın ve sertifika dosyası $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem içeri aktarın.
  * İşlem tamamlandığında bir "Başarıyla içeri aktarıldı" iletisini görmeniz gerekir.

### <a name="application-level"></a>Uygulama düzeyi
.NET uygulamaları için aşağıdaki kod parçacığını PEM biçiminde bir sertifika güven ekleyebilirsiniz. Değişkeni başlatmak `certPath` ile `$CERTDIR\certs\azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>Aşağı Akış cihaz ağ geçidine bağlanma
Ağ geçidi cihazı ana bilgisayar adına başvuran bir bağlantı dizesi ile IOT Hub cihaz SDK'sı başlatmanız gerekir. Bu ekleyerek yapılır `GatewayHostName` özelliği, cihaz bağlantı dizesi. Örneğin, eklenen biz, bir cihaz için bir örnek cihaz bağlantı dizesi işte `GatewayHostName` özelliği:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Her şeyi olmuştur hangi testlerin ayarlanmış bir örnek komut doğru budur. Sohuld "Tamam doğrulandı." iletisini.
   >
   >openssl s_client-mygateway.contoso.com:8883 - CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts bağlanma

## <a name="routing-messages-from-downstream-devices"></a>Aşağı Akış cihazlardan yönlendirme iletileri
IOT Edge çalışma zamanı yalnızca modülleri tarafından gönderilen iletiler gibi aşağı akış cihazlardan gönderilen iletiler yönlendirebilirsiniz. Bu verileri buluta göndermeden önce ağ geçidi üzerinde çalışan bir modüldeki analiz gerçekleştirmenize olanak sağlar. Rota adlı bir aşağı akış CİHAZDAN ileti göndermek için kullanılacak `sensor` bir modül adı için `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Başvurmak [modülü oluşturma makale] [ lnk-module-composition] ileti yönlendirme hakkında daha fazla bilgi.

## <a name="next-steps"></a>Sonraki adımlar
[IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlamak][lnk-module-dev].

<!-- Images -->
[1]: ./media/how-to-create-transparent-gateway/gateway-setup.png

<!-- Links -->
[lnk-install-windows-x64]: ./how-to-install-iot-edge-windows-with-windows.md
[lnk-module-composition]: ./module-composition.md
[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/quickstart-send-telemetry-dotnet.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus
