---
title: Uzun vadeli yedekleme bekletme - Azure SQL veritabanı yapılandırma | Microsoft Docs
description: Azure kurtarma Hizmetleri kasasına otomatik yedeklemelerini depolamak ve Azure kurtarma Hizmetleri Kasası'nı geri yükleme hakkında bilgi edinin
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 05/08/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: feefe68fbe6681ee4b450503606ac8c4f25d5a39
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130269"
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention-using-azure-recovery-services-vault"></a>Yapılandırma ve Azure kurtarma Hizmetleri kasası kullanarak Azure SQL veritabanı uzun vadeli yedekleme bekletme geri yükleme

Azure SQL veritabanı yedeklemelerini depolamak ve Azure portal veya PowerShell kullanarak kasaya korunur yedekleri kullanarak bir veritabanını kurtarmak için Azure kurtarma Hizmetleri kasası yapılandırabilirsiniz.

> [!NOTE]
> Ekim 2016 uzun vadeli yedekleme bekletme önizlemesini ilk sürümünü bir parçası olarak, Azure kurtarma Hizmetleri kasasına yedekleme depolandı. Bu güncelleştirme bu bağımlılığı kaldırır, ancak geriye dönük uyumluluk için özgün API'si 31 May 2018 kadar desteklenir. Azure Hizmetleri kurtarma kasasında yedeklemeleri etkileşimde gerekiyorsa, bkz: [Azure kurtarma Hizmetleri kasası kullanılarak uzun vadeli yedekleme bekletme](sql-database-long-term-backup-retention-configure-vault.md). 


## <a name="azure-portal"></a>Azure portalına

Aşağıdaki bölümlerde, Azure portalında Azure kurtarma Hizmetleri kasası yapılandırmak, yedekleme kasası ve geri yükleme Kasası'ndan görüntülemek için nasıl kullanıldığını gösterir.

### <a name="configure-the-vault-register-the-server-and-select-databases"></a>Kasa yapılandırmak, sunucuyu kaydetmek ve veritabanlarını seçin

Bir Azure kurtarma Hizmetleri kasası yapılandırma [otomatik yedeklemelerini Beklet](sql-database-long-term-retention.md) hizmet katmanı için saklama süresinden daha uzun bir süre. 

1. Açık **SQL Server** sunucunuz için sayfa.

   ![SQL server sayfası](./media/sql-database-get-started-portal/sql-server-blade.png)

2. **Uzun süreli yedek saklama**'ya tıklayın.

   ![uzun süreli yedek saklama bağlantısı](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Üzerinde **uzun vadeli yedekleme bekletme** sayfasında sunucunuz için gözden geçirin ve önizleme koşullarına (bunu - yapmış olduğunuz veya bu özellik artık Önizleme sürümünde olduğu sürece) kabul edin.

   ![önizleme koşullarını kabul edin](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Uzun vadeli yedekleme bekletme yapılandırmak için kılavuzdaki bu veritabanını seçin ve ardından **yapılandırma** araç çubuğunda.

   ![uzun süreli yedek saklama için veritabanı seçme](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Üzerinde **yapılandırma** sayfasında, **gerekli ayarları Yapılandır** altında **kurtarma hizmeti kasasının**.

   ![kasayı yapılandırma bağlantısı](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Üzerinde **kurtarma Hizmetleri kasası** sayfasında, varsa mevcut bir kasayı seçin. Aboneliğinize ait kurtarma hizmetleri kasası yoksa akıştan çıkın ve kurtarma hizmetleri kasası oluşturun.

   ![Kasa bağlantı oluşturma](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Üzerinde **kurtarma Hizmetleri kasaları** sayfasında, **Ekle**.

   ![Kasa bağlantısı ekleme](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Üzerinde **kurtarma Hizmetleri kasası** sayfasında, Kurtarma Hizmetleri kasası için geçerli bir ad sağlayın.

   ![yeni kasa adı](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Aboneliğinizi ve kaynak grubunu seçtikten sonra kasa için bir konum belirleyin. İşiniz bittiğinde **Oluştur**'a tıklayın.

   ![Kasa oluşturma](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Kasanın Azure SQL mantıksal sunucusuyla aynı bölgede olması ve mantıksal sunucuyla aynı kaynak grubunu kullanması gerekir.
   >

10. Yeni kasa oluşturulduktan sonra geri dönmek için gerekli adımları yürütün **kurtarma Hizmetleri kasası** sayfası.

11. Üzerinde **kurtarma Hizmetleri kasası** sayfasında, kasaya tıklayın ve ardından **seçin**.

   ![var olan kasayı seçme](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Üzerinde **yapılandırma** sayfasında, yeni bir bekletme ilkesi için geçerli bir ad, varsayılan saklama ilkeyi uygun şekilde değiştirin ve ardından **Tamam**.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)
   
   >[!NOTE]
   >Bekletme İlkesi adları, boşluk dahil olmak üzere bazı karakterler izin vermez.

13. Üzerinde **uzun vadeli yedekleme bekletme** sayfasında veritabanınız için **kaydetmek** ve ardından **Tamam** uzun vadeli yedekleme bekletme ilkesi seçilen tüm veritabanlarına uygulanacak.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Yapılandırdığınız Azure Kurtarma Hizmetleri kasasında bu yeni ilkeyi kullanan uzun süreli yedek saklamayı etkinleştirmek için **Kaydet**'e tıklayın.

   ![saklama ilkesi tanımlama](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Yapılandırma yapıldıktan sonra yedekler sonraki yedi gün içinde kasada görünür. Bu öğreticiye devam etmek için yedeklerin kasada görünmesini bekleyin.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Azure portalını kullanarak uzun vadeli bekletme içinde yedekleri görüntüle

Veritabanı yedeklerinizi hakkındaki bilgileri görüntüleyin [uzun vadeli yedekleme bekletme](sql-database-long-term-retention.md). 

1. Azure portalında Azure kurtarma Hizmetleri kasanız için veritabanı yedeklerinizi açın (Git **tüm kaynakları** ve aboneliğinize ilişkin kaynaklar listesinden seçin) veritabanı yedeklerinizi tarafından kullanılan depolama alanı miktarı görüntülemek için Kasa.

   ![kurtarma hizmetleri kasasını ve yedekleri görüntüleme](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Açık **SQL veritabanı** veritabanınız için sayfa.

   ![Yeni örnek db sayfası](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Araç çubuğunda **Geri yükle**'ye tıklayın.

   ![geri yükleme araç çubuğu](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Geri yükleme sayfasında, tıklatın **uzun vadeli**.

5. Azure kasası yedeklemelerinin altında **Bir yedekleme seçin**'e tıklayarak uzun süreli saklama kapsamındaki kullanılabilir veritabanı yedeklerini görüntüleyebilirsiniz.

   ![kasadaki yedekler](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-the-azure-portal"></a>Azure portalını kullanarak uzun vadeli yedekleme bekletme içinde bir yedekten bir veritabanını geri yükleyin

Azure kurtarma Hizmetleri kasasında bir yedekten yeni bir veritabanı için veritabanı geri.

1. Üzerinde **Azure kasası yedeklemeleri** sayfasında, yedekleme geri yükleyin ve ardından **seçin**.

   ![kasadaki bir yedeği seçme](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. **Veritabanı adı** metin kutusunda geri yüklenen veritabanı için bir ad girin.

   ![yeni veritabanı adı](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Veritabanınızı kasadaki yedekten yeni veritabanına geri yüklemek için **Tamam**'a tıklayın.

4. Geri yükleme işinin durumunu görüntülemek için araç çubuğundaki bildirim simgesine tıklayın.

   ![kasadan geri yükleme işi ilerleme durumu](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Geri yükleme işi tamamlandığında açmak **SQL veritabanları** yeni geri yüklenen veritabanı görüntülemek için sayfa.

   ![kasadan geri yüklenen veritabanı](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Buradan [var olan veritabanına kopyalamak için geri yüklenen veritabanından veri ayıklama veya var olan veritabanını silerek geri yüklenen veritabanının adını var olan veritabanının adıyla değiştirme](sql-database-recovery-using-backups.md#point-in-time-restore) gibi görevleri gerçekleştirmek için SQL Server Management Studio kullanarak geri yüklenen veritabanına bağlanabilirsiniz.
>

## <a name="powershell"></a>PowerShell

Aşağıdaki bölümlerde, PowerShell Azure kurtarma Hizmetleri kasası yapılandırmak, yedekleme kasası ve geri yükleme Kasası'ndan görüntülemek için nasıl kullanıldığını gösterir.

### <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Kullanım [yeni AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) bir kurtarma Hizmetleri kasası oluşturmak için.

> [!IMPORTANT]
> Kasanın Azure SQL mantıksal sunucusuyla aynı bölgede olması ve mantıksal sunucuyla aynı kaynak grubunu kullanması gerekir.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-for-its-long-term-retention-backups"></a>Kurtarma kasası, uzun vadeli bekletme yedeklemeleri için kullanmak için sunucunuzu ayarlayın

Kullanım [kümesi AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet'ini daha önce oluşturulan kurtarma Hizmetleri kasası belirli bir Azure SQL sunucusu ile ilişkilendirin.

```PowerShell
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Saklama ilkesi tanımlama

Saklama ilkesi, bir veritabanı yedeğinin saklanacağı süreyi belirlediğiniz yerdir. Kullanmak [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet'ini ilkeleri oluşturmak için şablon olarak kullanmak için varsayılan saklama ilkesini alır. Bu şablonda saklama süresi için 2 yıl ayarlanır. Ardından, çalıştırın [yeni AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) son olarak bir ilke oluşturmak için. 

> [!NOTE]
> Bazı cmdlet'leri çalıştırmadan önce kasası bağlamını ayarlayın gerektirir ([kümesi AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) birkaç ilgili parçacıkları Bu cmdlet görürsünüz. İlke kasaya parçası olduğundan bağlamını ayarlayın. Her kasa için birden fazla saklama ilkesi oluşturabilir ve ardından belirli veritabanlarına istediğiniz ilkeyi uygulayabilirsiniz. 


```PowerShell
# Retrieve the default retention policy for the AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention value to two years (you can set to any time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-to-use-the-previously-defined-retention-policy"></a>Bir veritabanını önceden tanımlanan saklama ilkesini kullanacak şekilde yapılandırma

Kullanım [kümesi AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) belirli bir veritabanına yeni ilkeyi uygulamak için cmdlet.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Uzun süreli saklama kapsamındaki yedekleme bilgilerini ve yedekleri görüntüleme

Veritabanı yedeklerinizi hakkındaki bilgileri görüntüleyin [uzun vadeli yedekleme bekletme](sql-database-long-term-retention.md). 

Yedekleme bilgilerini görüntülemek için aşağıdaki cmdlet'leri kullanın:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set the vault context to the vault we want to restore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Bir veritabanını uzun süreli yedek saklama kapsamındaki bir yedekten geri yükleme

Uzun vadeli yedekleme bekletme geri kullanan [geri yükleme-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet'i.

```PowerShell
# Restore the most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> Buradan, gerekli görevleri gerçekleştirmek için SQL Server Management Studio'yu kullanarak geri yüklenen veritabanına bağlanmak, geri yüklenen veritabanından var olan veritabanına kopyalamak için veya varolan silmek için bir bit veri ayıklamak gibi veritabanı ve geri yüklenen yeniden adlandırın Varolan bir veritabanı adı veritabanı. Bkz: [geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="how-to-cleanup-backups-in-recovery-services-vault"></a>Kurtarma Hizmetleri kasası temizleme yedeklemelerin nasıl

1 Temmuz 2018 itibariyle LTR V1 API kullanım dışıdır ve tüm mevcut Yedeklemelerinizin kurtarma hizmeti kasa içinde SQL veritabanı tarafından yönetilen LTR depolama kapsayıcıları geçirdiğiniz. Özgün yedeklemeler için artık ücretlendirilen emin olmak için kasalarını geçişten sonra kaldırılmış. Ancak, kasanız üzerinde bir kilit yerleştirdiyseniz yedeklemeleri var. devam edecektir. Gereksiz maliyetleri önlemek için eski yedeklemeleri aşağıdaki betiği kullanarak kurtarma hizmeti kasadan el ile kaldırabilirsiniz. 

```PowerShell
<#
.EXAMPLE
    .\Drop-LtrV1Backup.ps1 -SubscriptionId “{vault_sub_id}” -ResourceGroup “{vault_resource_group}” -VaultName “{vault_name}” 
#>
[CmdletBinding()]
Param (
    [Parameter(Mandatory = $true, HelpMessage="The vault subscription ID")]
    $SubscriptionId,

    [Parameter(Mandatory = $true, HelpMessage="The vault resource group name")]
    $ResourceGroup,

    [Parameter(Mandatory = $true, HelpMessage="The vault name")]
    $VaultName
)

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $SubscriptionId

$vaults = Get-AzureRmRecoveryServicesVault
$vault = $vaults | where { $_.Name -eq $VaultName }

Set-AzureRmRecoveryServicesVaultContext -Vault $vault

$containers = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL

ForEach ($container in $containers)
{
   $canDeleteContainer = $true  
   $ItemCount = 0
   Write-Host "Working on container" $container.Name
   $items = Get-AzureRmRecoveryServicesBackupItem -container $container -WorkloadType AzureSQLDatabase
   ForEach ($item in $items)
   {
          write-host "Deleting item" $item.name
          Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints -item $item -Force
   }

   Write-Host "Deleting container" $container.Name
   Unregister-AzureRmRecoveryServicesBackupContainer -Container $container
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](sql-database-automated-backups.md)
- Uzun süreli yedek saklama hakkında bilgi edinmek için bkz. [uzun süreli yedek saklama](sql-database-long-term-retention.md)
- Yedekleri geri yükleme hakkında bilgi edinmek için bkz. [yedekten geri yükleme](sql-database-recovery-using-backups.md)
