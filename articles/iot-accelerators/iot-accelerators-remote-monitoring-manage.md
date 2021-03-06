---
title: Azure tabanlı uzaktan izleme çözümündeki cihazları yönetme | Microsoft Docs
description: Bu öğreticide Uzaktan İzleme çözümü hızlandırıcısına bağlı cihazların nasıl yönetileceği gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/12/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 63baf6397b2542311525bac740c50b5eacbd35cf
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097436"
---
# <a name="tutorial-configure-and-manage-devices-connected-to-your-monitoring-solution"></a>Öğretici: İzleme çözümünüze bağlı cihazları yapılandırma ve yönetme

Bu öğreticide bağlı IoT cihazlarınızı yapılandırmak ve yönetmek için Uzaktan İzleme çözümü hızlandırıcısını kullanacaksınız. Çözüm hızlandırıcısına yeni bir cihaz ekleyecek, cihazı yapılandıracak ve cihazın üretici yazılımını güncelleştireceksiniz.

Contoso, tesislerinden birini genişletmek için yeni makineler sipariş etmiştir. Yeni makinelerin teslim edilmesini beklerken çözümünüzün davranışını test etme amacıyla bir simülasyon çalıştırmak istiyorsunuz. Simülasyonu çalıştırmak için Uzaktan İzleme çözümü hızlandırıcısına yeni bir sanal motor cihazı ekleyecek ve bu sanal cihazın eylemlere ve yapılandırma güncelleştirmelerine doğru yanıt verip vermediğini test edeceksiniz.

Uzaktan İzleme çözümü hızlandırıcısı, cihazları yapılandırmak ve yönetmek üzere genişletilebilir bir yöntem sağlamak üzere [işler](../iot-hub/iot-hub-devguide-jobs.md) ve [doğrudan yöntemler](../iot-hub/iot-hub-devguide-direct-methods.md) gibi IoT Hub özelliklerini kullanır. Bu öğreticide sanal cihazlar kullanılmaktadır ancak cihaz geliştiricileri [Uzaktan İzleme çözümü hızlandırıcısına bağlı fiziksel cihazlara da](iot-accelerators-connecting-devices.md) doğrudan yöntemler uygulayabilir.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Sanal cihaz sağlama.
> * Sanal cihazı test etme.
> * Cihazın üretici yazılımını güncelleştirme.
> * Cihazı yeniden yapılandırma.
> * Cihazlarınızı düzenleme.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi takip etmek için Azure aboneliğinizde Uzaktan İzleme çözümü hızlandırıcısının dağıtılmış örneğine sahip olmanız gerekir.

Uzaktan İzleme çözümü hızlandırıcısını henüz dağıtmadıysanız [Bulut tabanlı uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını tamamlamanız gerekir.

## <a name="add-a-simulated-device"></a>Sanal cihaz ekleme

Çözümün **Devices** (Cihazlar) sayfasına gidin ve **+ New device** (Yeni cihaz) öğesine tıklayın:

[![Sanal cihaz sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

**New device** (Yeni cihaz) panelinde **Simulated** (Sanal) öğesini seçin, sağlanacak cihaz sayısını **1** olarak bırakın, **Faulty Engine** (Arızalı Motor) cihaz modelini seçin ve **Apply** (Uygula) öğesine tıklayarak sanal cihazı oluşturun:

[![Sanal motor cihazı sağlama](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Sanal cihazı test etme

Sanal cihazınızın telemetri ve raporlama özellik değeri değerleri gönderip göndermediğini test etmek için **Devices** (Cihazlar) sayfasındaki cihaz listesinden seçin. Cihazınızla ilgili canlı bilgiler **Device Details** (Cihaz Ayrıntıları) panelinde görüntülenir:

[![Yeni sanal motor cihazını görüntüleme](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

**Device Details** (Cihaz Ayrıntıları) sayfasında yeni cihazınızın telemetri verileri gönderdiğini doğrulayın. Cihazınızdan gelen farklı titreşim telemetrisi akışını görüntülemek için **Vibration** (Titreşim) öğesine tıklayın:

[![Görüntülenecek telemetri akışını seçme](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

**Device Details** (Cihaz Ayrıntıları) panelinde etiket değerleri, desteklediği metotlar ve cihaz tarafından bildirilen özellikler gibi cihaz hakkındaki diğer bilgiler görüntülenir.

Ayrıntılı tanılama verilerini görüntülemek için sayfayı kaydırarak **Diagnostics** (Tanılama) bölümüne inin:

[![Cihaz tanılama bilgilerini görüntüleme](./media/iot-accelerators-remote-monitoring-manage/devicediagnostics-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicediagnostics-expanded.png#lightbox)

## <a name="act-on-a-device"></a>Cihazda eylem gerçekleştirme

Sanal motor cihazının çözüm hızlandırıcısından başlatılan eylemlere doğru yanıt verip vermediğini test etmek için **FirmwareUpdate** metodunu çağırın. Bir metot çalıştırarak cihazda eylem gerçekleştirmek için cihazı listeden seçip **Jobs** (İşler) öğesine tıklayın. Birden fazla cihazda eylem gerçekleştirme istiyorsanız birden fazla cihaz seçebilirsiniz. **Jobs** (İşler) panelinde **Run method** (Metodu çalıştır) öğesini seçin. **Engine** (Motor) cihaz modeli üç metot belirtir: **FirmwareUpdate**, **FillTank** ve **EmptyTank**:

[![Motor metotları](./media/iot-accelerators-remote-monitoring-manage/devicesmethods-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmethods-expanded.png#lightbox)

**FirmwareUpdate** girişini seçin, iş adını **UpdateEngineFirmware**, cihaz yazılımı sürümünü **2.0.0**, üretici yazılımı URI'sini **http://contoso.com/engine.bin** olarak ayarlayıp **Apply** (Uygula) öğesine tıklayın:

[![Üretici yazılımı güncelleştirme yöntemini zamanlama](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatejob-inline.png)](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatejob-expanded.png#lightbox)

İşin durumunu izlemek için **View job status** (İş durumunu görüntüle) öğesine tıklayın:

[![Zamanlanmış cihaz yazılımı güncelleştirme işini izleme](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatestatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/firmwareupdatestatus-expanded.png#lightbox)

İş tamamlandıktan sonra **Devices** (Cihazlar) sayfasına dönün. Motor cihazının yeni cihaz yazılımı sürümü görüntülenir.

**Devices** (Cihazlar) sayfasından farklı türlerde birden fazla cihaz seçtiğinizde de bu cihazlarda metot çalıştırmak üzere bir iş oluşturabilirsiniz. **Jobs** (İşler) panelinde yalnızca seçilen cihazların tümünde kullanılabilen metotlar gösterilir.

## <a name="reconfigure-a-device"></a>Cihazı yeniden yapılandırma

Motorun yapılandırma özelliklerini güncelleştirip güncelleştiremeyeceğinizi test etmek için cihazı **Devices** (Cihazlar) sayfasındaki listeden seçin. Ardından **Jobs** (İşler) öğesine tıklayıp **Reconfigure** (Yeniden yapılandır) öğesini seçin. İşler panelinde seçilen cihaz için güncelleştirilebilecek özellik değerleri gösterilir:

[![Cihazı yeniden yapılandırma](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Motorun konumunu güncelleştirmek için iş adını **UpdateEngineLocation**, boylamı **-122.15**, konumu **Factory 2**, enlemi **47.62** olarak ayarlayıp **Apply** (Uygula) öğesine tıklayın:

[![Cihazın özellik değerini güncelleştirme](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

İşin durumunu izlemek için **View job status** (İş durumunu görüntüle) öğesine tıklayın:

[![Cihazın özellik değerini güncelleştirme](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

İş tamamlandıktan sonra **Dashboard** (Pano) sayfasına gidin. Motor cihazı haritada yeni konumunda görüntülenir:

[![Motor konumunu görüntüleme](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Cihazlarınızı düzenleme

Operatör olarak cihazlarınızı düzenlemeyi ve yönetmeyi kolaylaştırmak için uygun takım adıyla etiketleyebilirsiniz. Contoso'da saha hizmeti etkinlikleri için iki farklı takım vardır:

* Smart Vehicle takımı tırları ve prototip cihazlarını yönetmektedir.
* Smart Building takımı ise soğutucuları, asansörleri ve motorları yönetmektedir.

Tüm cihazlarınızı görüntülemek için **Devices** (Cihazlar) sayfasına gidin ve **All devices** (Tüm cihazlar) filtresini seçin:

[![Tüm cihazları göster](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Etiket ekleme

**Trucks** (Tırlar) ve **Prototyping** (Prototip) cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın:

[![Prototype (prototip) ve truck (tır) cihazlarını seçme](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect-expanded.png#lightbox)

**Tag** (Etiket) öğesini seçin, iş adını **AddConnectedVehicleTag** olarak değiştirip **FieldService** adlı bir metin etiketi ekleyip **ConnectedVehicle** değerini verin. Ardından **Apply** (Uygula) öğesine tıklayın:

[![Prototype (prototip) ve truck (tır) cihazlarına etiket ekleme](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

Cihaz sayfasında tüm **Chiller** (Soğutucu), **Elevator** (Asansör) ve **Engine** (Motor) cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın:

[![Chiller (soğutucu), elevator (asansör) ve engine (motor) cihazlarını seçme](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmultiselect2-expanded.png#lightbox)

**Tag** (Etiket) öğesini seçin, iş adını **AddSmartBuildingTag** olarak değiştirip **FieldService** adlı bir metin etiketi ekleyip **SmartBuilding** değerini verin. Ardından **Apply** (Uygula) öğesine tıklayın:

[![Chiller (soğutucu), elevator (asansör) ve engine (motor) cihazlarına etiket ekleme](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Filtre oluşturma

Artık bu etiket değerlerini kullanarak filtre oluşturabilirsiniz. **Devices** (Cihazlar) sayfasında **Manage device groups** (Cihaz gruplarını yönet) öğesine tıklayın:

[![Cihaz gruplarını yönetme](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

**FieldService** etiket adını ve koşul olarak **SmartBuilding** değerini kullanan bir metin filtresi oluşturun. Filtreyi **Smart Building** adıyla kaydedin:

[![Smart Building adlı bir filtre oluşturma](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

**FieldService** etiket adını ve koşul olarak **ConnectedVehicle** değerini kullanan bir metin filtresi oluşturun. Filtreyi **Connected Vehicle** adıyla kaydedin.

[![Connected Vehicle adlı bir filtre oluşturma](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Contoso operatörü artık operasyon ekibine göre cihazları sorgulayabilir:

[![Connected Vehicle adlı bir filtre oluşturma](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın. Kullanmadığınız zaman çözüm hızlandırıcısının maliyetlerini düşürmek için ayarlar panelinden sanal cihazları durdurabilirsiniz:

[![Telemetriyi duraklatma](./media/iot-accelerators-remote-monitoring-manage/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/togglesimulation-expanded.png#lightbox)

Bir sonraki öğreticiye başlamaya hazır olduğunuzda sanal cihazları yeniden başlatabilirsiniz.

Çözüm hızlandırıcısına ihtiyacınız kalmadıysa [Provisioned solutions](https://www.azureiotsolutions.com/Accelerators#dashboard) (Sağlanan çözümler) sayfasından silebilirsiniz:

![Çözümü sil](media/iot-accelerators-remote-monitoring-manage/deletesolution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Uzaktan İzleme çözümü hızlandırıcısına bağlı cihazların nasıl yapılandırılacağı ve yönetileceği gösterilmektedir. Çözüm hızlandırıcısını bağlı cihazlarınızdaki sorunları tanımlama ve düzeltme amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazlarla sorunları tanımlamak ve düzeltmek için cihaz uyarılarını kullanma](iot-accelerators-remote-monitoring-maintain.md)
