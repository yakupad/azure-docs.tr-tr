---
title: Azure Hızlı Başlangıç - Java Depolama SDK’sı V10 kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, Java Depolama SDK'sı kullanarak nesne (Blob) depolamasında kapsayıcı oluşturur, dosya karşıya yükler, nesneleri listeler ve indirirsiniz.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: rogarana
ms.openlocfilehash: a789269e73e1817f6a45e1e5948dbfaa21efd283
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704479"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-java-sdk-v10-preview"></a>Hızlı başlangıç: Java SDK V10 (Önizleme) kullanarak blobları karşıya yükleme, indirme ve listeleme

Bu hızlı başlangıçta, Azure Blob depolamadaki bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için yeni Java Depolama SDK’sını nasıl kullanabileceğinizi öğreneceksiniz. Yeni Java SDK'sı RxJava ile Duyarlı programlama modelini kullanarak size zaman uyumsuz işlemler sağlar. [Buradan](https://github.com/ReactiveX/RxJava) RxJava hakkında daha fazla bilgi edinin. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* [Maven](http://maven.apache.org/download.cgi)’ı yükleme ve komut satırından veya tercih ettiğiniz herhangi bir Java IDE’den çalışacak şekilde yapılandırma
* [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)’yı yükleme ve yapılandırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan [örnek uygulama](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart), temel bir konsol uygulamasıdır. 

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar.

Proje içeri aktarmayı bitirdiğinde **Quickstart.java**’yı açın (**src/main/java/quickstart** konumunda yer alır).

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
Bu çözüm, depolama hesabı adınızın ve anahtarınızın çözümü çalıştıran makinede yerel olarak bulunan ortam değişkenlerinde güvenli bir şekilde depolanmasını gerektirir. Ortam değişkenlerini oluşturmak için işletim sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

### <a name="for-linux"></a>Linux için

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="for-windows"></a>Windows için

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek, varsayılan dizininizde (Windows kullanıcıları için AppData\Local\Temp) bir test dosyası oluşturur, ardından test dosyasını Blob depolamaya yükleyecek komutları girmenizi ister, kapsayıcıdaki blobları listeler ve eski ile yeni dosyaları karşılaştırabilmeniz için karşıya yüklenmiş dosyayı yeni bir adla indirir. 

Örneği komut satırındaki Maven ile çalıştırmak istiyorsanız bir kabuğu açın ve kopyalanan dizininizdeki **storage-blobs-java-v10-quickstart**’a gidin. Sonra `mvn compile exec:java` komutunu girin.

Aşağıda, uygulamayı Windows’da çalıştırmak istemeniz durumunda karşılaşacağınız çıktının bir örneği verilmiştir.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Örneğin denetimi sizdedir; bu nedenle, kodu yürütmesini sağlamak için komutları girin. Girişlerin büyük/küçük harfe duyarlı olduğunu unutmayın.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için **E** ve Enter tuşuna basın. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için **Quickstart.java** dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyelim.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir - her bir dosya, listede yanında yer alan dosya tarafından kullanılır.

* StorageURL nesnesinin depolama hesabına işaret eden bir örneğini oluşturun.

    [**StorageURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-preview) nesnesi depolama hesabınızı temsil eder ve yeni işlem hattı oluşturmanızı sağlar. İşlem hattı ([HTTP İşlem Hattı](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline)), yetkilendirme, günlüğe kaydetme ve yeniden deneme mekanizmalarıyla istekleri ve yanıtları işlemek için kullanılan bir dizi ilkedir. İşlem hattı ile [**ServiceURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-preview) nesnesinin bir örneğini oluşturabilirsiniz; bu da blob kapsayıcılarında işlem çalıştırmak için gereken [**ContainerURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview) örneğini oluşturmanıza olanak tanır.

* ContainerURL nesnesinin, eriştiğiniz kapsayıcıyı temsil eden bir örneğini oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için kullanılır.

    **ContainerURL** kapsayıcı hizmetine bir erişim noktası sağlar. **ContainerURL** nesnesini kullanarak, blob oluşturmak için gereken [**BlobURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview) örneğini oluşturabilirsiniz.

* [BlobURL](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-preview) nesnesinin ilgilendiğiniz bloba işaret eden bir örneğini oluşturun.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma 

Bu bölümde, bir ContainerURL örneği ve bunun içinde de yeni bir kapsayıcı oluşturursunuz. Örnekteki kapsayıcının adı **quickstart**'tır. 

Örnek her çalıştırıldığında yeni bir kapsayıcı oluşturmak istediğimizden, bu örnekte [containerURL.create](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-preview) komutu kullanılır. Alternatif olarak, kapsayıcıyı önceden oluşturabilirsiniz. Böylece kapsayıcıyı kodda oluşturmanıza gerek kalmaz.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en sık kullanılan bloblardır ve bu hızlı başlangıçta bu bloblar kullanılmıştır. 

Dosyayı bloba yüklemek için, hedef kapsayıcıda bloba bir başvuru alın. Blob başvurunuz olduğunda, BlockBlobURL örneğinde [Upload](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-preview) (diğer adı PutBlob), [StageBlock](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-preview#com_microsoft_azure_storage_blob__block_blob_u_r_l_stageBlock_String_Flowable_ByteBuffer__long_LeaseAccessConditions_) (diğer adı PutBLock) gibi düşük düzeyli API'leri veya [TransferManager](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-preview) sınıfında sağlanan [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-preview) yöntemi gibi yüksek düzeyli API'leri kullanarak buna dosya yükleyebilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob zaten varsa blobun üzerine yazılır.

Örnek kod, karşıya yükleme ve indirme işlemleri için kullanılacak bir yerel dosya oluşturur. karşıya yüklenecek dosyayı **sourceFile**, blobun url’sini **blob** olarak depolama. Aşağıdaki örnek, dosyayı **quickstart** adlı kapsayıcınıza yükler.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {
    
    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());
            
    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Blok blobları herhangi bir metin veya ikili dosya türünde olabilir. Sayfa blobları öncelikli olarak IaaS VM'leri desteklemek için kullanılan VHD dosyalarında kullanılır. Ekleme blobları verilerin sona eklenmesine yöneliktir ve çoğunlukla günlüğe kaydetme için kullanılır. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[containerURL.listBlobsFlatSegment](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-preview) kullanarak kapsayıcıdaki nesnelerin listesini alabilirsiniz. Bu yöntem bir kerede en çok 5000 nesne ve listede daha fazla nesne varsa bir devamlılık işareti (ileri işareti) döndürür. Bunu yapmak için, önceki listBlobsFlatSegment yanıtında ileri işareti bulunduğu sürece tekrar tekrar kendini çağıran bir yardımcı işlevi oluştururuz.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs, 
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse -> 
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))    
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {                
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }
    
    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */
            
        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */
            
    return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[blobURL.download](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-preview) komutunu kullanarak blobları yerel diskinize indirin.

Aşağıdaki kod önceki bir bölümde karşıya yüklenmiş olan blobu indirir; yerel diskinizde her iki dosyayı da görebilmeniz için indirilen blobun adına "_DOWNLOADED" son ekini koyar. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, [containerURL.delete](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-preview) kullanarak kapsayıcının tamamını silebilirsiniz. Bu yöntem, kapsayıcıdaki dosyaları da siler.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları Java kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. 

> [!div class="nextstepaction"]
> [Java kaynak kodu için Depolama SDK V10](https://github.com/Azure/azure-storage-java/tree/New-Storage-SDK-V10-Preview)
> [API Başvurusu](https://docs.microsoft.com/en-us/java/api/storage/client?view=azure-java-preview)
> [RxJava hakkında daha fazla bilgi edinin](https://github.com/ReactiveX/RxJava)