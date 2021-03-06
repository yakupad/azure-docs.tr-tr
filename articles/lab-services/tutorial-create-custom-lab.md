---
title: Azure DevTest Labs kullanarak bir laboratuvar oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, Azure DevTest Labs kullanarak bir laboratuvar oluşturursunuz.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 5a93feec7996fc0ebf742b8d62b159dca5f1c1ed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636996"
---
# <a name="tutorial-set-up-a-lab-by-using-azure-devtest-labs"></a>Öğretici: Azure DevTest Labs kullanarak bir laboratuvar ayarlama
Bu öğreticide, Azure portalı kullanarak bir laboratuvar oluşturursunuz. Laboratuvar yöneticisi bir kuruluşta laboratuvar ayarlar, laboratuvarda sanal makineler oluşturur ve ilkeler yapılandırır. Laboratuvar kullanıcıları (örneğin: geliştirici ve test ediciler), laboratuvarda sanal makineler talep eder, sanal makinelere bağlanır ve sanal makineleri kullanır. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar oluşturma
> * Laboratuvara sanal makineler (VM) ekleme
> * Laboratuvar Kullanıcısı rolüne kullanıcı ekleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-lab"></a>Laboratuvar oluşturma
Aşağıdaki adımlar, Azure portal kullanarak Azure DevTest Labs’de nasıl bir laboratuvar oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki ana menüden **Kaynak oluştur**’u (listenin en üstünde) seçin, **Geliştirici araçları**’nın üzerine gelin ve **DevTest Labs** seçeneğine tıklayın. 

    ![Yeni DevTest Laboratuvarı menüsü](./media/tutorial-create-custom-lab/new-custom-lab-menu.png)
1. **DevTest Laboratuvarı Oluştur** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar adı** alanına laboratuvar için bir ad girin. 
    2. **Abonelik** alanına, laboratuvarı oluşturmak istediğiniz aboneliği seçin. 
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin. 
    4. **Konum** alanında, laboratuvarın oluşturulmasını istediğiniz konumu/bölgeyi seçin. 
    5. **Oluştur**’u seçin. 
    6. **Panoya sabitle**’yi seçin. Laboratuvarı oluşturduktan sonra laboratuvar, panoda gösterilir. 

        ![DevTest Labs laboratuvar bölümü oluşturma](./media/tutorial-create-custom-lab/create-custom-lab-blade.png)

## <a name="add-a-vm-to-the-lab"></a>Laboratuvara bir sanal makine ekleme

1. **DevTest Lab** sayfasında, araç çubuğunda **+Ekle**’yi seçin. 

    ![Ekle düğmesi](./media/tutorial-create-custom-lab/add-vm-to-lab-button.png)
1. **Temel seçin** sayfasında, **Ubuntu** anahtar sözcüğüyle arama yapın ve listedeki temel görüntülerden birini seçin. 
1. **Sanal makine** sayfasında aşağıdaki eylemleri gerçekleştirin: 
    1. **Sanal makine adı** alanına, sanal makine için bir ad girin. 
    2. **Kullanıcı adı** alanına, sanal makineye erişimi olan kullanıcı için bir ad girin. 
    3. **Bir değer yazın** alanına, kullanıcının parolasını girin. 
    4. **Gelişmiş ayarlar**’ı seçin.
    5. **Bu makineyi talep edilebilir yap** için **Evet**’i seçin.
    6. **Örnek sayısı** değerinin **1** olarak ayarlandığını onaylayın. **2** olarak ayarlarsanız `<base image name>00' and <base image name>01` adlarıyla 2 sanal makine oluşturulur. Örneğin: `win10vm00` ve `win10vm01`. 
    7. **Gelişmiş** sayfasını kapatmak için **Tamam**’a tıklayın. 
    8. **Oluştur**’u seçin. 

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-virtual-machine.png)
    9. **Talep edilebilir sanal makineler** listesinde sanal makinenin durumunu görürsünüz. Sanal makine oluşturulması yaklaşık 25 dakika sürebilir. Adı, laboratuvarı içeren geçerli kaynak grubunun adıyla başlayan sanal makine ayrı bir Azure kaynak grubunda oluşturulur. Örneğin, laboratuvar `labrg` ise sanal makine, `labrg3988722144002` kaynak grubunda oluşturulabilir. 

        ![Sanal makine oluşturma durumu](./media/tutorial-create-custom-lab/vm-creation-status.png)
1. Sanal makine oluşturulduktan sonra, **Talep edilebilir sanal makineler** listesinde bu sanal makineyi görürsünüz. 

## <a name="add-a-user-to-the-lab-user-role"></a>Laboratuvar Kullanıcısı rolüne kullanıcı ekleme

1. Soldaki menüden **Yapılandırma ve ilkeler**’i seçin. 

    ![Yapılandırma ve ilkeler](./media/tutorial-create-custom-lab/configuration-and-policies-menu.png)
1. Menüden **Erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğundan **+ Ekle**’yi seçin. 

    ![Erişim denetimi - Kullanıcı ekle düğmesi](./media/tutorial-create-custom-lab/access-control-add.png)
1. **İzin ekle** sayfasında aşağıdaki eylemleri gerçekleştirin:
    1. **Rol** için **DevTest Labs Kullanıcısı**’nı seçin. 
    2. Eklemek istediğiniz **kullanıcıyı** seçin. 
    3. **Kaydet**’i seçin.

        ![İzin ekleme](./media/tutorial-create-custom-lab/add-lab-user.png)
4. **Yapılandırma ve ilkeler - Erişim denetimi (IAM)** penceresini kapatmak için sağ köşedeki **X** simgesini seçin. 

## <a name="cleanup-resources"></a>Kaynakları temizleme
Sonraki öğreticide, bir laboratuvar kullanıcısının laboratuvardaki bir sanal makineyi nasıl talep edebileceği ve bu sanal makineye nasıl bağlanabileceği gösterilmektedir. Bu öğreticiyi uygulamak istemiyorsanız ve bu öğreticinin parçası olarak oluşturulan kaynakları temizlemek istiyorsanız aşağıdaki adımları izleyin: 

1. Azure portalında, menüden **Kaynak grupları**’nı seçin. 
2. Laboratuvarı oluşturduğunuz kaynak grubunuzu seçin. 
3. Araç çubuğundan **Kaynak grubunu sil**’i seçin. Bir kaynak grubu silindiğinde, laboratuvar dahil olmak üzere gruptaki tüm kaynaklar silinir. 
4. `<your resource group name><random numbers>` adıyla sizin için oluşturulan ek kaynak grubunu silmek için bu adımları yineleyin. Örneğin: `splab3988722144001`. Sanal makineler, laboratuvarın bulunduğu kaynak grubunda değil, bu kaynak grubunda oluşturulur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sanal makine ile laboratuvar oluşturdunuz ve bir kullanıcıya bu laboratuvara erişme izni verdiniz. Laboratuvar kullanıcısı olarak laboratuvara erişme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Öğretici: Laboratuvara erişme](tutorial-use-custom-lab.md)

