---
title: Azure yığınında kullanan ayrıcalıklı uç noktasını | Microsoft Docs
description: Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) kullanmak nasıl Azure yığınında (Azure yığın işleci için) gösterir.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: fiseraci
ms.openlocfilehash: 9fb928b7cb8e1a83734b64a8b9c19bc3cf3203ba
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32153193"
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Azure yığınında ayrıcalıklı uç noktası kullanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın operatör olarak, Yönetici portalı'nı, PowerShell veya Azure Resource Manager API'leri en günlük yönetim görevleri için kullanmanız gerekir. Ancak, bazı ortak işlemleri daha az kullanmanız gerekir *ayrıcalıklı endpoint* (CESARETLENDİRİCİ). CESARETLENDİRİCİ gerekli bir görevi gerçekleştirmenize yardımcı olmak için yeterli yetenekleri sağlayan bir önceden yapılandırılmış Uzaktan PowerShell konsoludur. Uç nokta kullanır [PowerShell JEA (yalnızca yeterince yönetim)](https://docs.microsoft.com/powershell/jea/overview) cmdlet'leri yalnızca sınırlı sayıda kullanıma sunmak için. CESARETLENDİRİCİ erişmek ve kısıtlı bir cmdlet kümesi çağırmak için bir düşük ayrıcalıklı hesap kullanılır. Hiç yönetici hesabı gereklidir. Ek güvenlik için komut dosyası izin verilmiyor.

Aşağıdaki gibi görevleri gerçekleştirmek için CESARETLENDİRİCİ kullanabilirsiniz:

- Alt düzey görevler gerçekleştirmesini [tanılama günlüklerinin toplanması](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Active Directory Federasyon Hizmetleri (AD FS) tümleştirme Microsoft Graph tümleştirme Kurulumu dağıtımdan sonra etki alanı adı sistemi (DNS) ileticiler ekleme gibi tümleşik sistemler için birçok dağıtım sonrası datacenter tümleştirme görevleri gerçekleştirmek için Sertifika döndürme, vb.
- Tümleşik bir sistemde geniş kapsamlı sorun giderme için geçici, üst düzey erişim elde etmek için destek çalışmak için.

CESARETLENDİRİCİ her eylem (ve karşılık gelen çıktısını) PowerShell oturumunda gerçekleştiren kaydeder. Bu, tam saydamlığı ve işlemlerini tam denetim sağlar. Gelecekteki denetimleri için bu günlük dosyaları koruyabilirsiniz.

> [!NOTE]
> Azure yığın Geliştirme Seti (ASDK içinde), bazı CESARETLENDİRİCİ kullanılabilir komutlar doğrudan PowerShell oturumundan Geliştirme Seti konakta çalıştırabilirsiniz. Ancak, bu tek yöntem tümleşik sistemleri ortamında belirli işlemlerin gerçekleştirilmesi kullanılabilir olduğundan günlük toplama gibi CESARETLENDİRİCİ kullanarak bazı işlemleri test etmek isteyebilirsiniz.

## <a name="access-the-privileged-endpoint"></a>Ayrıcalıklı uç noktasına erişmek

CESARETLENDİRİCİ CESARETLENDİRİCİ barındıran sanal makinede uzak PowerShell oturumu aracılığıyla erişebilirsiniz. İçinde ASDK, bu sanal makine adlı **AzS ERCS01**. Tümleşik bir sistem kullanıyorsanız, her çalıştırma CESARETLENDİRİCİ üç örneği vardır bir sanal makine içinde (*önek*-ERCS01, *önek*-ERCS02, veya *önek*- ERCS03) dayanıklılık için farklı konaklarda. 

Tümleşik bir sistem için bu yordama başlamadan önce IP adresi veya DNS aracılığıyla CESARETLENDİRİCİ erişebildiğinden emin olun. Azure yığın ilk dağıtımdan sonra DNS tümleştirme henüz ayarlanmamış olduğundan yalnızca IP adresine göre CESARETLENDİRİCİ erişebilir. OEM donanım satıcınıza adlı bir JSON dosyası sağlayacak **AzureStackStampDeploymentInfo** CESARETLENDİRİCİ IP adreslerini içerir.


> [!NOTE]
> Güvenlik nedenleriyle, biz, CESARETLENDİRİCİ yalnızca donanım yaşam döngüsü ana bilgisayar üzerinde çalışan bir sağlamlaştırılmış sanal makine veya ayrılmış, güvenli bir bilgisayar gibi bağlanmasını gerektirecek bir [ayrıcalıklı erişim iş istasyonu](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations). Yeni yazılım yükleme de dahil olmak üzere kendi özgün yapılandırmadan sonra özgün yapılandırmanın donanım yaşam döngüsü konağının değiştirilmemelidir veya CESARETLENDİRİCİ bağlanmak için kullanılmalıdır.

1. Güven oluşturun.

    - Tümleşik bir sistemde CESARETLENDİRİCİ donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu üzerinde çalışan sağlamlaştırılmış sanal makineye güvenilen bir konak olarak eklemek için yükseltilmiş bir Windows PowerShell oturumunda aşağıdaki komutu çalıştırın.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - ADSK çalıştırıyorsanız, Geliştirme Seti ana bilgisayara oturum açın.

2. Sağlamlaştırılmış donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonunda çalıştıran sanal makine üzerinde bir Windows PowerShell oturumu açın. CESARETLENDİRİCİ barındıran sanal makinede uzak oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
    - Tümleşik bir sistemde:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS> `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      `ComputerName` Parametresi, IP adresini veya DNS adını CESARETLENDİRİCİ barındıran sanal makinelerden biri olabilir. 
    - ADSK çalıştırıyorsanız:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01 `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   İstendiğinde, aşağıdaki kimlik bilgilerini kullanın:

      - **Kullanıcı adı**: CloudAdmin hesabı biçiminde belirtin  **&lt; *Azure yığın etki alanı*&gt;\cloudadmin**. (ASDK için kullanıcı adı. **azurestack\cloudadmin**.)
      - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

    > [!NOTE]
    > ERCS uç noktasını bağlamak adımları birinci ve ikinci bir ERCS olduğu zaten bağlanmaya çalıştığınız henüz VM IP adresiyle yeniden deneyin.

3.  Bağlandıktan sonra istemi değiştirir **[*IP adresi veya ERCS VM adı*]: PS >** veya **[azs-ercs01]: PS >** ortamına bağlı olarak. Buradan, çalıştırmak `Get-Command` kullanılabilir cmdlet'lerinin listesini görüntülemek için.

    Bu cmdlet'lerin çoğu yalnızca tümleşik sistemi ortamları (örneğin, veri merkezi tümleştirmesiyle ilgili cmdlet'leri) yöneliktir. Aşağıdaki cmdlet ASDK içinde doğrulandı:

    - Clear-ana bilgisayar
    - Kapat PrivilegedEndpoint
    - Çıkış-PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get-Command
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Ölçü nesnesi
    - CloudAdminUser yeni
    - Dışarı varsayılan
    - Remove-CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Test-AzureStack
    - Stop-AzureStack
    - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktası kullanımıyla ilgili ipuçları 

Yukarıda belirtildiği gibi CESARETLENDİRİCİ olan bir [PowerShell JEA](https://docs.microsoft.com/powershell/jea/overview) uç noktası. Güçlü bir güvenlik katmanı sağlarken JEA endpoint bazı komut dosyası veya sekme tamamlama gibi temel PowerShell özelliklerini azaltır. Komut dosyası işlemi herhangi bir türde çalışırsanız, işlemi şu hata ile başarısız oluyor **ScriptsNotAllowed**. Bu beklenen bir davranıştır.

Bu nedenle, örneğin, belirli bir cmdlet için parametre listesini almak için aşağıdaki komutu çalıştırın:

```PowerShell
    Get-Command <cmdlet_name> -Syntax
```

Alternatif olarak, kullanabileceğiniz [alma-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) yerel makinenizdeki geçerli oturuma tüm CESARETLENDİRİCİ cmdlet'lerini içeri aktarmak için cmdlet. Bunu yaparak, tüm cmdlet'ler ve CESARETLENDİRİCİ işlevlerini artık genel olarak, komut dosyası yerel makinenizde, sekme tamamlama ve daha fazla ile birlikte kullanılabilir. 

Yerel makinenizde CESARETLENDİRİCİ oturum içeri aktarmak için aşağıdaki adımları uygulayın:

1. Güven oluşturun.

    -Bir tümleşik sistemde CESARETLENDİRİCİ donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu üzerinde çalışan sağlamlaştırılmış sanal makineye güvenilen bir konak olarak eklemek için yükseltilmiş bir Windows PowerShell oturumunda aşağıdaki komutu çalıştırın.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - ADSK çalıştırıyorsanız, Geliştirme Seti ana bilgisayara oturum açın.

2. Sağlamlaştırılmış donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonunda çalıştıran sanal makine üzerinde bir Windows PowerShell oturumu açın. CESARETLENDİRİCİ barındıran sanal makinede uzak oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
    - Tümleşik bir sistemde:
      ````PowerShell
        $cred = Get-Credential

        $session = New-PSSession -ComputerName <IP_address_of_ERCS> `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      `ComputerName` Parametresi, IP adresini veya DNS adını CESARETLENDİRİCİ barındıran sanal makinelerden biri olabilir. 
    - ADSK çalıştırıyorsanız:
     
      ````PowerShell
       $cred = Get-Credential

       $session = New-PSSession -ComputerName azs-ercs01 `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   İstendiğinde, aşağıdaki kimlik bilgilerini kullanın:

      - **Kullanıcı adı**: CloudAdmin hesabı biçiminde belirtin  **&lt; *Azure yığın etki alanı*&gt;\cloudadmin**. (ASDK için kullanıcı adı. **azurestack\cloudadmin**.)
      - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

3. Yerel makinenize CESARETLENDİRİCİ oturum içe
    ````PowerShell 
        Import-PSSession $session
    ````
4. Şimdi, sekme tamamlama kullanın ve her zamanki gibi tüm işlevleri ve CESARETLENDİRİCİ cmdlet'leri ile yerel PowerShell oturumunuzu üzerinde Azure yığınının güvenlik tutumunu azalan olmadan komut dosyası yapın. Keyfini çıkarın!


## <a name="close-the-privileged-endpoint-session"></a>Ayrıcalıklı endpoint oturumunu kapatma

 Daha önce belirtildiği gibi her eylem (ve karşılık gelen çıktısını) PowerShell oturumunda gerçekleştiren CESARETLENDİRİCİ kaydeder. Kullanarak oturumu kapatmanız gerekir `Close-PrivilegedEndpoint` cmdlet'i. Bu cmdlet doğru uç nokta kapatır ve bir dış dosya paylaşımı bekletme için günlük dosyaları aktarır.

Uç nokta oturumu kapatmak için:

1. CESARETLENDİRİCİ tarafından erişilebilen bir harici dosya paylaşımı oluşturun. Bir geliştirme seti ortamında, yalnızca bir dosya paylaşımı Geliştirme Seti konakta oluşturabilirsiniz.
2. Çalıştırma `Close-PrivilegedEndpoint` cmdlet'i. 
3. Dökümü günlük dosyasının depolanacağı yol için istenir. Biçiminde daha önce oluşturduğunuz dosya paylaşımı belirtin &#92; &#92; *servername*&#92;*sharename*. Bir yol belirtmezseniz, cmdlet başarısız olur ve oturumu açık kalır. 

    ![Dökümü hedef yolu belirlediğiniz gösterir Kapat PrivilegedEndpoint cmdlet çıktısında](media/azure-stack-privileged-endpoint/closeendpoint.png)

Dökümü günlük dosyaları dosya paylaşımına başarıyla aktarıldıktan sonra otomatik olarak CESARETLENDİRİCİ silinmiş. 

> [!NOTE]
> Cmdlet'lerini kullanarak CESARETLENDİRİCİ oturumu kapatmak, `Exit-PSSession` veya `Exit`, veya PowerShell Konsolu kapatmanız yeterlidir, dökümü günlükleri için bir dosya paylaşımı transfer yok. CESARETLENDİRİCİ kalırlar. Sonraki çalıştırmanızda `Close-PrivilegedEndpoint` ve bir dosya paylaşımı içerir, önceki dökümü günlüklerinden oturumlara ayrıca aktarır. Kullanmayın `Exit-PSSession` veya `Exit` ; CESARETLENDİRİCİ oturumu kapatmaya kullanmak `Close-PrivilegedEndpoint` yerine.


## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın tanılama araçları](azure-stack-diagnostics.md)
