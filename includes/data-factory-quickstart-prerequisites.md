## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

### <a name="azure-roles"></a>Azure rolleri
Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, *katkıda bulunan*, *sahip* veya *yönetici* rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır. Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin, sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için [Rol ekleme](../articles/billing/billing-add-change-azure-subscription-administrator.md) makalesine bakın.

### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu hızlı başlangıçta, genel amaçlı Azure depolama hesabını (özel olarak Blob depolama) hem *kaynak* hem de *hedef* veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). 

#### <a name="get-the-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu hızlı başlangıçta, Azure depolama hesabınızın adını ve anahtarını kullanırsınız. Aşağıdaki yordamda, depolama hesabınızın adını ve anahtarını alma adımlarını verilmiştir: 

1. Bir web tarayıcısında [Azure portalına](https://portal.azure.com) gidin. Azure kullanıcı adınızı ve parolanızı kullanarak oturum açın. 
2. Sol taraftaki menüde **Diğer hizmetler**’i seçin, **Depolama** anahtar sözcüğünü kullanarak filtre uygulayın ve **Depolama hesapları**'nı seçin.

   ![Depolama hesabı arama](media/data-factory-quickstart-prerequisites/search-storage-account.png)
3. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse) ve depolama hesabınızı seçin. 
4. **Depolama hesabı** sayfasında, menüden **Erişim anahtarları**'nı seçin.

   ![Depolama hesabı adını ve anahtarını alma](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. **Depolama hesabı adı** ve **key1** kutularının değerlerini panoya kopyalayın. Bunları Not Defteri’ne veya başka bir düzenleyiciye yapıştırın ve kaydedin. Bunları daha sonra bu hızlı başlangıçta kullanacaksınız.   

#### <a name="create-the-input-folder-and-files"></a>Giriş klasörünü ve dosyaları oluşturma
Bu bölümde, Azure Blob depolama alanında **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz. Kapsayıcıda **giriş** adlı bir klasör oluşturur ve giriş klasörüne örnek bir dosya yüklersiniz. 

1. **Depolama hesabı** sayfasında **Genel Bakış**’a geçin ve sonra **Bloblar**’ı seçin. 

   ![Bloblar seçeneğini belirleyin](media/data-factory-quickstart-prerequisites/select-blobs.png)
2. **Blob hizmeti** sayfasında, araç çubuğundaki **+ Kapsayıcı**’yı seçin. 

   ![Kapsayıcı ekle düğmesi](media/data-factory-quickstart-prerequisites/add-container-button.png)    
3. **Yeni kapsayıcı** iletişim kutusunda ad olarak **adftutorial** girin ve ardından **Tamam**’ı seçin. 

   ![Kapsayıcı adını girin](media/data-factory-quickstart-prerequisites/new-container-dialog.png)
4. Kapsayıcılar listesinde **adftutorial**’ı seçin. 

   ![Kapsayıcıyı seçin](media/data-factory-quickstart-prerequisites/seelct-adftutorial-container.png)
1. **Kapsayıcı** sayfasında araç çubuğundaki **Karşıya Yükle** öğesini seçin.  

   ![Karşıya yükle düğmesi](media/data-factory-quickstart-prerequisites/upload-toolbar-button.png)
6. **Blobu karşıya yükleme** sayfasında **Gelişmiş**’i seçin.

   ![Gelişmiş bağlantı seçeneğini belirleme](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
7. **Not Defteri**'ni başlatın ve aşağıdakileri içeren **emp.txt** adlı bir dosya oluşturun: Bu dosyayı **c:\ADFv2QuickStartPSH** klasörüne kaydedin. Yoksa **ADFv2QuickStartPSH** klasörünü oluşturun.
    
   ```
   John, Doe
   Jane, Doe
   ```    
8. Azure portalında **Blobu karşıya yükleme** sayfasından **Dosyalar** kutusu için **emp.txt** dosyasına göz atıp seçin. 
9. **Klasöre yükle** kutusuna **input** değerini girin. 

    ![Blobu karşıya yükleme ayarları](media/data-factory-quickstart-prerequisites/upload-blob-settings.png)    
10. Klasörün **input**, dosyanın ise **emp.txt** olduğunu onaylayıp **Karşıya Yükle**’yi seçin.
    
    Listede **emp.txt** dosyasını ve karşıya yükleme durumunu görmeniz gerekir. 
12. Köşedeki **X** simgesine tıklayarak **Blobu karşıya yükleme** sayfasını kapatın. 

    ![Blobu karşıya yükleme sayfasını kapatma](media/data-factory-quickstart-prerequisites/close-upload-blob.png)
1. **Kapsayıcı** sayfasını açık tutun. Bu hızlı başlangıcın sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız.