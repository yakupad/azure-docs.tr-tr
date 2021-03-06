---
title: SQL barındırma sunucuları Azure Stack üzerinde | Microsoft Docs
description: SQL bağdaştırıcısı kaynak sağlayıcısı aracılığıyla sağlama SQL örnekleri ekleme.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: de2e1defeff9ab2dd78bdf019009b62955f73b88
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970560"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı için barındırma sunucuları ekleme

İçinde bir SQL örneğini bir sanal makine'de (VM) barındırabilir [Azure Stack](azure-stack-poc.md), veya bir VM'de SQL kaynak sağlayıcısı örneği bağlanabilir sürece, Azure Stack ortamınıza dışında.

## <a name="overview"></a>Genel Bakış

SQL barındırma sunucusu eklemeden önce aşağıdaki zorunlu ve genel gereksinimlerini gözden geçirin.

### <a name="mandatory-requirements"></a>Zorunlu gereksinimleri

* SQL Server örneğinde SQL kimlik doğrulamasını etkinleştirin. SQL kaynak sağlayıcısı sanal makine etki alanına katılmış olmadığından, yalnızca SQL kimlik doğrulaması'nı kullanarak bir barındırma sunucusuna bağlanabilirsiniz.
* Azure Stack üzerinde yüklü olduğunda genel olarak SQL örnekleri için IP adreslerini yapılandırın. Bu ağ üzerinde SQL örneğinin bağlantı gerekli olacak şekilde kaynak sağlayıcısı ve kullanıcılar, Web uygulamaları gibi kullanıcı ağ üzerinden iletişim kurar.

### <a name="general-requirements"></a>Genel gereksinimler

* SQL örneği kullanmak için kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından atayın. Diğer bir tüketici tarafından kullanılan bir SQL örneği kullanamazsınız. Bu kısıtlama, uygulama hizmetleri için de geçerlidir.
* Bir hesap (aşağıda açıklanmıştır) kaynak sağlayıcısı için uygun ayrıcalık düzeyleri ile yapılandırın.
* İşiniz SQL örnekleri ve konaklarının yönetmekten sorumludur.  Örneğin, kaynak sağlayıcısı olmayan güncelleştirmeleri uygulamak, yedeklemeler işlemek veya işlemek kimlik bilgisi döndürme.

### <a name="sql-server-virtual-machine-images"></a>SQL Server sanal makine görüntüleri

SQL Iaas sanal makine görüntüleri, Market yönetimi özelliği yoluyla kullanılabilir. Bu görüntüler Azure'da kullanıma sunulan SQL VM'ler ile aynıdır.

Her zaman en son sürümünü indirin emin olun **SQL Iaas uzantısı** bir Market öğesi kullanarak bir SQL VM dağıtmadan önce. Iaas uzantısı ve karşılık gelen portalı geliştirmeleri otomatik düzeltme eki uygulama gibi ek özellikler sağlamak ve yedekleyin. Bu uzantı hakkında daha fazla bilgi için bkz. [SQL Server Aracısı uzantısı ile Azure sanal Makineler'de yönetim görevlerini otomatikleştiren](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Şablonları dahil olmak üzere SQL Vm'leri dağıtmak için diğer seçenekler [Azure Stack hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Bir çok düğümlü Azure Stack üzerinde yüklü olan herhangi bir barındırma sunucusu, bir kullanıcı aboneliği ve olmayan varsayılan sağlayıcı aboneliği oluşturulmalıdır. Bunlar, Kullanıcı Portalı'ndan veya bir PowerShell oturumunda uygun bir oturum açma ile oluşturulmalıdır. Tüm barındırma sunucuları Faturalanabilir Vm'leri ve uygun SQL lisansına sahip olması gerekir. Hizmet Yöneticisi _olabilir_ bu aboneliğin sahibi olabilir.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar

SQL sysadmin daha düşük ayrıcalıklara sahip bir yönetici kullanıcı oluşturabilirsiniz. Kullanıcı, yalnızca şu işlemler için izinler gerekir:

* Veritabanı: Create, Alter, kapsama (için her zaman açık yalnızca), bırakın, yedekleme
* Kullanılabilirlik Grubu: Alter, birleştirme, ekleme/kaldırma veritabanı
* Oturum açma: Oluşturma, seçin, Alter, Drop, iptal etme
* Seçme işlemlerinin: \[ana\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn) (AlwaysOn) birincilindeki, sys.databases, \[ana\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[ana\].\[ sys\].\[ availability_groups\] (AlwaysOn) sys.master_files

### <a name="additional-security-information"></a>Ek güvenlik bilgileri

Aşağıdaki bilgiler, ek güvenlik rehberliği sağlar:

* Tüm Azure Stack depolama, Azure Stack üzerinde herhangi bir SQL örneğine şifrelenmiş bir blob depolama alanı kullanacaktır BitLocker'ı kullanarak şifrelenir.
* SQL kaynak sağlayıcısı, TLS 1.2 tam olarak destekler. SQL RP yönetilen herhangi bir SQL Server için TLS 1.2 yapılandırıldığından emin olun _yalnızca_ ve RP için varsayılan olur. SQL Server desteği TLS 1.2, tüm desteklenen sürümleri bkz [Microsoft SQL Server için TLS 1.2 desteği](https://support.microsoft.com/en-us/help/3135244/tls-1-2-support-for-microsoft-sql-server).
* Ayarlanacak kullanım SQL Server Configuration Manager **ForceEncryption** SQL sunucusuna tüm iletişimi sağlamak üzere seçeneği her zaman şifrelenir. Bkz: [şifrelenmiş bağlantılar zorlamak için sunucuyu yapılandırmak için](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine?view=sql-server-2017#ConfigureServerConnections).
* Tüm istemci uygulamaları da şifreli bir bağlantı iletişim kuran emin olun.
* RP, SQL Server örneği tarafından kullanılan sertifikaları güvenecek şekilde yapılandırılmıştır.

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>SQL server'ı barındıran bir tek başına bağlanarak kapasitesi sağlar.

Tek başına kullanabilirsiniz (olmayan-yüksek kullanılabilirlik) herhangi bir SQL Server 2014 veya SQL Server 2016 sürümünü kullanan SQL sunucularının. Sysadmin ayrıcalıklarına sahip bir hesabın kimlik bilgilerini sağlayın.

Önceden kurulmuş bir tek başına barındırma sunucusu eklemek için aşağıdaki adımları izleyin:

1. Azure Stack operatörü portalda Hizmet Yöneticisi olarak oturum açın.

2. Seçin **Gözat** &gt; **yönetim kaynakları** &gt; **SQL barındırma sunucuları**.

   ![SQL barındırma sunucuları](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Altında **SQL barındırma sunucuları**, SQL Server'ın kaynak sağlayıcısının arka uç olarak hizmet örneklerini SQL kaynak sağlayıcısı bağlanabilirsiniz.

   ![SQL bağdaştırıcısı Panosu](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Üzerinde **barındıran bir SQL Server ekleme**, SQL Server örneğinizi bağlantı ayrıntılarını sağlayın.

   ![SQL barındırma sunucusu Ekle](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    İsteğe bağlı olarak, bir örnek adı sağlayın ve örneği için varsayılan bağlantı noktası 1433'ın atanmamış bir bağlantı noktası numarası belirtin.

   > [!NOTE]
   > SQL örneği kullanıcı ve yönetici Azure Resource Manager tarafından erişilebilir olduğu sürece, kaynak sağlayıcısı denetiminde yerleştirilebilir. SQL örneği __gerekir__ kaynak sağlayıcıya özel olarak ayrılmış.

4. Sunucular eklediğinizde, bunları mevcut bir SKU'ya atayın veya yeni bir SKU'ya oluşturmanız gerekir. Altında **barındıran bir sunucu ekleme**seçin **SKU'ları**.

   * Mevcut bir SKU kullanmak için kullanılabilen bir SKU seçin ve ardından **Oluştur**.
   * Bir SKU oluşturmak için Seç **+ yeni SKU Oluştur**. İçinde **oluşturma SKU**gerekli bilgileri girin ve ardından **Tamam**.

     ![Bir SKU oluşturma](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>SQL Always On kullanılabilirlik grupları'nı kullanarak yüksek kullanılabilirlik sağlayın

SQL Always On örnekleri yapılandırmak ek adımlar gerektirir ve üç VM (veya fiziksel makinelere.) gerektirir Bu makalede, Always On kullanılabilirlik grupları düz bir anlayışa sahip olduğunuz varsayılır. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde ile tanışın](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Always On kullanılabilirlik grupları (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> SQL bağdaştırıcısı kaynak sağlayıcısı _yalnızca_ daha sonra Always On kullanılabilirlik grupları için örnekler veya SQL 2016 SP1 Enterprise'ı destekler. Bu bağdaştırıcı yapılandırma otomatik dengeli dağıtımı gibi yeni SQL özellikleri gerektirir.

### <a name="automatic-seeding"></a>Otomatik dengeli dağıtımı

Etkinleştirmelisiniz [otomatik dengeli dağıtımı](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) her kullanılabilirlik grubu için her SQL Server örneği üzerinde.

Tüm örneklerinde otomatik dengeli dağıtımı etkinleştirmek için düzenleyin ve ardından ikincil her örneği için birincil çoğaltmadaki aşağıdaki SQL komutunu çalıştırın:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON '<secondary_node>'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Kullanılabilirlik grubu köşeli ayraçlar içine alınmalıdır unutmayın.

İkincil düğümlerinde aşağıdaki SQL komutunu çalıştırın:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="configure-contained-database-authentication"></a>Bağımsız veritabanı kimlik doğrulamasını yapılandırma

Bir kapsanan veritabanı bir kullanılabilirlik grubuna eklemeden önce bağımsız veritabanı kimlik doğrulaması sunucu seçeneği 1 kullanılabilirlik grubu için bir kullanılabilirlik çoğaltması barındıran her sunucu örneği olarak ayarlandığından emin olun. Daha fazla bilgi için [veritabanı kimlik doğrulama sunucu yapılandırma seçeneği bulunan](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017).

Her örnek için bağımsız veritabanı kimlik doğrulama sunucusu seçeneğini ayarlamak için aşağıdaki komutları kullanın:

  ```sql
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>SQL her zaman barındırma sunucuları eklemek için

1. Bir Hizmet Yöneticisi Azure Stack yönetim portalında oturum açın

2. Seçin **Gözat** &gt; **yönetim kaynakları** &gt; **SQL barındırma sunucuları** &gt; **+Ekle**.

   Altında **SQL barındırma sunucuları**, SQL Server Kaynak sağlayıcısı, SQL Server'ın kaynak sağlayıcısının arka uç olarak hizmet gerçek örneklerini bağlanabilirsiniz.

3. SQL Server Örneğinize ilişkin bağlantı bilgilerini ile formunu doldurun. Always On dinleyicisi (ve isteğe bağlı bağlantı noktası numarası ve örnek adı) FQDN adresini kullandığınızdan emin olun. Sysadmin ayrıcalıklarına sahip yapılandırılmış hesap için bilgileri sağlayın.

4. SQL Always On kullanılabilirlik grubu örnekleri desteğini etkinleştirmek için Always On kullanılabilirlik grubu kutuyu işaretleyin.

   ![Her zaman etkinleştirin](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. SQL Always On örneği için bir SKU ekleyin.

   > [!IMPORTANT]
   > Tek başına sunucular, aynı SKU durumlarda Always On ile karıştırılamaz. İlk barındırma sunucusu sonuçları hata ekledikten sonra türlerini karıştırmak çalışılıyor.

## <a name="sku-notes"></a>SKU notları

Hizmet teklifleri ayırt etmek için SKU'ları kullanabilir. Örneğin, aşağıdaki özelliklere sahip bir SQL Enterprise örneğine sahip olabilir:
  
* yüksek kapasite
* yüksek performans
* yüksek kullanılabilirlik

SKU'ları belirli kullanıcılar veya gruplar bu sürümde atanamaz.

 SKU'ları portalda görünür olması için bir saat sürebilir. SKU tamamen oluşturulana kadar kullanıcılar bir veritabanı oluşturulamıyor.

>[!TIP]
>Yansıtılan bir SKU adı SKU, kapasite ve performans gibi sunucular yeteneklerini açıklar. Ad ve uygun SKU için kendi veritabanları dağıtma kullanıcılara yardımcı olmak için yardımcı işlevi görür.

En iyi uygulama, bir SKU tüm barındırma sunucuları aynı kaynak ve performansın özelliklere sahip olmalıdır.

## <a name="make-the-sql-databases-available-to-users"></a>SQL veritabanlarını kullanıcılar için kullanılabilir yap

Planlar ve teklifler SQL veritabanları kullanıcılar için kullanılabilir hale getirmek için oluşturun. Ekleme **Microsoft.SqlAdapter** hizmet planına ve bir yeni kota oluştur.

## <a name="next-steps"></a>Sonraki adımlar

[Veritabanlarını ekleme](azure-stack-sql-resource-provider-databases.md)
