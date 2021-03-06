---
title: Azure aboneliklerini ve hesaplarını etkinleştirme | Microsoft Docs
description: Yeni ve mevcut hesaplar için Azure Resource Manager API'lerini kullanarak erişimi etkinleştirin ve ortak hesap sorunlarını çözün.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 4a5e613169bf3173b7585b49803fc7ac7f5186ce
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297980"
---
# <a name="activate-azure-subscriptions-and-accounts-with-azure-cost-management"></a>Azure Maliyet Yönetimi ile Azure aboneliklerini ve hesaplarını etkinleştirme

Azure Resource Manager kimlik bilgilerinizi eklemeniz veya güncelleştirmeniz, Azure Maliyet Yönetimi’nin Azure Kiracınızdaki tüm hesapları ve abonelikleri keşfetmesini sağlar. Sanal makinelerinizde Azure Tanılama da etkinse, Azure Maliyet Yönetimi, CPU ve bellek gibi genişletilmiş ölçümleri toplayabilir. Bu makalede, yeni ve mevcut hesaplar için Azure Resource Manager API'leri kullanılarak erişimin nasıl etkinleştirileceği açıklanmaktadır. Ayrıca genel hesap sorunlarının nasıl çözümleneceği de açıklanmaktadır.

Azure Maliyet Yönetimi, abonelik _etkinleştirilmemiş_ olduğunda Azure abonelik verilerinizin çoğuna erişemez. Azure Maliyet Yönetimi’nin erişebilmesi için, _etkinleştirilmemiş_ hesapları düzenlemeniz gerekir.

## <a name="required-azure-permissions"></a>Gerekli Azure izinleri

Bu makaledeki yordamları tamamlamak için belirli izinler gerekir. Sizin veya kiracı yöneticinizin aşağıdaki izinlerin ikisine de sahip olmanız gerekir:

- CloudynCollector uygulamasını Azure AD kiracınıza kaydetme izni.
- Azure aboneliklerinizdeki bir role uygulamayı atayabilme özelliği.

Azure aboneliklerinizde hesaplarınızın CloudynCollector uygulamasını atamak için `Microsoft.Authorization/*/Write` erişimine sahip olması gerekir. Bu eylemin izni, [Sahip](../role-based-access-control/built-in-roles.md#owner) rolüyle veya [Kullanıcı Erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) rolüyle verilir.

Hesabınıza **Katkıda Bulunan** rolü atandıysa, uygulamayı atamak için yeterli izniniz yoktur. Azure aboneliğinize CloudynCollector uygulamasını atamaya çalışırken bir hata alırsınız.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleme

1. [Azure portalda](https://portal.azure.com) oturum açın.
2. Azure portalında **Azure Active Directory** seçeneğini belirleyin.
3. Azure Active Directory’de **Kullanıcı ayarları**’nı seçin.
4. **Uygulama kayıtları** seçeneğini işaretleyin.
    - Bu **Evet** olarak ayarlanırsa, yönetici olmayan kullanıcılar AD uygulamalarını kaydedebilir. Bu ayar, Azure AD kiracısı içindeki herhangi bir kullanıcının bir uygulamayı kaydedebileceği anlamına gelir. Gereken Azure aboneliği izinleriyle devam edebilirsiniz.  
    ![Uygulama kayıtları](./media/activate-subs-accounts/app-register.png)
    - **Uygulama kayıtları** seçeneği **Hayır** olarak ayarlanırsa, yalnızca kiracı yönetici kullanıcıları Azure Active Directory uygulamalarını kaydedebilir. Kiracı yöneticinizin, CloudynCollector uygulamasını kaydetmesi gerekir.


## <a name="add-an-account-or-update-a-subscription"></a>Hesap ekleme veya aboneliği güncelleştirme

Bir hesap eklediğinizde veya bir aboneliği güncelleştirdiğinizde, Azure Maliyet Yönetimi’ne Azure verilerinize erişme izni verirsiniz.

### <a name="add-a-new-account-subscription"></a>Yeni hesap ekleme (abonelik)

1. Azure Maliyet Yönetimi portalında, sağ üst kısımdaki dişli simgesine tıklayın ve **Bulut Hesapları**’nı seçin.
2. **Yeni hesap ekle**’ye tıklayın, böylece **Yeni hesap ekle** kutusu görüntülenir. Gerekli bilgileri girin.  
    ![Yeni hesap ekle kutusu](./media/activate-subs-accounts//add-new-account.png)

### <a name="update-a-subscription"></a>Aboneliği güncelleştirme

1. Azure Maliyet Yönetimi’nde Hesap Yönetimi’nde önceden mevcut olan _etkinleştirilmemiş_ bir aboneliği güncelleştirmek istiyorsanız, üst _kiracı GUID_’inin sağındaki düzenle kalem simgesine tıklayın. Abonelikler bir üst kiracı altında gruplanır, bu nedenle abonelikleri tek tek etkinleştirmekten kaçının.
    ![Abonelikleri yeniden keşfetme](./media/activate-subs-accounts/existing-sub.png)
2. Gerekirse Kiracı Kimliğini girin. Kiracı Kimliğinizi bilmiyorsanız, bulmak için aşağıdaki adımları kullanın:
    1. [Azure Portal](https://portal.azure.com) oturum açın.
    2. Azure portalında **Azure Active Directory** seçeneğini belirleyin.
    3. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.
    4. Dizin Kimliği GUID’ini kopyalayın. Bu değer kiracı kimliğinizdir.
    Daha fazla bilgi için bkz. [Kiracı kimliğini alma](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
3. Gerekirse, Ücret Kimliğinizi seçin. Ücret kimliğinizi bilmiyorsanız, bulmak için aşağıdaki adımları kullanın.
    1. Azure portalının sağ üst kısmında kullanıcı bilgilerinize tıklayın ve sonra **Faturamı görüntüle**’ye tıklayın.
    2. **Faturalama Hesabı** bölümünde **Abonelikler**’e tıklayın.
    3. **Aboneliklerim** bölümünde aboneliği seçin.
    4. Ücret kimliğiniz, **Teklif kimliği** bölümünde gösterilir. Abonelik için Teklif Kimliğini kopyalayın.
4. Yeni hesap ekle (veya Aboneliği Düzenle) kutusunda, **Kaydet**’e (veya **İleri**’ye) tıklayın. Azure portalına yeniden yönlendirilirsiniz.
5. Portalda oturum açın. **Kabul Et**’e tıklayarak Azure Maliyet Yönetimi Toplayıcı’ya Azure hesabınıza erişme yetkisi verin.

    Azure Maliyet Yönetimi Hesapları yönetim sayfasına yeniden yönlendirilirsiniz ve aboneliğiniz, **etkin** Hesap Durumu ile güncelleştirilir. Resource Manager sütununun altında yeşil bir onay işareti simgesi görüntülenmelidir.

    Aboneliklerden biri veya daha fazlası için yeşil bir onay işareti simgesi görmezseniz bu, abonelik için okuyucu uygulamasını (CloudynCollector) oluşturma izninizin olmadığı anlamına gelir. Abonelik için daha yüksek izinleri olan bir kullanıcının bu işlemi yinelemesi gerekir.

İşlem boyunca adım adım yol gösteren [Azure Maliyet Yönetimi ile Azure Resource Manager’a Bağlanma](https://youtu.be/oCIwvfBB6kk) videosunu izleyin.

>[!VIDEO https://www.youtube.com/embed/oCIwvfBB6kk?ecver=1]

## <a name="resolve-common-indirect-enterprise-set-up-problems"></a>Genel dolaylı kurumsal kurulum sorunlarını çözümleme

Azure Maliyet Yönetimi portalını ilk kullandığınızda, Kurumsal Anlaşma veya Bulut Çözümü Sağlayıcı (CSP) kullanıcısıysanız şu iletileri görebilirsiniz:

- **Azure Maliyet Yönetimi’ni Ayarlama** sihirbazında *Belirtilen API anahtarı, üst düzey bir kayıt anahtarı değil* iletisi görüntülenir.
- Kurumsal Anlaşma portalında *Doğrudan Kayıt - Hayır* görüntülenir.
- Azure Maliyet Yönetimi portalında *Son 30 gün için kullanım verisi bulunamadı. Azure Maliyet Yönetimi portalında, Azure hesabınız için işaretlemenin etkinleştirildiğinden emin olmak için lütfen dağıtımcınızla görüşün* iletisi görüntülenir.

Önceki ileti, bir kurumsal bayi veya CSP aracılığıyla Azure Kurumsal Anlaşma satın aldığınızı belirtir. Azure Maliyet Yönetimi’nde verilerinizi görüntüleyebilmeniz için satıcınızın veya CSP’nin Azure hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir.

Sorunların çözümü:

1. Kurumsal bayinin hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir. Yönergeler için bkz. [Dolaylı Müşteri Ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).
2. Azure Maliyet Yönetimi ile kullanmak üzere Azure Kurumsal Anlaşma anahtarını oluşturursunuz. Yönergeler için bkz. [Azure Kurumsal Anlaşma kaydetme ve maliyet verilerini görüntüleme](https://docs.microsoft.com/azure/cost-management/quick-register-ea).

Azure Maliyet Yönetimi’ni ayarlamak için Azure Kurumsal Anlaşma API anahtarını oluşturabilmeniz için aşağıda belirtilen kaynaklarda yer alan yönergeleri izleyerek Azure Faturalama API’sini etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için Raporlama API’lerine genel bakış](../billing/billing-enterprise-api.md)
- **API’lere veri erişimini etkinleştirme** bölümünde [Microsoft Azure kurumsal portal Raporlama API’si](https://ea.azure.com/helpdocs/reportingAPI)

Departman yöneticilerine, hesap sahiplerine ve kurumsal yöneticilere Faturalama API’si ile _ücretleri görüntüleme_ izni vermeniz de gerekebilir.

Yalnızca bir Azure hizmet yöneticisi Maliyet Yönetimini etkinleştirebilir. Ortak yönetici izinleri yeterli değil. Ancak, yönetici gereksinimine geçici bir çözüm sunabilirsiniz. Azure Active Directory yöneticinizin PowerShell betiği ile **CloudynAzureCollector**’ı yetkilendirme izni vermesini isteyebilirsiniz. Aşağıdaki betik, **CloudynAzureCollector** adlı Azure Active Directory Hizmet Sorumlusu’nu kaydetme izni verir.

```
#THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

#Tenant - enter your tenant ID or Name
$tenant = "<ReplaceWithYourTenantID>"

#Cloudyn Collector application ID
$appId = "83e638ef-7885-479f-bbe8-9150acccdb3d"

#URL to activate the consent screen
$url = "https://login.windows.net/"+$tenant+"/oauth2/authorize?api-version=1&response_type=code&client_id="+$appId+"&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2FCloudynJava&prompt=consent"

#Choose your browser, the default is Internet Explorer

#Chrome
#[System.Diagnostics.Process]::Start("chrome.exe", "--incognito $url")

#Firefox
#[System.Diagnostics.Process]::Start("firefox.exe","-private-window $url" )

#IExplorer
[System.Diagnostics.Process]::Start("iexplore.exe","$url -private" )

```

## <a name="next-steps"></a>Sonraki adımlar

- Maliyet Yönetimi için ilk öğreticiyi önceden tamamlamadıysanız, [Kullanım ve maliyetleri gözden geçirme](tutorial-review-usage.md) bölümünden bilgi edinin.
