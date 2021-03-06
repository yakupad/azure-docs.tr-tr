---
title: Bir SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için bir Windows VM'ye yönetilen kimliği kullanma
description: Azure depolama, depolama hesabı erişim anahtarı yerine SAS kimlik bilgisi kullanarak erişmek için bir Windows VM yönetilen hizmet kimliği kullanmayı gösteren öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 983cecdcdb95dca398f728dbdbe5feac69075d6a
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248379"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Öğretici: Azure depolama bir SAS kimlik bilgisi erişmek için bir Windows VM yönetilen hizmet kimliği kullanın.

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide bir Windows sanal makinesi için Yönetilen hizmet kimliği etkinleştirmek ve ardından depolama paylaşılan erişim imzası (SAS) kimlik bilgisi almak için Yönetilen hizmet kimliğini kullanma gösterilmektedir. Özellikle, bir [Hizmet SAS kimlik bilgileri](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Hizmet SAS, bir hesap erişim anahtarına sokmadan sınırlı süre için bir depolama hesabındaki nesnelere ve belirli bir hizmete (Bu örnekte, blob hizmeti), sınırlı erişim olanağı sağlar. Depolama işlemleri yaparken, örneğin Depolama SDK'sını kullanırken SAS kimlik bilgilerini olağan şekilde kullanabilirsiniz. Bu öğreticide, yükleme ve Azure Storage PowerShell kullanarak bir blob indirme gösterir. Şunları öğrenirsiniz:


> [!div class="checklist"]
> * Windows Sanal Makinesinde Yönetilen Hizmet Kimliği'ni etkinleştirme 
> * VM'nize Resource Manager'da yer alan depolama hesabı SAS için erişim verme 
> * VM'nizin kimliğini kullanarak erişim belirteci alma ve Resource Manager'dan SAS almak için bu belirteci kullanma 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Windows sanal makinesi oluşturma

Bu öğretici için, yeni bir Windows VM oluşturuyoruz. Mevcut VM'yi yönetilen hizmet kimliği de etkinleştirebilirsiniz.

1.  Azure portalın sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. Burada oluşturulan **Kullanıcı adı** ve **Parola**, sanal makinede oturum açmak için kullandığınız kimlik bilgileridir.
4.  Açılan listede sanal makine için uygun **Aboneliği** seçin.
5.  İçinde sanal makinenin oluşturulmasını istediğiniz yeni bir **Kaynak Grubu** seçmek için, **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alternatif resim metni](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Vm'nizde yönetilen hizmet kimliğini etkinleştirme

Bir sanal makine yönetilen hizmet kimliği, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. Arka planda, yönetilen hizmet kimliği etkinleştiriliyor iki şeyi yapar: yazmaçlar VM'nizi ve yönetilen kimliğini oluşturmak için Azure Active Directory ile kimlik VM'de yapılandırır.

1. Yeni sanal makinenizin kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. VM Sol paneldeki "ayarlar" altında tıklayın **yapılandırma**.
3. Kaydolun ve yönetilen hizmet kimliği etkinleştirmek için işaretleyin **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.

    ![Alternatif resim metni](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Henüz bir depolama hesabınız yoksa, şimdi oluşturacaksınız. Ayrıca, bu adımı atlayıp mevcut bir depolama hesabı SAS kimlik bilgisi, VM yönetilen hizmet kimliği erişim. 

1. Azure portalın sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. **Depolama**'ya ve **Depolama Hesabı**'na tıklayın; yeni bir "Depolama hesabı oluştur" paneli görüntülenir.
3. Daha sonra kullanacağınız depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **Hesap türü** sırasıyla "Kaynak yöneticisi" ve "Genel amaçlı" olarak ayarlanmalıdır. 
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluşturma](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında bir blob kapsayıcısı oluşturma

Daha sonra yeni depolama hesabına dosya yükleyecek ve indireceğiz. Dosyalar için blob depolaması gerektiğinden, dosyanın depolanacağı bir blob kapsayıcısı oluşturmalıyız.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Sol tarafta, "Blob hizmeti" öğesinin altındaki **Kapsayıcılar** bağlantısına tıklayın.
3. Sayfanın en üstündeki **+ Kapsayıcı**'ya tıklayın; "Yeni kapsayıcı" paneli belirir.
4. Kapsayıcıya bir ad verin, erişim düzeyini seçin ve ardından **Tamam**'a tıklayın. Belirttiğiniz ad bu öğreticide daha sonra kullanılacaktır. 

    ![Depolama kapsayıcısı oluşturma](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-managed-service-identity-access-to-use-a-storage-sas"></a>Bir SAS depolama kullanmak için sanal makinenin yönetilen hizmet kimliği erişim 

Azure Depolama Azure AD kimlik doğrulamayı yerel olarak desteklemez.  Ancak, Kaynak Yöneticisi'nden depolama SAS almak için bir yönetilen hizmet Kimliği'ni kullanın sonra depolamaya erişmek için SAS'ı kullanın.  Bu adımda, depolama hesabınıza SAS, VM yönetilen hizmet kimliği erişim verin.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.   
2. Sol bölmedeki **Erişim denetimi (IAM)** bağlantısına tıklayın.  
3. VM’nize yönelik yeni bir rol ataması eklemek için sayfanın üst kısmındaki **+ Ekle**’ye tıklayın.
4. Sayfanın sağ tarafında, **Rol** olarak "Depolama Hesabı Katılımcısı" seçeneğini ayarlayın.  
5. Sonraki açılan listede **Erişimin atanacağı hedef** olarak "Sanal Makine" seçeneğini ayarlayın.  
6. Ardından, uygun aboneliğin **Abonelik**’te listelendiğinden emin olun ve sonra **Kaynak Grubu**’nu "Tüm kaynak grupları" olarak ayarlayın.  
7. Son olarak, **Seç**'in altındaki açılan listede Windows Sanal Makinenizi seçin ve **Kaydet**'e tıklayın. 

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>VM kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma 

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacağız.

Bu bölümde Azure Resource Manager PowerShell cmdlet’lerini kullanmanız gerekir.  Bunu yüklemediyseniz, devam etmeden önce [en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/overview).

1. Azure portalında **Sanal Makineler**'e gidin, Windows sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın.
2. Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın. 
4. PowerShell'in Invoke-WebRequest kullanarak, Azure Resource Manager için bir erişim belirteci almak için yerel yönetilen hizmet kimliği uç noktasına bir istek olun.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > "Resource" parametre değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    
    Ardından, $response nesnesinde JavaScript Nesne Gösterimi (JSON) biçimlendirilmiş dizesi olarak depolanan “Content” öğesini ayıklayın. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Ardından, yanıttan erişim belirtecini ayıklayın.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan SAS kimlik bilgileri alma 

Artık depolama SAS kimlik bilgisi oluşturmak için önceki bölümde biz alınan erişim belirteci kullanarak Resource Manager'ı çağırmak için PowerShell kullanın. Biz SAS kimlik bilgilerini aldıktan sonra depolama işlemleri diyoruz.

Bu istek için, aşağıdaki HTTP istek parametrelerini kullanarak SAS kimlik bilgilerini oluşturacağız:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Bu parametreler SAS kimlik bilgileri için isteğin POST gövdesine eklenmiştir. SAS kimliği oluştururken kullanılan parametreler hakkında daha fazla bilgi için bkz. [Liste Hizmeti SAS REST başvurusu](/rest/api/storagerp/storageaccounts/listservicesas).

İlk olarak, Parametreler JSON biçimine dönüştür ve ardından depolama birimi çağrısı `listServiceSas` SAS oluşturmak için uç nokta kimlik bilgisi:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> URL büyük/küçük harfe duyarlıdır; dolayısıyla daha önce Kaynak Grubunu adlandırırken kullandığınız büyük/küçük harf düzenini kullanmaya dikkat edin ("resourceGroups" adındaki büyük "G" harfi dahil). 

Şimdi biz SAS kimlik bilgisi yanıttan ayıklayın:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

SAS kimlik bilgileri incelemek, şöyle bir şey görürsünüz:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Sonra "test.txt" adlı bir dosya oluştururuz. SAS kimlik bilgisi ile kimlik doğrulaması kullanacağınızı `New-AzureStorageContent` cmdlet'i, dosyanın bizim blob kapsayıcısına yükleyin ve ardından dosyayı indirin.

```bash
echo "This is a test text file." > test.txt
```

Önce, `Install-Module Azure.Storage` kullanarak Azure Depolama cmdlet'lerini yüklediğinizden emin olun. Sonra `Set-AzureStorageBlobContent` PowerShell cmdlet'ini kullanarak yeni oluşturduğunuz blobu karşıya yükleyin:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Yanıt:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Ayrıca `Get-AzureStorageBlobContent` PowerShell cmdlet'ini kullanarak yeni karşıya yüklediğiniz blobu da indirebilirsiniz:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Yanıt:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için Yönetilen hizmet kimliği oluşturma öğrendiniz.  Azure Depolama SAS hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Paylaşılan erişim imzaları (SAS) kullanma](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)


