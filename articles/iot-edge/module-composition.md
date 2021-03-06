---
title: Azure IOT kenar modül bileşimi | Microsoft Docs
description: Dağıtım bildirimi dağıtmak için hangi modüllerin nasıl bildirir, bunları dağıtma ve aralarındaki ileti yollarını oluşturma öğrenin.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 209f159d9003838edb36728828758b76730118ff
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098473"
---
# <a name="learn-how-to-use-deployment-manifests-to-deploy-modules-and-establish-routes"></a>Modülleri dağıtmak ve yollar oluşturmak için dağıtım bildirimleri kullanmayı öğrenin

Her IOT sınır cihazı en az iki modüllerini çalıştırır: IOT kenar çalışma zamanı yapmak $edgeAgent ve $edgeHub,. Bu standart iki ek olarak, tüm IOT sınır cihazı herhangi bir sayıda işlemleri gerçekleştirmek için birden fazla modülü çalıştırabilirsiniz. Bir cihaza aynı anda bu modülleri dağıttığınızda modüllerine dahil edilir ve birbirleri ile nasıl etkileşim kurduklarını bildirmek için bir yönteme ihtiyacınız vardır. 

*Dağıtım bildirimi* açıklayan bir JSON belgesi:

* Kapsayıcı görüntünün her modül, erişim özel kapsayıcı kayıt defterleri için kimlik bilgilerini ve nasıl her modül oluşturulabilir ve yönetilebilir yönergeleri içerir kenar Aracısı yapılandırması.
* Nasıl akışı modülleri arasında ve sonunda IOT Hub'ına iletileri içeren kenar hub yapılandırması.
* İsteğe bağlı olarak, istenen özelliklerini modülü çiftlerini.

Tüm IOT sınır cihazları dağıtım bildirimi ile yapılandırılması gerekir. Yeni yüklenen bir IOT kenar çalışma zamanı hata kodu ile geçerli bir bildirim yapılandırılana kadar bildirir. 

Azure IOT kenar eğitimlerine Azure IOT kenar portalında Sihirbazı aracılığıyla giderek bir dağıtım bildirimi oluşturun. Program aracılığıyla REST veya IOT Hub hizmeti SDK'sını kullanarak dağıtım bildirimi de uygulayabilirsiniz. Daha fazla bilgi için bkz: [anlamak IOT kenar dağıtımları][lnk-deploy].

## <a name="create-a-deployment-manifest"></a>Bir dağıtım bildirimi oluşturma

Yüksek bir düzeyde dağıtım bildirimi IOT kenar cihazda dağıtılan IOT kenar modülleri için bir modül twin'ın istenen özelliklerini yapılandırır. İki bu modüllerin her zaman mevcut: `$edgeAgent`, ve `$edgeHub`.

Yalnızca IOT kenar çalışma (aracı ve hub) içeren bir dağıtım bildirimi geçerlidir.

Bildirim bu yapı aşağıdaki gibidir:

```json
{
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
            }
        },
        "$edgeHub": {
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "{module1}": {  // optional
            "properties.desired": {
                // desired properties of module with id {module1}
            }
        },
        "{module2}": {  // optional
            ...
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Modüllerini yapılandırın

IOT kenar çalışma zamanı modülleri dağıtımınızda yükleme bildirmeniz gerekir. İçindeki tüm modülleri için yapılandırma ve yönetim bilgisi gider **$edgeAgent** özellikleri istenen. Bu bilgiler kenar aracının kendisi için yapılandırma parametrelerini içerir. 

Veya dahil edilmelidir özelliklerinin tam listesi için bkz: [özelliklerini kenar aracısı ve kenar hub](module-edgeagent-edgehub.md).

Bu yapı $edgeAgent özellikleri izleyin:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
            "settings":{
                "registryCredentials":{ // give the edge agent access to container images that aren't public
                    }
                }
            }
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "{module1}": { // optional
                // configuration and management details
            },
            "{module2}": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Yollar bildirme

Edge hub bildirimli olarak modülleri arasında ve modülleri ve IOT hub'ı arasında iletileri yönlendirmek için bir yol sağlar. İçindeki rota bilgilerini gider şekilde kenar hub'ı tüm iletişimi yöneten **$edgeHub** özellikleri istenen. Aynı dağıtım içinde birden çok yol olabilir.

Yollar içinde bildirilen **$edgeHub** aşağıdaki söz dizimini özelliklerle istenen:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "{route1}": "FROM <source> WHERE <condition> INTO <sink>",
            "{route2}": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Bir kaynak ve bir havuz her rotanın gerekiyor, ancak iletilere filtre uygulamak için kullanabileceğiniz isteğe bağlı bir durumdur. 


### <a name="source"></a>Kaynak
Kaynak iletileri alınacağı yeri belirtir. Aşağıdaki değerlerden herhangi birini olabilir:

| Kaynak | Açıklama |
| ------ | ----------- |
| `/*` | Tüm cihaz bulut iletilerini herhangi bir aygıt veya modülü |
| `/messages/*` | Bir aygıt veya bazı veya hiçbir çıktı aracılığıyla bir modül tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/*` | Bazı veya hiçbir çıktı aracılığıyla bir modül tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/{moduleId}/*` | Hiçbir çıktı {Moduleıd} tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/{moduleId}/outputs/*` | Bazı çıkışıyla {Moduleıd} tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/{moduleId}/outputs/{output}` | {Moduleıd} kullanılarak {çıkış} gönderilen herhangi bir cihaz bulut iletisi |

### <a name="condition"></a>Koşul
Koşul rota bildiriminde isteğe bağlıdır. Tüm iletileri havuz kaynağına geçirmek istiyorsanız, yalnızca dışlamayı **burada** yan tümcesi tamamen. Veya kullanabilirsiniz [IOT hub'ı sorgu dili] [ lnk-iothub-query] belirli iletileri veya koşulu karşılıyor ileti türlerini filtre uygulamak için.

IOT kenar modülleri arasında geçiş iletiler aygıtlar ile Azure IOT hub'ı arasında geçirmek iletileri ile aynı biçimlendirilir. Tüm iletileri JSON olarak biçimlendirilmiş ve sahip **systemProperties**, **appProperties**, ve **gövde** parametreleri. 

Aşağıdaki sözdizimini kullanarak tüm üç parametre geçici sorgular oluşturabilirsiniz: 

* Sistem Özellikleri: `$<propertyName>` veya `{$<propertyName>}`
* Uygulama özellikleri: `<propertyName>`
* Gövde özellikleri: `$body.<propertyName>` 

İleti özellikleri için sorguları oluşturma hakkında daha fazla örnekler için bkz: [yollar sorgu ifadeleri cihaz bulut ileti](../iot-hub/iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).

Bir ağ geçidi aygıt bir yaprak aygıttan gelen iletiler için filtre uygulamak istediğiniz zaman IOT kenarına özgü bir örnektir. Modüllerden gelen iletileri içeren adlı bir sistem özelliği **connectionModuleId**. Böylece IOT Hub'ına doğrudan yaprak aygıtlardan iletileri yönlendirmek istiyorsanız, aşağıdaki rota modülü iletileri dışarıda bırakmak için kullanın:

```sql
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Havuz
Havuz iletileri gönderildiği tanımlar. Aşağıdaki değerlerden herhangi birini olabilir:

| Havuz | Açıklama |
| ---- | ----------- |
| `$upstream` | IOT Hub'ına ileti gönderme |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Giriş için ileti gönderilirken `{input}` Modülü `{moduleId}` |

IOT kenar en az bir kere garantileri sağlar. Yerel bir yol iletiyi, havuz için teslim durumda kenar hub iletileri depolar. Edge hub'ın IOT hub'ını veya hedef modülü bağlanamazsa, örneğin, bağlı değil.

Edge hub belirtilen süre kadar iletileri depolayan `storeAndForwardConfiguration.timeToLiveSecs` özelliği [kenar hub istenen özellikleri](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Tanımlamak veya istenen özelliklerini güncelleştir 

Dağıtım bildirimi IOT kenar cihaza dağıtılan her modülün modül çifti için istediğiniz özellikleri belirtebilirsiniz. İstenen özelliklerde dağıtım bildiriminde belirtildiğinde, tüm istenen özelliklerinde şu anda modülü twin üzerine yazın.

Dağıtım bildiriminde bir modül twin'ın istenen özellikleri belirtmezseniz, IOT hub'ı herhangi bir şekilde modülü twin değiştirmez ve istenen özellikleri programlı olarak ayarlamak mümkün olacaktır.

Cihaz çiftlerini değiştirmenize izin mekanizmalarının aynısını modülü çiftlerini değiştirmek için kullanılır. Daha fazla bilgi için bkz: [cihaz çifti Geliştirici Kılavuzu](../iot-hub/iot-hub-devguide-device-twins.md).   

## <a name="deployment-manifest-example"></a>Dağıtım bildirim örneği

Bu dağıtım bildirim JSON belgesini örneği.

```json
{
  "moduleContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "ContosoRegistry": {
                "username": "myacr",
                "password": "{password}",
                "address": "myacr.azurecr.io"
              }
            }
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
              "createOptions": ""
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
              "createOptions": ""
            }
          }
        },
        "modules": {
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
              "createOptions": "{}"
            }
          },
          "filtermodule": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "myacr.azurecr.io/filtermodule:latest",
              "createOptions": "{}"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {
          "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 10
        }
      }
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* $EdgeAgent ve $edgeHub dahil edilebilir ya da özelliklerinin tam listesi için bkz: [özelliklerini kenar aracısı ve kenar hub](module-edgeagent-edgehub.md).

* Artık IOT kenar modülleri nasıl kullanıldığı bildiğinize göre [IOT kenar modülleri geliştirmek için Araçlar ve gereksinimleri anlamanız][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
