---
title: Azure Service Fabric ters proxy'si | Microsoft Docs
description: Service Fabric'in ters proxy, mikro hizmetler iç ve dış küme iletişimi için kullanın.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/03/2017
ms.author: bharatn
ms.openlocfilehash: bec2e443b920a1f163b7b328197d3688d207ed35
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309128"
---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Azure Service Fabric ters proxy
Azure Service Fabric'te yerleşik ters proxy bulmak ve http uç noktaları olan diğer hizmetlerle iletişim kurma bir Service Fabric kümesinde çalışan mikro hizmetler yardımcı olur.

## <a name="microservices-communication-model"></a>Mikro hizmet iletişimi modeli
Service fabric'te mikro hizmetler, kümedeki düğümlerin bir alt kümesi üzerinde çalıştırın ve çeşitli nedenlerle düğümler arasında geçiş yapabilirsiniz. Sonuç olarak, bir mikro hizmet uç noktaları dinamik olarak değiştirebilirsiniz. Mikro hizmet bulma ve kümedeki diğer hizmetleriyle iletişim kurmak için aşağıdaki adımları izleyerek gitmeniz gerekir:

1. Adlandırma hizmeti aracılığıyla hizmet konumu çözümleyin.
2. Hizmete bağlanın.
3. Önceki adımlarda service çözümleme uygulayan bir döngüde sarmalayın ve yeniden deneme ilkelerine bağlantı hataları uygulamak için

Daha fazla bilgi için [Bağlan ve hizmetlerle iletişim kurma](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-the-reverse-proxy"></a>Ters proxy kullanarak iletişim kurma
Ters proxy, her düğüm üzerinde çalışan ve uç nokta çözümleme işleme hizmeti olan, otomatik yeniden deneme ve diğer istemci hizmetlerini adına bağlantı hataları. Ters proxy, istemci hizmetlerini gelen istekleri işleme gibi çeşitli ilkeler uygulamak için yapılandırılabilir. Ters proxy kullanarak herhangi bir istemci-tarafı HTTP iletişim kitaplıklarını kullanın ve değil özel çözümlemesini gerektirir ve yeniden deneme mantığı hizmetinde istemci hizmeti sağlar. 

Ters proxy, diğer hizmetlere istekleri göndermek için kullanılacak İstemci Hizmetleri için yerel düğümde bir veya daha fazla uç noktalarını kullanıma sunar.

![İç iletişim][1]

> [!NOTE]
> **Desteklenen platformlar**
>
> Service Fabric ters proxy şu anda aşağıdaki platformları destekler.
> * *Windows Küme*: Windows 8 ve sonraki veya Windows Server 2012 ve sonraki sürümler
> * *Linux kümesi*: Ters Proxy için Linux kümeleri şu anda kullanılabilir değil
>

## <a name="reaching-microservices-from-outside-the-cluster"></a>Mikro hizmetler küme dışındaki ulaşma
Varsayılan dış iletişim mikro hizmetlere yönelik olduğu her hizmet doğrudan dış istemcilerden erişilemez katılımı, bir modeldir. [Azure Load Balancer](../load-balancer/load-balancer-overview.md), mikro hizmetler ve dış istemciler arasında bir ağ sınırında olduğu ağ adresi çevirisi gerçekleştirir ve iç IP: BağlantıNoktası uç noktalar dış isteklerini iletir. Bir mikro hizmet ait uç nokta dış istemcilere doğrudan erişilebilir hale getirmek için önce kümede hizmetin kullandığı her bir bağlantı noktası trafik iletmek için yük dengeleyici yapılandırmanız gerekir. Ayrıca, çoğu mikro hizmetler, özellikle de durum bilgisi olan mikro hizmetler, tüm küme düğümlerinde Canlı yok. Mikro hizmetler, yük devretme sırasında düğümler arasında taşıyabilirsiniz. Bu gibi durumlarda, yük dengeleyici, trafiği ileterek çoğaltmaların hedef düğüm konumunu etkili bir şekilde belirlenemiyor.

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a>Mikro Hizmetler aracılığıyla küme dışındaki ters proxy sunucudan ulaşma
Yük Dengeleyicide bir bireysel hizmet bağlantı noktasını yapılandırmak yerine, yük dengeleyici yalnızca ters proxy bağlantı noktası yapılandırabilirsiniz. Bu yapılandırma, kümenin dışındaki istemcilerin ek yapılandırma olmadan ters proxy kullanarak küme içinde hizmetlere erişmek olanak tanır.

![Dış iletişimi][0]

> [!WARNING]
> Yük Dengeleyici ters proxy bağlantı noktası yapılandırdığınızda, bir HTTP uç noktasını açığa tüm mikro Hizmetleri kümedeki küme dışında'ten adreslenebilir. Başka bir deyişle, mikro hizmetler iç olacak şekilde tasarlanmış belirlenen kötü niyetli bir kullanıcı tarafından keşfedilebilir olabilir. Bu potenially yararlanılabilir ciddi güvenlik açıklarını sunar; Örneğin:
>
> * Kötü niyetli bir kullanıcı, sürekli olarak yeterli saldırı yüzeyini yok. bir iç hizmet çağırarak bir hizmet reddi saldırısı başlatabilir.
> * Kötü niyetli bir kullanıcının istenmeyen davranışla bir iç hizmet için hatalı biçimlendirilmiş paketlerini teslim edebilir.
> * İç olacak şekilde tasarlanmış bir hizmet, böylece kötü niyetli bir kullanıcı için bu hassas bilgileri gösterme, küme dışındaki hizmetlere açığa amaçlanmaz özel veya hassas bilgiler döndürebilir. 
>
> Tam olarak anlamak ve kümenizi ve ters proxy bağlantı noktası genel yapmadan önce bunun üzerinde çalışan uygulamalar için olası güvenlik sonuçları azaltmak emin olun. 
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a>Ters proxy kullanarak Hizmetleri çözmeye yönelik URI biçimi
Ters proxy, gelen istek iletilmesi gereken hizmet bölümü tanımlamak için belirli bir Tekdüzen Kaynak Tanımlayıcısı (URI) biçimi kullanır:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **http (s):** ters proxy, HTTP veya HTTPS trafiğini kabul edecek şekilde yapılandırılabilir. HTTPS iletme için başvurmak [güvenli hizmet ters proxy ile bağlanma](service-fabric-reverseproxy-configure-secure-communication.md) HTTPS üzerinde dinlemek için ters proxy ayarladıktan sonra.
* **Küme tam etki alanı adı (FQDN) | iç IP:** dış istemciler için mycluster.eastus.cloudapp.azure.com gibi küme etki alanı üzerinden erişilebilir olması ters Ara sunucu yapılandırabilirsiniz. Varsayılan olarak, ters proxy her düğüm üzerinde çalışır. İç trafiği için ters proxy 10.0.0.1 gibi herhangi bir düğüm iç IP veya localhost üzerinde erişilebilir.
* **Bağlantı noktası:** ters proxy için belirtilen bağlantı noktası, 19081 gibi budur.
* **ServiceInstanceName:** olmadan erişmeye çalıştığınız dağıtılan hizmet örneği tam olarak nitelenmiş adını budur "fabric: /" düzeni. Örneğin, ulaşmak için *fabric: / Uygulamam/Hizmetim/* hizmeti kullandığınız *Uygulamam/Hizmetim*.

    Hizmet örneği adı büyük/küçük harfe duyarlıdır. URL'de hizmet örneği adı farklı büyük/küçük harf kullanılması, istekleri ile (404 bulunamadı) kodunu başarısız olmasına neden olur.
* **Yol soneki:** gerçek URL yolu gibi budur *myapi/değerler/ekleme/3*, bağlanmak istediğiniz hizmet için.
* **PartitionKey:** bölümlenmiş bir hizmet için hesaplanan bölüm anahtarı, erişmek istediğiniz bölümün budur. Bu Not *değil* bölüm kimliği GUID. Bu parametre, tek bölüm düzeni kullanan hizmetler için gerekli değildir.
* **PartitionKind:** hizmet bölüm şeması budur. Bu, 'Int64Range' veya 'Adlı' olabilir. Bu parametre, tek bölüm düzeni kullanan hizmetler için gerekli değildir.
* **ListenerName** hizmet uç noktalarının biçimindedir {"Uç noktalar": {"Listener1": "Bitiş noktası 1", "Listener2": "Endpoint2" …}}. Hizmet birden çok uç noktayı kullanıma sokan, bu istemci isteği iletilmesi gereken uç nokta tanımlar. Hizmetin yalnızca bir dinleyicisi varsa bu yoksayılabilir.
* **TargetReplicaSelector** bu hedef çoğaltma veya örnek nasıl seçilmelidir belirtir.
  * Hedef hizmet durum bilgisi olan olduğunda TargetReplicaSelector aşağıdakilerden biri olabilir: 'PrimaryReplica', 'RandomSecondaryReplica' veya 'RandomReplica'. Bu parametre belirtilmediğinde 'PrimaryReplica' varsayılandır.
  * Hedef hizmet durum bilgisiz olduğunda, ters proxy hizmeti bölümü isteği iletmek için rastgele bir örneğini seçer.
* **Zaman aşımı:** bu istemci istek adına hizmeti için ters Ara sunucu tarafından oluşturulan HTTP isteği için zaman aşımını belirtir. Varsayılan değer 60 saniyedir. Bu isteğe bağlı bir parametredir.

### <a name="example-usage"></a>Örnek Kullanım
Bir örnek olarak alalım *fabric: / Uygulamam/Hizmetim* aşağıdaki URL'yi temel bir HTTP dinleyicisi açılır hizmeti:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Hizmeti için kaynakları şunlardır:

* `/index.html`
* `/api/users/<userId>`

Hizmet bölümleme düzeni, tekli kullanıyorsa *PartitionKey* ve *PartitionKind* sorgu dizesi parametresi gerekli değildir ve hizmet ağ geçidi olarak kullanarak ulaşılabilir:

* Harici olarak: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Dahili olarak: `http://localhost:19081/MyApp/MyService`

Hizmet Tekdüzen Int64 bölümleme düzenini kullanıyorsa *PartitionKey* ve *PartitionKind* hizmete ilişkin bir bölüm ulaşmak için sorgu dizesi parametreleri kullanılmalıdır:

* Harici olarak: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Dahili olarak: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Hizmet sunan kaynaklarına ulaşmak için yalnızca URL'ye hizmet adından sonra kaynak yolu yerleştirin:

* Harici olarak: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Dahili olarak: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Ağ geçidi, ardından bu istekleri hizmet URL'sine yönlendiren:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Bağlantı noktası paylaşımı özel işlem Hizmetleri
Service Fabric ters proxy hizmeti adresi tekrar çözün ve bir hizmet erişildiğinde isteği yeniden deneyin. dener. Bir hizmetine ulaşılamıyor, genel olarak, hizmet örneği veya çoğaltma normal yaşam döngüsü bir parçası olarak farklı bir düğüme taşınmıştır. Ters proxy, bu durumda, bir uç nokta artık başlangıçta çözümlenen adresinde açık olduğunu belirten bir ağ bağlantısı hatası alabilirsiniz.

Ancak, çoğaltmalar veya hizmet örnekleri bir ana bilgisayar işlemi paylaşabilir ve aynı zamanda bir http.sys tabanlı bir web sunucusu tarafından barındırılan bir bağlantı noktası paylaşabilir dahil olmak üzere:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Bu durumda web sunucusu ana bilgisayar işlemi ve isteklere yanıt olarak kullanılabilir, ancak çözümlenen hizmet örneği veya çoğaltma artık ana bilgisayarda mevcut değil. Bu durumda, ağ geçidi web sunucusundan bir HTTP 404 yanıtı alırsınız. Bu nedenle, bir HTTP 404 yanıt iki farklı anlamlara sahip olabilir:

- #1. durum: Hizmet adresinin doğru olduğundan, ancak kullanıcının istenen kaynak yok.
- #2. durum: Hizmet adresi yanlış ve kullanıcının istenen kaynak üzerinde farklı bir düğüme bulunmayabilir.

Bir kullanıcı hata olarak kabul edilen normal HTTP 404, ilk bir durumdur. Ancak, İkinci durumda, mevcut bir kaynak kullanıcı istedi. Ters proxy hizmeti taşınmış olduğundan bunu bulamıyoruz. Ters proxy adresini yeniden çözümlemek ve isteği yeniden denemek gerekir.

Ters proxy, bu nedenle bu iki durum arasında ayrım yapmak için bir yönteme ihtiyaç duyar. Bu ayrım yapmak için sunucudan bir ipucu gereklidir.

* Varsayılan olarak, ters proxy #2. durum varsayar ve gidermek ve isteği yeniden göndermek çalışır.
* Hizmet ters proxy #1 çalışmasına belirtmek için aşağıdaki HTTP yanıt üst bilgisi döndürmesi gerekir:

  `X-ServiceFabric : ResourceNotFound`

Bu HTTP yanıt üst bilgisi, istenen kaynak yok ve çözümlemeyi tekrar hizmet adresinin ters proxy çalışmaz normal bir HTTP 404 durumu belirtir.

## <a name="setup-and-configuration"></a>Kurulum ve yapılandırma

### <a name="enable-reverse-proxy-via-azure-portal"></a>Azure portal aracılığıyla ters Proxy'yi Etkinleştir

Azure portalında yeni bir Service Fabric küme oluşturma sırasında ters proxy etkinleştirmek için bir seçenek sunar.
Altında **oluşturma Service Fabric kümesi**, adım 2: küme yapılandırması, düğüm türü yapılandırması, "Ters Proxy'yi Etkinleştir" onay kutusunu seçin.
Güvenli ters Proxy'yi yapılandırmak için SSL sertifikası adım 3'te belirtilebilir: güvenlik, küme güvenlik ayarlarını yapılandırma "bir ters proxy SSL sertifikası ekleme" ve sertifika ayrıntılarını girmek için onay kutusunu işaretleyin.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Azure Resource Manager şablonları aracılığıyla ters Proxy'yi Etkinleştir

Kullanabileceğiniz [Azure Resource Manager şablonu](service-fabric-cluster-creation-via-arm.md) Service Fabric ters Ara sunucu kümesi için etkinleştirmek için.

Başvurmak [HTTPS Ters Proxy Yapılandırma güvenli bir küme içinde](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample/README.md#configure-https-reverse-proxy-in-a-secure-cluster) için Azure Resource Manager şablonu örnekleri güvenli yapılandırmak için ters proxy ile bir sertifika ve işleme sertifika geçişi.

İlk olarak, dağıtmak istediğiniz küme için şablon alın. Örnek şablonları kullanabilir veya özel bir Resource Manager şablonu oluşturun. Ardından, aşağıdaki adımları kullanarak ters proxy etkinleştirebilirsiniz:

1. Ters proxy için bir bağlantı noktasını tanımlamak [parametreler bölümü](../azure-resource-manager/resource-group-authoring-templates.md) şablonu.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Her bir nodetype nesneleri için bağlantı noktasını belirtin **küme** [kaynak türü bölümüne](../azure-resource-manager/resource-group-authoring-templates.md).

    Bağlantı noktası, parametre adı, reverseProxyEndpointPort tanımlanır.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. Ters dışından Ara sunucuya Azure kümesine yönelik olarak, 1. adımda belirttiğiniz bağlantı noktası için Azure Load Balancer kuralları ayarlayın.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. SSL sertifikaları için ters proxy bağlantı noktası üzerinde yapılandırmak için sertifikaya eklemek ***reverseProxyCertificate*** özelliğinde **küme** [kaynak türü bölümüne](../resource-group-authoring-templates.md) .

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Küme sertifikası farklı bir ters proxy sertifikasını destekleme
 Ters proxy sertifikasını kümenin güvenliğini sertifikasından farklıysa, ardından daha önce belirtilen sertifika sanal makine üzerinde yüklenmeli ve Service Fabric erişebilmesi erişim denetim listesine (ACL) eklendi. Bu yapılabilir **virtualMachineScaleSets** [kaynak türü bölümüne](../resource-group-authoring-templates.md). Yükleme için bu sertifika için osProfile ekleyin. ACL sertifika şablonu uzantısı bölümünü güncelleştirebilirsiniz.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Var olan bir kümede ters proxy etkinleştirmek için küme sertifikası farklı sertifikaları kullandığınızda, ters proxy sertifikasını yükleyin ve ters proxy etkinleştirmeden önce küme üzerinde bir ACL güncelleştirin. Tamamlamak [Azure Resource Manager şablonu](service-fabric-cluster-creation-via-arm.md) belirtilen ayarları kullanarak dağıtım daha önce ters proxy etkinleştirmek için bir dağıtım başlamadan önce adımları 1-4.

## <a name="next-steps"></a>Sonraki adımlar
* HTTP iletişim hizmetleri arasında bir örneğini görmek bir [GitHub üzerinde örnek proje](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Güvenli HTTP hizmetine ileten ters proxy ile](service-fabric-reverseproxy-configure-secure-communication.md)
* [Reliable Services uzaktan iletişimi ile uzak yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [OWIN güvenilir hizmetler kullanan Web](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services kullanarak WCF iletişim](service-fabric-reliable-services-communication-wcf.md)
* Ek bir ters proxy yapılandırma seçenekleri için Applicationgateway'inin/Http bölümüne başvurun [özelleştirme Service Fabric küme ayarlarını](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
