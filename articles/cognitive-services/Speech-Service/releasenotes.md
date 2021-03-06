---
title: Bilişsel hizmetler konuşma SDK Belgeleri
description: Sürüm Notları - en son sürümlerde nelerin değiştiğini
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: wolfma
ms.openlocfilehash: a28df7faaf98e5cba8ad4afe7aa1a829195d7828
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281499"
---
# <a name="release-notes"></a>Sürüm notları

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Bilişsel hizmetler konuşma SDK buradan 0.5.0 sürümünü: Temmuz 2018 sürüm

**Yeni Özellikler**

* Android platform desteği (API 23: Android 6.0 Marshmallow veya üzeri).
  Kullanıma [Android Hızlı Başlangıç](quickstart-java-android.md).
* Windows üzerinde .NET Standard 2.0 desteği.
  Kullanıma [.NET Core hızlı](quickstart-csharp-dotnetcore-windows.md).
* Deneysel: Destek UWP üzerinde Windows (sürüm 1709 veya üzeri)
  * Kullanıma sunduğumuz [UWP hızlı](quickstart-csharp-uwp.md).
  * Not: Windows uygulama sertifikası Kiti (WACK) Speech SDK'sı ile oluşturulan UWP uygulamaları henüz geçmeyin.
* Otomatik yeniden bağlanma ile uzun süreli tanıma destekler.

**İşlevsel değişiklikler**

* `StartContinuousRecognitionAsync()` uzun tanıma çalıştırmayı destekler
* Tanıma işleminin sonucu daha fazla alan içeriyor: ses başına ve süresi (hem de saat döngüsü) tanınan metin tanıma durumu, örn, temsil eden ek değerler uzaklığı `InitialSilenceTimeout`, `InitialBabbleTimeout`.
* AuthorizationToken factory örnekleri oluşturmak için destek.

**Bozucu değişiklikler**

* Tanıma olayları: NoMatch olay türü, hata olayı birleştirilir.
* C# SpeechOutputFormat OutputFormat C++ ile hizalanmış tutmak için yeniden adlandırılır.
* Bazı yöntemleri dönüş türünü `AudioInputStream` biraz değiştirilmiş bir arabirimi:
   * Java'da,. `read` yöntemi şimdi döndürür `long` yerine `int`.
   * C# ' ta, `Read` yöntemi şimdi döndürür `uint` yerine `int`.
   * C++ ' ta `Read` ve `GetFormat` yöntemleri artık dönüş `size_t` yerine `int`.
* C++: ses giriş akışları örneklerini artık yalnızca olarak geçirilebilir bir `shared_ptr`.

**Hata düzeltmeleri**

* Sonuçta hatalı dönüş değerleri sabit olduğunda `RecognizeAsync()` zaman aşımına uğrar.
* Windows media foundation kitaplıkları bağımlılığı kaldırılır. SDK artık çekirdek ses API'leri kullanıyor.
* Belge düzeltmesi: desteklenen bölgeleri nelerdir açıklamak için bir bölge sayfası eklendi.

**Bilinen sorunlar**

* Android için Speech SDK'sı konuşma sentezi sonuçları çeviri raporlamaz.
  Bu, sonraki sürümde düzeltilecektir.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel hizmetler konuşma SDK 0.4.0: Haziran 2018'den sürüm

**İşlevsel değişiklikler**

- AudioInputStream

  Bir tanıyıcı, artık bir akışı ses kaynağı olarak tüketebilir. İlgili ayrıntılı bilgi için bkz: [yapılır kılavuzunda](how-to-use-audio-input-streams.md).

- Ayrıntılı çıkış biçimi

  Oluşturulurken bir `SpeechRecognizer`, talep edebilir `Detailed` veya `Simple` çıkış biçimi. `DetailedSpeechRecognitionResult` Maskelenmiş küfür ile bir güven puanı, tanınan metin, ham bir sözcük formu, normalleştirilmiş form ve normalleştirilmiş form içerir.

**Yeni değişiklik**

- Değiştirin `SpeechRecognitionResult.Text` gelen `SpeechRecognitionResult.RecognizedText` C#.

**Hata düzeltmeleri**

- Olası bir geri çağırma sorunu USP katmanda kapatılırken düzeltin.

- Ses giriş dosyası bir tanıyıcı kullanılan, onu için gerekenden daha uzun dosya tanıtıcısı bulunduran.

- İleti pompası tanıyıcı arasındaki birkaç kilitlenmeleri kaldırıldı.

- Ateş bir `NoMatch` hizmetinden gelen yanıt zaman aşımına neden.

- Gecikmeli yüklenen Windows media foundation kitaplıkları. Bu kitaplığı yalnızca, mikrofon giriş için gerekli.

- Karşıya yükleme hızı için ses verilerini iki kez özgün ses hızı hakkında sınırlıdır.

- Windows üzerinde C# .NET derlemeleri artık güçlü-olarak adlandırılır.

- Belge düzeltmesi: `Region` bir tanıyıcı oluşturmak için gerekli bir bilgidir.

Daha fazla örnek eklenmiştir ve sürekli olarak güncelleştiriliyor. En son örnekleri için bkz [konuşma SDK örnek GitHub deposunda](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Bilişsel hizmetler konuşma SDK 0.2.12733: Mayıs 2018 sürüm

Bilişsel hizmetler konuşma SDK'sının ilk genel Önizleme sürümü.
