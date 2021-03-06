---
title: Azure'da bir Service Fabric uygulamasına HTTPS uç noktası ekleme | Microsoft Docs
description: Bu öğreticide bir ASP.NET Core ön uç web hizmetine HTTPS uç noktası ekleme ve uygulamayı bir kümeye dağıtma işlemi hakkında bilgi alacaksınız.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/12/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 309a43d3383658029f4fe7f90f869888bac67bb1
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130059"
---
# <a name="tutorial-add-an-https-endpoint-to-an-aspnet-core-web-api-front-end-service"></a>Öğretici: Bir ASP.NET Core Web API’si ön uç hizmetine HTTPS uç noktası ekleme

Bu öğretici, bir serinin üçüncü bölümüdür.  Service Fabric üzerinde çalışan bir ASP.NET Core hizmetinde HTTPS’yi etkinleştirme hakkında bilgi edineceksiniz. İşlemi tamamladığınızda, 443 numaralı bağlantı noktasını dinleyen HTTPS özellikli bir ASP.NET Core web ön ucu içeren bir oylama uygulamasına sahip olursunuz. Oylama uygulamasını [.NET Service Fabric uygulaması derleme](service-fabric-tutorial-deploy-app-to-party-cluster.md) bölümünde el ile oluşturmak istemiyorsanız, tamamlanmış uygulamanın [kaynak kodunu indirebilirsiniz](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/).

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Hizmette bir HTTPS uç noktası tanımlama
> * Kestrel’i HTTPS kullanacak şekilde yapılandırma
> * Uzak küme düğümlerine SSL sertifikası yükleme
> * Sertifikanın özel anahtarına AĞ HİZMETİ erişimi verme
> * Azure yük dengeleyicide 443 numaralı bağlantı noktasını açma
> * Uygulamayı uzak kümeye dağıtma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Uygulamayı uzak kümeye dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme
> * [Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure geliştirme](https://www.visualstudio.com/) ve **ASP.NET ve Web geliştirme** iş yükleriyle **Visual Studio 2017** sürüm 15.5 veya üstünü yükleyin.
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="obtain-a-certificate-or-create-a-self-signed-development-certificate"></a>Sertifika edinme veya otomatik olarak imzalanan geliştirme sertifikası oluşturma

Üretim uygulamaları için [bir sertifika yetkilisinden (CA)](https://wikipedia.org/wiki/Certificate_authority) alınan bir sertifikayı kullanın. Geliştirme ve test için otomatik olarak imzalanan bir sertifika oluşturup bunu kullanabilirsiniz. Service Fabric SDK’sı, otomatik olarak imzalanan bir sertifika oluşturup `Cert:\LocalMachine\My` sertifika deposuna aktaran *CertSetup.ps1* betiğini sağlar. Yönetici olarak bir komut istemi açın ve aşağıdaki komutu çalıştırarak konusu "CN=localhost" olan bir sertifika oluşturun:

```powershell
PS C:\program files\microsoft sdks\service fabric\clustersetup\secure> .\CertSetup.ps1 -Install -CertSubjectName CN=localhost
```

Zaten bir sertifika PFX dosyanız varsa sertifikayı `Cert:\LocalMachine\My` sertifika deposuna aktarmak için aşağıdaki komutu çalıştırın:

```powershell

PS C:\mycertificates> Import-PfxCertificate -FilePath .\mysslcertificate.pfx -CertStoreLocation Cert:\LocalMachine\My -Password (ConvertTo-SecureString "!Passw0rd321" -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

## <a name="define-an-https-endpoint-in-the-service-manifest"></a>Hizmet bildiriminde bir HTTPS uç noktası tanımlama

Visual Studio’yu **yönetici** olarak başlatın ve Oylama çözümünü açın. Çözüm Gezgini'nde *VotingWeb/PackageRoot/ServiceManifest.xml* dosyasını açın. Hizmet bildirimi, hizmet uç noktalarını tanımlar.  **Uç Noktalar** bölümünü bulun ve var olan "ServiceEndpoint" uç noktasını düzenleyin.  Adı "EndpointHttps" olarak değiştirin, protokolü *https*, türü *Girdi*, bağlantı noktasını ise *443* olarak ayarlayın.  Yaptığınız değişiklikleri kaydedin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="configure-kestrel-to-use-https"></a>Kestrel’i HTTPS kullanacak şekilde yapılandırma

Çözüm Gezgini'nde *VotingWeb/VotingWeb.cs* dosyasını açın.  Kestrel’i HTTPS kullanacak şekilde yapılandırın ve `Cert:\LocalMachine\My` deposunda sertifikayı arayın. Deyimleri kullanarak aşağıdakileri ekleyin:

```csharp
using System.Net;
using Microsoft.Extensions.Configuration;
using System.Security.Cryptography.X509Certificates;
```

`ServiceInstanceListener` değerini yeni *EndpointHttps* uç noktasını kullanacak ve 443 numaralı bağlantı noktasını dinleyecek şekilde güncelleştirin.

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Ayrıca, Kestrel’in konuyu kullanarak sertifikayı `Cert:\LocalMachine\My` deposunda bulabilmesi için aşağıdaki yöntemi ekleyin.  Önceki PowerShell komutuyla otomatik olarak imzalanan bir sertifika oluşturduysanız "&lt;your_CN_value&gt;" değerini "localhost" ile değiştirin veya sertifikanızın CN değerini kullanın.

```csharp
private X509Certificate2 GetCertificateFromStore()
{
    var store = new X509Store(StoreName.My, StoreLocation.LocalMachine);
    try
    {
        store.Open(OpenFlags.ReadOnly);
        var certCollection = store.Certificates;
        var currentCerts = certCollection.Find(X509FindType.FindBySubjectDistinguishedName, "CN=<your_CN_value>", false);
        return currentCerts.Count == 0 ? null : currentCerts[0];
    }
    finally
    {
        store.Close();
    }
}
```

## <a name="give-network-service-access-to-the-certificates-private-key"></a>Sertifikanın özel anahtarına AĞ HİZMETİ erişimi verme

Önceki adımlardan birinde sertifikayı geliştirme bilgisayarındaki `Cert:\LocalMachine\My` deposuna aktarmıştınız.  Ayrıca, hizmeti (varsayılan olarak, AĞ HİZMETİ) çalıştıran hesaba sertifikanın özel anahtarı için açık erişim vermeniz gerekir. Bu işlemi el ile (certlm.msc aracını kullanarak) yapabilirsiniz, ancak hizmet bildiriminin **SetupEntryPoint** noktasında [bir başlangıç betiği yapılandırarak](service-fabric-run-script-at-service-startup.md) otomatik olarak bir PowerShell betiği çalıştırmanız daha iyi olur.

### <a name="configure-the-service-setup-entry-point"></a>Hizmet kurulumu giriş noktasını yapılandırma

Çözüm Gezgini'nde *VotingWeb/PackageRoot/ServiceManifest.xml* dosyasını açın.  **CodePackage** bölümünde **SetupEntryPoint** düğümünü ve sonra bir **ExeHost** düğümünü ekleyin.  **ExeHost** düğümünde **Program** ayarını "Setup.bat" ve **WorkingFolder** ayarını "CodePackage" olarak belirleyin.  VotingWeb hizmeti başlatıldığında, VotingWeb.exe başlatılmadan önce CodePackage klasöründe Setup.bat betiği yürütülür.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Setup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>

    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

### <a name="add-the-batch-and-powershell-setup-scripts"></a>Toplu işi ve PowerShell kurulum betiklerini ekleme

PowerShell’i **SetupEntryPoint** noktasından çalıştırmak için PowerShell.exe dosyasını bir PowerShell dosyasını işaret eden toplu iş dosyasında çalıştırabilirsiniz. İlk olarak, toplu iş dosyasını hizmet projesine ekleyin.  Çözüm Gezgini’nde **VotingWeb**’e sağ tıklayıp **Ekle**->**Yeni Öğe**’yi seçin ve "Setup.bat" adlı yeni bir dosya ekleyin.  *Setup.bat* dosyasını düzenleyin ve aşağıdaki komutu ekleyin:

```bat
powershell.exe -ExecutionPolicy Bypass -Command ".\SetCertAccess.ps1"
```

*Setup.bat* dosya özelliklerini, **Çıkış Dizinine Kopyala** değerini "Daha yeniyse kopyala" olarak ayarlayacak şekilde değiştirin.
![Dosya özelliklerini ayarlama][image1]

Çözüm Gezgini’nde **VotingWeb**’e sağ tıklayıp **Ekle**->**Yeni Öğe**’yi seçin ve "SetCertAccess.ps1" adlı yeni bir dosya ekleyin.  *SetCertAccess.ps1* dosyasını düzenleyin ve aşağıdaki betiği ekleyin:

```powershell
$subject="localhost"
$userGroup="NETWORK SERVICE"

Write-Host "Checking permissions to certificate $subject.." -ForegroundColor DarkCyan

$cert = (gci Cert:\LocalMachine\My\ | where { $_.Subject.Contains($subject) })[-1]

if ($cert -eq $null)
{
    $message="Certificate with subject:"+$subject+" does not exist at Cert:\LocalMachine\My\"
    Write-Host $message -ForegroundColor Red
    exit 1;
}elseif($cert.HasPrivateKey -eq $false){
    $message="Certificate with subject:"+$subject+" does not have a private key"
    Write-Host $message -ForegroundColor Red
    exit 1;
}else
{
    $keyName=$cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName

    $keyPath = "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys\"
    $fullPath=$keyPath+$keyName
    $acl=(Get-Item $fullPath).GetAccessControl('Access')


    $hasPermissionsAlready = ($acl.Access | where {$_.IdentityReference.Value.Contains($userGroup.ToUpperInvariant()) -and $_.FileSystemRights -eq [System.Security.AccessControl.FileSystemRights]::FullControl}).Count -eq 1

    if ($hasPermissionsAlready){
        Write-Host "Account $userGroupCertificate already has permissions to certificate '$subject'." -ForegroundColor Green
        return $false;
    } else {
        Write-Host "Need add permissions to '$subject' certificate..." -ForegroundColor DarkYellow

        $permission=$userGroup,"Full","Allow"
        $accessRule=new-object System.Security.AccessControl.FileSystemAccessRule $permission
        $acl.AddAccessRule($accessRule)
        Set-Acl $fullPath $acl

        Write-Output "Permissions were added"

        return $true;
    }
}

Modify the *SetCertAccess.ps1* file properties to set **Copy to Output Directory** to "Copy if newer".
```

### <a name="run-the-setup-script-as-a-local-administrator"></a>Kurulum betiğini yerel yönetici olarak çalıştırma

Varsayılan olarak, hizmet kurulumu giriş noktası yürütülebilir dosyası, Service Fabric ile aynı kimlik bilgileri altında (genellikle NetworkService hesabı) çalışır. *SetCertAccess.ps1* için yönetici ayrıcalıkları gerekir. Uygulama bildiriminde güvenlik izinlerini bir yerel yönetici hesabı altındaki başlangıç betiğini çalıştıracak şekilde değiştirebilirsiniz.

Çözüm Gezgini’nde *Voting/ApplicationPackageRoot/ApplicationManifest.xml* dosyasını açın. İlk olarak bir **Sorumlular** bölümü oluşturun ve yeni bir kullanıcı ekleyin (örneğin, "SetupAdminUser". SetupAdminUser kullanıcı hesabını Administrators sistem grubuna ekleyin.
Sonra, VotingWebPkg **ServiceManifestImport** bölümünde bir **RunAsPolicy** yapılandırarak SetupAdminUser sorumlusunu kurulum giriş noktasına uygulayın. Bu ilke, Service Fabric’e Setup.bat dosyasının SetupAdminUser olarak (yönetici ayrıcalıklarıyla) çalıştığını belirtir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Çözüm Gezgini'nde, **Voting** uygulamasını seçin ve **Uygulama URL'si** özelliğini “ https://localhost:443 ” olarak ayarlayın.

Tüm dosyaları kaydedin ve F5’e basarak uygulamayı yerel olarak çalıştırın.  Uygulama dağıtıldıktan sonra bir web tarayıcısında [https://localhost:443](https://localhost:443) sayfası açılır. Otomatik olarak imzalanan bir sertifika kullanıyorsanız, bilgisayarınızın bu web sitesinin güvenliğine güvenmediğini bildiren bir uyarı görürsünüz.  Web sayfasına devam edin.

![Oylama uygulaması][image2]

## <a name="install-certificate-on-cluster-nodes"></a>Küme düğümlerine sertifika yükleme

Uygulamayı Azure'a dağıtmadan önce sertifikayı uzak küme düğümlerinin `Cert:\LocalMachine\My` deposuna yükleyin.  Ön uç web hizmeti bir küme düğümünde başladığında, başlangıç betiği sertifikayı arar ve erişim izinlerini yapılandırır.

İlk olarak, sertifikayı bir PFX dosyasına aktarın. certlm.msc uygulamasını açın ve **Kişisel**>**Sertifikalar**’a gidin.  *localhost* sertifikasına sağ tıklayın ve **Tüm Görevler**>**Dışarı Aktar**’ı seçin.

![Sertifikayı dışarı aktarma][image4]

Dışarı aktarma sihirbazında **Evet, özel anahtarı dışarı aktar** seçeneğini belirleyin ve Kişisel Bilgi Değişimi (PFX) biçimini seçin.  Dosyayı *C:\Users\sfuser\votingappcert.pfx* konumuna aktarın.

Ardından, [Add-AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) cmdlet’ini kullanarak sertifikayı uzak kümeye yükleyin.

> [!Warning]
> Geliştirme ve test uygulamaları için otomatik olarak imzalanan bir sertifika yeterlidir. Üretim uygulamaları için otomatik olarak imzalanan sertifika yerine [bir sertifika yetkilisinden (CA)](https://wikipedia.org/wiki/Certificate_authority) sertifikayı kullanın.

```powershell
Connect-AzureRmAccount

$vaultname="sftestvault"
$certname="VotingAppPFX"
$certpw="!Password321#"
$groupname="voting_RG"
$clustername = "votinghttps"
$ExistingPfxFilePath="C:\Users\sfuser\votingappcert.pfx"

$appcertpwd = ConvertTo-SecureString -String $certpw -AsPlainText -Force

Write-Host "Reading pfx file from $ExistingPfxFilePath"
$cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2 $ExistingPfxFilePath, $certpw

$bytes = [System.IO.File]::ReadAllBytes($ExistingPfxFilePath)
$base64 = [System.Convert]::ToBase64String($bytes)

$jsonBlob = @{
   data = $base64
   dataType = 'pfx'
   password = $certpw
   } | ConvertTo-Json

$contentbytes = [System.Text.Encoding]::UTF8.GetBytes($jsonBlob)
$content = [System.Convert]::ToBase64String($contentbytes)

$secretValue = ConvertTo-SecureString -String $content -AsPlainText -Force

# Upload the certificate to the key vault as a secret
Write-Host "Writing secret to $certname in vault $vaultname"
$secret = Set-AzureKeyVaultSecret -VaultName $vaultname -Name $certname -SecretValue $secretValue

# Add a certificate to all the VMs in the cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $secret.Id -Verbose
```

## <a name="open-port-443-in-the-azure-load-balancer"></a>Azure yük dengeleyicide 443 numaralı bağlantı noktasını açma

Henüz açık değilse yük dengeleyicide 443 numaralı bağlantı noktasını açın.

```powershell
$probename = "AppPortProbe6"
$rulename="AppPortLBRule6"
$RGname="voting_RG"
$port=443

# Get the load balancer resource
$resource = Get-AzureRmResource | Where {$_.ResourceGroupName –eq $RGname -and $_.ResourceType -eq "Microsoft.Network/loadBalancers"}
$slb = Get-AzureRmLoadBalancer -Name $resource.Name -ResourceGroupName $RGname

# Add a new probe configuration to the load balancer
$slb | Add-AzureRmLoadBalancerProbeConfig -Name $probename -Protocol Tcp -Port $port -IntervalInSeconds 15 -ProbeCount 2

# Add rule configuration to the load balancer
$probe = Get-AzureRmLoadBalancerProbeConfig -Name $probename -LoadBalancer $slb
$slb | Add-AzureRmLoadBalancerRuleConfig -Name $rulename -BackendAddressPool $slb.BackendAddressPools[0] -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -Probe $probe -Protocol Tcp -FrontendPort $port -BackendPort $port

# Set the goal state for the load balancer
$slb | Set-AzureRmLoadBalancer
```

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma

Tüm dosyaları kaydedin, Hata Ayıklama’dan Yayın’a geçin ve F6’ya basarak yeniden oluşturun.  Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. [Bir kümeye uygulama dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md) bölümünde oluşturulan kümenin bağlantı uç noktasını veya başka bir kümeyi seçin.  Uygulamayı uzak kümede yayımlamak için **Yayımla**’ya tıklayın.

Uygulama dağıtılırken bir web tarayıcısı açın ve [https://mycluster.region.cloudapp.azure.com:443](https://mycluster.region.cloudapp.azure.com:443) sayfasına gidin (URL'yi kümenizin bağlantı uç noktasıyla güncelleştirin). Otomatik olarak imzalanan bir sertifika kullanıyorsanız, bilgisayarınızın bu web sitesinin güvenliğine güvenmediğini bildiren bir uyarı görürsünüz.  Web sayfasına devam edin.

![Oylama uygulaması][image3]

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Hizmette bir HTTPS uç noktası tanımlama
> * Kestrel’i HTTPS kullanacak şekilde yapılandırma
> * Uzak küme düğümlerine SSL sertifikası yükleme
> * Sertifikanın özel anahtarına AĞ HİZMETİ erişimi verme
> * Azure yük dengeleyicide 443 numaralı bağlantı noktasını açma
> * Uygulamayı uzak kümeye dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

[image1]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/SetupBatProperties.png
[image2]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/VotingAppLocal.png
[image3]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/VotingAppAzure.png
[image4]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/ExportCert.png
