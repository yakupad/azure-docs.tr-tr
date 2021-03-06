---
title: include dosyası
description: include dosyası
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a69a739f36854cacd6b361ca2bd17d904e9c4c96
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38753697"
---
Kaynak kilitleri, yanlışlıkla kritik kaynakları silmesini veya kuruluşunuzdaki kullanıcıların engelleyin. Rol tabanlı erişim denetiminin aksine, kaynak kilitleri tüm kullanıcılar ve roller için bir kısıtlama uygular. 

Kilit düzeyini **CanNotDelete** veya **ReadOnly** olarak ayarlayabilirsiniz. Kilit düzeyi görüntülenir Portalı'nda **Sil** ve **salt okunur** sırasıyla.

* **CanNotDelete** yetkili kullanıcılar hala okuyabilir ve bir kaynağı değiştirme, ancak bunlar bir kaynağı silemezsiniz anlamına gelir. 
* **Salt okunur** yetkili kullanıcılar, bir kaynak okuyabilir, ancak silebilir veya kaynak güncelleştirme anlamına gelir. Bu kilit uygulama tarafından verilen izinleri tüm yetkili kullanıcılara kısıtlama için benzer **okuyucu** rol. 

> [!TIP]
> Uygularken dikkat bir **salt okunur** kilit. Gibi görünen bazı işlemleri operations ek eylemler gerçekten gerekli okuyun. Örneğin, bir **salt okunur** bir depolama hesabı üzerindeki kilidi anahtarları listeleme gelen tüm kullanıcıları engeller. Yazma işlemlerini listenin döndürülen anahtarları için kullanılabilir olmadığından anahtarları işlemi bir POST isteği gerçekleştirilir. A **salt okunur** bir App Service kaynak kilidi, o etkileşime yazma erişim gerektirdiğinden kaynak dosyalarını görüntüleme Visual Studio sunucu Gezgini'nde engeller.

Bir üst kapsamda bir kilit uyguladığınızda, ilgili kapsam içindeki tüm kaynaklar aynı kilit devralır. Kaynaklar daha sonra eklediğiniz kilit üst öğeden devralır. Devralmada en kısıtlayıcı kilit önceliklidir.

Resource Manager kilitleri uygulamak gönderilen operations oluşan yönetim düzlemi gerçekleşen işlemlere `https://management.azure.com`. Kilit, kendi işlevleri nasıl işlem kaynakları kısıtlama. Kaynak değişiklikleri kısıtlıdır, ancak kaynak işlemleri sınırlı değildir. Örneğin, bir salt okunur kilidi SQL veritabanı, veritabanı silmesini veya engeller. Oluşturma, güncelleştirme ya da veritabanı verilerini silme engellemez. Bu işlemler için gönderilmediği için veri işlem izin verilen `https://management.azure.com`.

### <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kimler oluşturabilir ve kuruluşunuzdaki kilitlerini Sil
Yönetim kilitlerini oluşturmak veya silmek için `Microsoft.Authorization/locks/*` eylemlerine erişiminiz olması gerekmektedir. Yerleşik rollerden yalnızca **Sahip** ve **Kullanııcı Erişiimi Yöneticisi** bu eylemleri kullanabilir.
