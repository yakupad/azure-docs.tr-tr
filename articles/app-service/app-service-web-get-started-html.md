---
title: Azure'da statik HTML web uygulaması oluşturma | Microsoft Docs
description: Statik bir HTML örnek uygulaması dağıtarak Azure App Service'te web uygulaması çalıştırmayı öğrenin.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: e48c2aceb2a8f45d01b922a186900780c1c5ef51
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38968765"
---
# <a name="create-a-static-html-web-app-in-azure"></a>Azure'da statik bir HTML web uygulaması oluşturma

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıç öğreticisinde, temel bir HTML+CSS sitesinin Azure'a nasıl dağıtılacağı gösterilmektedir. Bu hızlı başlangıcı [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)'de tamamlayacaksınız ama bu komutları [Azure CLI](/cli/azure/install-azure-cli) ile yerel olarak da çalıştırabilirsiniz.

![Örnek uygulamanın giriş sayfası](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="install-web-app-extension-for-cloud-shell"></a>Cloud Shell için web uygulama uzantısını yükleme

Bu hızlı başlangıcı tamamlamak için, [az web uygulama uzantısını](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-add) eklemelisiniz. Uzantı zaten yüklenmişse, bunu en son sürüme güncelleştirmeniz gerekir. Web uygulama uzantısını güncelleştirmek için `az extension update -n webapp` yazın.

Webapp uzantısını yüklemek için aşağıdaki komutu çalıştırın:

```bash
az extension add -n webapp
```

Uzantı yüklendiğinde, Cloud Shell aşağıda gösterilen örnekteki bilgileri gösterir:

```bash
The installed extension 'webapp' is in preview.
```

## <a name="download-the-sample"></a>Örneği indirme

Cloud Shell'de bir quickstart dizini oluşturun ve o dizine geçin.

```bash
mkdir quickstart

cd quickstart
```

Ardından, örnek uygulama deposunu quickstart dizininize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Örnek kodu içeren dizine geçin ve `az webapp up` komutunu çalıştırın.

Aşağıdaki komutta <app_name> kısmını benzersiz uygulama adıyla değiştirin.

```bash
cd html-docs-hello-world

az webapp up -n <app_name>
```

`az webapp up` komutu şu eylemleri gerçekleştirir:

- Varsayılan kaynak grubunu oluşturur.

- Bir varsayılan uygulama hizmeti planı oluşturur.

- Belirtilen adla bir uygulama oluşturur.

- Dosyaları geçerli çalışma dizininden web uygulamasına [sıkıştırıp dağıtır](https://docs.microsoft.com/azure/app-service/app-service-deploy-zip).

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:

```json
{
  "app_url": "https://<app_name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app_name>",
  "os": "Windows",
  "resourcegroup": "appsvc_rg_Windows_CentralUS ",
  "serverfarm": "appsvc_asp_Windows_CentralUS",
  "sku": "FREE",
  "src_path": "/home/username/quickstart/html-docs-hello-world ",
  < JSON data removed for brevity. >
}
```

`resourceGroup` değerini not edin. [Kaynakları temizleme](#clean-up-resources) bölümünde ihtiyacınız olacak.

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Bir tarayıcıda Azure web uygulaması URL'sine gidin: `http://<app_name>.azurewebsites.net`.

Sayfa bir Azure App Service web uygulaması çalıştırıyor.

![Örnek uygulama ana sayfası](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Tebrikler!** App Service'e ilk HTML uygulamanızı dağıttınız.

## <a name="update-and-redeploy-the-app"></a>Uygulamayı güncelleştirme ve yeniden dağıtma

Cloud Shell'de, nano metin düzenleyicisini açmak için `nano index.html` yazın. Aşağıda gösterildiği gibi H1 başlığında "Azure App Service - Sample Static HTML Site" yerine "Azure App Service" yazın.

![Nano index.html](media/app-service-web-get-started-html/nano-index-html.png)

Değişikliklerinizi kaydedin ve nanodan çıkın. Kaydetmek için `^O` ve çıkmak için `^X` komutunu kullanın.

Şimdi aynı `az webapp up` komutuyla uygulamayı yeniden dağıtacaksınız.

```bash
az webapp up -n <app_name>
```

Dağıtım tamamlandıktan sonra **Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Güncelleştirilen örnek uygulama giriş sayfası](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Yeni Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-html/portal1.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portalında App Service dikey penceresi](./media/app-service-web-get-started-html/portal2.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Bu kaynakların gelecekte gerekli olacağını düşünmüyorsanız, Cloud Shell’de aşağıdaki komutu çalıştırarak kaynak grubunu silin. Kaynak grubu adının [web uygulaması oluşturma](#create-a-web-app) adında otomatik olarak oluşturulduğunu unutmayın.

```bash
az group delete --name appsvc_rg_Windows_CentralUS
```

Bu komutun çalıştırılması bir dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel etki alanı eşleme](app-service-web-tutorial-custom-domain.md)
