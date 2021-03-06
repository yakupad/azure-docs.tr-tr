---
title: Azure Notification Hubs kullanarak iOS uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
documentationcenter: ios
keywords: anında iletme bildirimi,anında iletme bildirimleri,ios anında iletme bildirimleri
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 083b0c956055ab5b54a4af2eec57f096613cbe65
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38681528"
---
# <a name="tutorial-push-notifications-to-ios-apps-using-azure-notification-hubs"></a>Öğretici: Azure Notification Hubs kullanarak iOS uygulamalarına anında iletme bildirimleri gönderme
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğreticide, bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanırsınız. [Apple Anında İletilen Bildirim servisini (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturursunuz. 

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sertifika imzalama isteği dosyası oluşturma
> * Anında iletme bildirimleri için uygulamanızı isteme
> * Uygulama için bir sağlama profili oluşturun
> * iOS anında iletme bildirimleri için bildirim hub’ınızı yapılandırma
> * iOS uygulamanızı bildirim hub’larına bağlama
> * Test amaçlı anında iletme bildirimleri gönderme
> * Uygulamanızın bildirim aldığını doğrulama

Bu öğreticinin tamamlanan kodu [GitHub'da](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted) bulunabilir. 

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde [ücretsiz bir deneme hesabı](https://azure.microsoft.com/free) oluşturabilirsiniz. 
- [Windows Azure Messaging Framework]
- [Xcode]'un en son sürümü
- iOS 10 (veya sonraki bir sürümü) uyumlu bir cihaz
- [Apple Developer Program](https://developer.apple.com/programs/) üyeliği.
  
  > [!NOTE]
  > Anında iletme bildirimlerinin yapılandırma gereksinimleri nedeniyle, anında iletme bildirimlerini iOS Simülatörü'nün yerine fiziksel bir iOS cihazında (iPhone veya iPad) dağıtmanız ve test etmeniz gerekir.
  
Bu öğreticiyi tamamlamak iOS uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>iOS anında iletme bildirimleri için Notification Hub'ınızı yapılandırma
Bu bölümde, bir bildirim hub’ı oluşturur ve önceden oluşturduğunuz **.p12** anında iletme sertifikasını kullanarak APNS ile kimlik doğrulamasını yapılandırırsınız. Önceden oluşturduğunuz bir bildirim hub'ını kullanmak istiyorsanız 5. adıma geçebilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-your-notification-hub-with-apns-information"></a>APNS bilgileriyle bildirim hub’ınızı yapılandırma

1. **Bildirim Hizmetleri** bölümünde **Apple (APNS)** seçeneğini belirleyin. 
2. **Sertifika**’yı seçin.
3. **Dosya simgesini** seçin.
4. Daha önce dışarı aktardığınız **.p12** dosyasını seçin.
5. Doğru **parolayı** belirtin.
6. **Korumalı alan** modunu seçin. **Üretim** seçeneğini yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız kullanın.

    ![Azure portalında APNS sertifikası yapılandırma][7]

Bildirim hub'ınızı APNS ile birlikte çalışacak şekilde yapılandırdınız. Ayrıca uygulamanızı kaydetmenizi ve anlık iletme bildirimleri göndermenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="connect-your-ios-app-to-notification-hubs"></a>iOS uygulamanızı Notification Hubs'a bağlama
1. Xcode'da yeni bir iOS projesi oluşturun ve **Single View Application** (Tek Görünüm Uygulaması) şablonunu seçin.
   
    ![Xcode - Single View Application (Tek Görünüm Uygulaması)][8]
    
2. Yeni projeniz için seçenekleri ayarlarken, Apple Developer portalında paket kimliğini açarken kullandığınız **Product Name** (Ürün Adı) ve **Organization Identifier**'nı (Kuruluş Tanımlayıcısı) kullandığınızdan emin olun.
   
    ![Xcode - proje seçenekleri][11]
    
3. Project Navigator (Proje Gezgini) bölümünde projenizin adına ve ardından **General** (Genel) sekmesine tıklayıp **Signing** (İmzalama) seçeneğini bulun. Apple Developer hesabınız için uygun Team girişini seçtiğinizden emin olun. XCode, paket tanımlayıcınızı temel alarak önceden oluşturduğunuz Sağlama Profilini otomatik olarak aşağı çekmelidir.
   
    Xcode'da oluşturduğunuz yeni hazırlama profilini göremiyorsanız imzalama kimliğiniz için profilleri yenilemeyi deneyin. Menü çubuğunda **Xcode**'a tıklayın, **Preferences**'a (Tercihler) tıklayın, **Account** (Hesap) sekmesine tıklayın, **View Details** (Ayrıntıları Görüntüle) düğmesine tıklayın, imzalama kimliğinize tıklayın ve ardından sağ alt köşedeki yenile düğmesine tıklayın.

    ![Xcode - hazırlama profili][9]

4. **Capabilities** (Yetenekler) sekmesini seçin ve Push Notifications (Anında İletme Bildirimleri) seçeneğini etkinleştirmeyi unutmayın

    ![Xcode - anında iletme bildirimleri][12]
   
5. [Windows Azure Messaging Framework] paketini indirip açın. Xcode'da projenize sağ tıklayın ve **WindowsAzureMessaging.framework** klasörünü Xcode projenize eklemek için **Add Files to** (Dosyaları Şuraya Ekle) seçeneğine tıklayın. **Options** (Seçenekler) seçeneğine tıklayıp **Copy items if needed** (Gerekirse verileri kopyala) öğesinin seçili olduğundan emin olduktan sonra **Add** (Ekle) öğesine tıklayın.

    ![Azure SDK'nın sıkıştırmasını açma][10]

6. Projenize **HubInfo.h** adlı yeni bir üst bilgi dosyası ekleyin. Bu dosya, bildirim hub’ınız için sabitleri tutar. Aşağıdaki tanımları ekleyin ve dize sabiti yer tutucularını, daha önce not ettiğiniz *hub adınız* ve *DefaultListenSharedAccessSignature* ile değiştirin.

    ```obj-c
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
    ```
    
7. **AppDelegate.h** dosyanızı açıp aşağıdaki içeri aktarma yönergelerini ekleyin:

    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
        #import <UserNotifications/UserNotifications.h> 
        #import "HubInfo.h"
    ```
8. **AppDelegate.m file** dosyanızın **didFinishLaunchingWithOptions** yöntemine iOS sürümünüze bağlı olarak aşağıdaki kodu ekleyin. Bu kod, cihaz tanıtıcınızı APNs'ye kaydeder:

    ```obj-c
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
            UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```
   
9. Aynı dosyada, aşağıdaki yöntemleri ekleyin:

    ```obj-c
         - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
           SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
               if (error != nil) {
                   NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                   [self MessageBox:@"Registration Status" message:@"Registered"];
              }
          }];
         }
   
        -(void)MessageBox:(NSString *) title message:(NSString *)messageText
        {
         UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
    ```

    Bu kod, HubInfo.h içinde belirttiğiniz bağlantı bilgilerini kullanarak bildirim hub'ına bağlanır. Ardından, cihaz belirtecini bildirim hub'ına verir. Böylece bildirim hub'ı bildirim gönderebilir.

10. Aynı dosyaya, uygulama etkinken bildirim alınırsa **UIAlert** görüntülenmesi için aşağıdaki yöntemi ekleyin:

    ```obj-c
            - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
               NSLog(@"%@", userInfo);
               [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
           }
    ```

11. Herhangi bir hata olmadığını doğrulamak için cihazınızda uygulamayı derleyin ve çalıştırın.

## <a name="send-test-push-notifications"></a>Test amaçlı anında iletme bildirimleri gönderme
[Azure portalındaki] *Test Gönderimi* seçeneğini kullanarak uygulamanızda bildirim alma testi gerçekleştirebilirsiniz. Bu, cihazınıza test amaçlı anında iletme bildirimi gönderir.

![Azure portalı - Test Gönderimi][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="verify-that-your-app-receives-push-notifications"></a>Uygulamanızın anında iletme bildirimleri aldığını doğrulama
iOS'ta anında iletme bildirimlerini test etmek için, uygulamayı fiziksel bir iOS cihazına dağıtmanız gerekir. iOS Simülatörü'nü kullanarak Apple anında iletme bildirimleri gönderemezsiniz.

1. Uygulamayı çalıştırın ve kaydın başarılı olduğunu doğrulayın. Ardından, **Tamam**'a basın.
   
    ![iOS Uygulaması Anında İletme Bildirimi Kayıt Testi][33]
2. Daha sonra, önceki bölümde açıklandığı gibi, [Azure portalındaki] test amaçlı anında iletme bildirimi gönderirsiniz. 

3. Belirli Bildirim Hub'ından bildirimleri almak için kaydedilen tüm cihazlara anında iletme bildirimi gönderilir.
   
    ![iOS Uygulaması Anında İletilen Bildirim Alma Testi][35]

## <a name="next-steps"></a>Sonraki adımlar
Bu basit örnekte, tüm kayıtlı iOS cihazlarınıza anında iletme bildirimleri yayımladınız. Belirli iOS cihazlarına nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-ios-xplat-segmented-apns-push-notification.md)


<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Notify Users for iOS with .NET backend]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure portalındaki]: https://portal.azure.com
