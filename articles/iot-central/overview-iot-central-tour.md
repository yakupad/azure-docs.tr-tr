---
title: Azure IoT Central Kullanıcı Arabirimi turuna katılın | Microsoft Docs
description: Oluşturucu olarak, bir IoT çözümü oluşturmak için kullanabileceğiniz Azure IoT Central kullanıcı arabiriminin temel alanlarını tanıyın.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/13/2018
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 69898358026eab716c057f339d8594df43db136f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629380"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Azure IoT Central Kullanıcı Arabirimi turuna katılın

Bu makalede, Microsoft Azure IoT Central kullanıcı arabirimi tanıtılmaktadır. Kullanıcı arabirimini kullanarak bir Azure IoT Central çözümü ile bağlı cihazlarını oluşturabilir, yönetebilir ve kullanabilirsiniz.

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü tanımlayabilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

- Çözümünüze bağlanan cihaz türlerini tanımlama.
- Cihazlarınız için kurallar ve eylemler yapılandırma.
- Kullanıcı arabirimini, çözümünüzü kullanan bir _operatör_ için özelleştirme.

_Operatör_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Azure IoT Central çözümünüzü yönetebilirsiniz. Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

- Cihazlarınızı izleme.
- Cihazlarınızı yapılandırma.
- Cihazlarınızla ilgili sorunları giderme ve düzeltme.
- Yeni cihazları hazırlama.

## <a name="use-the-left-navigation-menu"></a>Sol gezinti menüsünü kullanma

Uygulamanın farklı alanlarına erişmek için sol gezinti menüsünü kullanın:

| Menü | Açıklama |
| ---- | ----------- |
| ![Sol gezinti menüsü](media/overview-iot-central-tour/navigationbar.png) | <ul><li>**Giriş** düğmesi, uygulamanızın giriş sayfasını gösterir. Oluşturucu olarak, bu giriş sayfasını operatörleriniz için özelleştirebilirsiniz.</li><li>**Device Explorer** düğmesi hem uygulamanızda tanımlanmış cihaz şablonlarını hem de her bir cihaz şablonuyla ilişkili sanal ve gerçek cihazları listeler. Operatör olarak, **Device Explorer**’ı kullanarak bağlı cihazlarınızı yönetebilirsiniz.</li><li>**Cihaz Kümeleri** düğmesi, cihaz kümelerini görüntüleyip oluşturmanızı sağlar. Operatör olarak, bir sorgu tarafından belirtilen cihazların mantıksal bir koleksiyonu olarak cihaz kümeleri oluşturabilirsiniz.</li><li>**Analiz** düğmesi, cihazlar ve cihaz kümeleri için cihaz telemetrisinden türetilen analizleri gösterir. Operatör olarak, uygulamanızdan içgörüler türetmek için cihaz verileri üzerinde özel görünümler oluşturabilirsiniz.</li><li>**Uygulama Oluşturucu** düğmesi, bir oluşturucunun kullandığı **Cihaz Şablonu Oluşturma** aracı gibi araçları gösterir.</li><li>**Yönetim** düğmesi, bir yöneticinin uygulama ayarlarını, kullanıcıları ve rolleri yönetebildiği uygulama yönetimi sayfalarını gösterir.</li></ul> |

## <a name="search-help-and-support"></a>Arama, yardım ve destek

Üstteki menü her sayfada görünür:

![Araç Çubuğu](media/overview-iot-central-tour/toolbar.png)

- Cihaz şablonlarını ve cihazları aramak için **Arama** simgesini seçin.
- Yardım ve destek almak için, kaynak listesinden **Yardım** açılır listesini seçin.
- Öğreticileri denetlemek için kullanıcı arabirimi temasını değiştirin veya uygulama oturumunu kapatıp **Hesap** simgesini seçin.

Kullanıcı arabirimi için açık renkli tema veya koyu renkli temayı seçebilirsiniz:

![Kullanıcı arabirimi için tema seçme](media/overview-iot-central-tour/themes.png)

## <a name="home-page"></a>Giriş sayfası

![Giriş sayfası](media/overview-iot-central-tour/homepage.png)

Giriş sayfası, Azure IoT Central uygulamanızda oturum açtığınızda gördüğünüz birinci sayfadır. Oluşturucu olarak, uygulamaya kutucuklar ekleyerek uygulamanın diğer kullanıcıları için giriş sayfasını özelleştirebilirsiniz. Daha fazla bilgi almak için [Azure IoT Central operatör görünümünü özelleştirme](tutorial-customize-operator.md) öğreticisine bakın.

## <a name="device-explorer"></a>Device explorer

![Gezgin sayfası](media/overview-iot-central-tour/explorer.png)

Gezgin sayfası, Azure IoT Central uygulamanızdaki _cihaz şablonlarını_ ve _cihazları_ gösterir.

* Cihaz şablonu, uygulamanıza bağlanabilen bir cihaz türünü tanımlar. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md).
* Cihaz, uygulamanızdaki gerçek veya sanal bir cihazı temsil eder. Daha fazla bilgi almak için bkz. [Azure IoT Central uygulamanıza yeni bir cihaz ekleme](tutorial-add-device.md).

## <a name="device-sets"></a>Cihaz kümeleri

![Cihaz Kümeleri sayfası](media/overview-iot-central-tour/devicesets.png)

_Cihaz kümeleri_ sayfası, oluşturucu tarafından oluşturulan cihaz kümelerini gösterir. Cihaz kümesi, ilişkili cihazların bir koleksiyonudur. Oluşturucu, bir cihaz kümesine eklenen cihazları belirlemek üzere bir sorgu tanımlar. Uygulamanızdaki analizleri özelleştirirken cihaz kümelerini kullanırsınız. Daha fazla bilgi almak için [Azure IoT Central uygulamanızda cihaz kümelerini kullanma](howto-use-device-sets.md) makalesine bakın.

## <a name="analytics"></a>Analiz

![Analiz sayfası](media/overview-iot-central-tour/analytics.png)

Analiz sayfasında, uygulamanıza bağlı cihazların nasıl davrandığını anlamanıza yardımcı olan grafikler gösterilir. Operatör bu sayfayı kullanarak bağlı cihazlarla ilgili sorunları izler ve araştırır. Oluşturucu, bu sayfada gösterilen grafikleri tanımlayabilir. Daha fazla bilgi almak için [Azure IoT Central uygulamanız için özel analizler oluşturma](howto-create-analytics.md) makalesine bakın.

## <a name="application-builder"></a>Uygulama Oluşturucu

![Uygulama Oluşturucu sayfası](media/overview-iot-central-tour/applicationbuilder.png)

Uygulama oluşturucu sayfası, cihaz şablonları oluşturma ve giriş sayfasını yapılandırma gibi bir oluşturucunun Azure IoT Central uygulaması oluşturmak için kullandığı araçların bağlantılarını içerir. Daha fazla bilgi almak için [Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama](tutorial-define-device-type.md) öğreticisine bakın.

## <a name="administration"></a>Yönetim

![Yönetim sayfası](media/overview-iot-central-tour/administration.png)

Yönetim sayfası, uygulamadaki kullanıcıları ve rolleri tanımlama gibi bir yöneticinin kullandığı araçların bağlantılarını içerir. Daha fazla bilgi almak için [Azure IoT Central uygulamanızı yönetme](howto-administer.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize ve kullanıcı arabiriminin düzenini tanıdığınıza göre önerilen sıradaki adım, [Azure IoT Central uygulaması oluşturma](quick-deploy-iot-central.md) hızlı başlangıcının tamamlanmasıdır.