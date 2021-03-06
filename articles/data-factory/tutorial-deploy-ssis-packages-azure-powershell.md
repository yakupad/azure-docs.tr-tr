---
title: PowerShell ile Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama | Microsoft Docs
description: Azure'da SSIS paketleri dağıtıp çalıştırabilmek için PowerShell ile Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı sağlamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: hero-article
ms.date: 06/27/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 8dc41eb3507368bb810c30a7680b73d0d1f26408
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085429"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>Azure Data Factory'de PowerShell ile Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama
Bu öğretici, Azure Data Factory’de bir Azure-SSIS tümleştirme çalışma zamanı (IR) sağlama adımlarını sunar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio'yu (SSMS) kullanarak Azure'da bu çalışma zamanında SQL Server Integration Services (SSIS) paketleri dağıtabilir ve çalıştırabilirsiniz. Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!NOTE]
> Bu makale, Azure SSIS IR sağlamak için Azure PowerShell’i kullanır. Azure SSIS IR sağlamak amacıyla Data Factory Kullanıcı Arabirimi (UI) kullanmak için bkz. [Öğretici: Azure SSIS tümleştirme çalışma zamanı oluşturma](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SSIS tümleştirme çalışma zamanı oluşturma
> * Azure SSIS tümleştirme çalışma zamanını başlatma
> * SSIS paketlerini dağıtma
> * Tam betiği gözden geçirme

## <a name="prerequisites"></a>Ön koşullar
- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure-SSIS IR hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime). 
- **Azure SQL Veritabanı sunucusu**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Bu sunucu, SSIS Katalog veritabanını (SSISDB) barındırır. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB’ye yürütme günlüklerini yazmasına olanak tanır. 
    - Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, Elastik Havuzun bir parçası veya Yönetilen Örnek (Önizleme) biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB hizmetini barındıracak veritabanı sunucusu türünü seçme konusunda yardım almak için bkz. [SQL Veritabanı ile Yönetilen Örneği (Önizleme) Karşılaştırma](create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview). SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek (Önizleme) ile kullanırsanız veya şirket içi verilere erişmeniz gerekiyorsa Azure-SSIS IR örneğinizi bir sanal ağa eklemeniz gerekir, bkz. [Sanal ağda Azure-SSIS IR oluşturma](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Veritabanı sunucusunda "**Azure hizmetlerine erişime izin ver**" ayarının **AÇIK** olduğundan emin olun. Bu ayar SSISDB barındırmak için Azure SQL Veritabanı'nı sanal ağ hizmet uç noktaları/Yönetilen Örnek (Önizleme) ile birlikte kullandığınızda geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Bu ayarı PowerShell kullanarak etkinleştirmek için bkz. [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1). 
    - Veritabanı sunucusunun güvenlik duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini veya istemci makinenin IP adresini içeren IP adresi aralığını ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md). 
    - Veritabanı sunucusuna bağlanmak için sunucu yöneticisi kimlik bilgilerinizle SQL kimlik doğrulamasını veya Azure Data Factory Yönetilen Hizmet Kimliği (MSI) bilgilerinizle Azure Active Directory (AAD) kimlik doğrulamasını kullanabilirsiniz.  İkinci seçenek için Data Factory MSI bilgilerinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). 
    - Azure SQL Veritabanı sunucunuzun SSIS Kataloğuna (SSISDB veritabanı) sahip olmadığını doğrulayın. Azure-SSIS IR’nin sağlanması, mevcut bir SSIS Kataloğunun kullanılmasını desteklemez. 
- **Azure PowerShell**. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) bölümündeki yönergeleri izleyin. Bulutta SSIS paketleri çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı sağlamak üzere betik çalıştırmak için PowerShell kullanıyorsanız. 

> [!NOTE]
> - Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. 
> - Azure SSIS Tümleştirme Çalışma Zamanı'nın kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **SSIS Tümleştirme Çalışma Zamanı**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun.

## <a name="launch-windows-powershell-ise"></a>Windows PowerShell ISE’yi başlatma
**Windows PowerShell ISE**’yi yönetici ayrıcalıklarıyla başlatın. 

## <a name="create-variables"></a>Değişken oluşturma
Şu betiği kopyalayıp yapıştırın: Değişkenlerin değerlerini belirtin. Azure SQL Veritabanında desteklenen **fiyatlandırma katmanı** listesi için bkz. [SQL Veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md).

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B". For standard/premium/Elastic Pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"
```

## <a name="validate-the-connection-to-database"></a>Veritabanı bağlantısını doğrulama
Azure SQL Veritabanı sunucunuz `<servername>.database.windows.net`’i doğrulamak için aşağıdaki betiği ekleyin. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

Betiğin bir parçası olarak bir Azure SQL veritabanı oluşturmak için aşağıdaki örneğe bakın: 

Önceden tanımlanmamış değişkenlerin değerlerini belirleyin. Örneğin: SSISDBServerName, FirewallIPAddress. 

```powershell
New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="log-in-and-select-subscription"></a>Oturum açma ve abonelik seçme
Oturum açıp Azure aboneliğinizi seçmek için betiğe aşağıdaki kodu ekleyin: 

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

Kaynak grubunuz zaten varsa, bu kodu betiğinize kopyalamayın. 

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma
Azure’da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın: 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
  
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier
```

## <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma
Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Bu komutun tamamlanması **20-30 dakika** sürer. 

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS kataloğunu (SSISDB) barındıran Azure SQL sunucunuza bağlanın. Azure SQL Veritabanı sunucusunun adı şu biçimdedir: `<servername>.database.windows.net`. 

SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure’da SSIS kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="full-script"></a>Tam betik
Bu bölümdeki PowerShell betiği, bulutta SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı örneğini yapılandırır. Bu betiği başarıyla çalıştırdıktan sonra, Microsoft Azure bulutundaki SSIS paketlerini Azure SQL Veritabanında barındırılan SSISDB ile dağıtıp çalıştırabilirsiniz.

1. Windows PowerShell Tümleşik Komut Dosyası Ortamı’nı (ISE) başlatın.
2. ISE'de komut isteminden aşağıdaki komutu çalıştırın.    
    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```
3. Bu bölümde PowerShell betiğini kopyalayıp ISE’ye yapıştırın.
4. Betiğin başında tüm parametreler için uygun değerleri sağlayın.
5. Betiği çalıştırın. Betiğin sonundaki `Start-AzureRmDataFactoryV2IntegrationRuntime` komutu **20-30 dakika** boyunca çalışır.

> [!NOTE]
> - Betik, SSIS Kataloğu veritabanını (SSISDB) hazırlamak için Azure SQL Veritabanı sunucusuna bağlanır.

> - Bir Azure-SSIS IR örneğini sağladığınızda, SSIS için Azure Feature Pack ve Access Redistributable da yüklenir. Bu bileşenler, Excel ile Access dosyalarıyla ve yerleşik bileşenler tarafından desteklenen veri kaynaklarına ek olarak çeşitli Azure veri kaynaklarıyla bağlantı kurma olanağı sunar. Ek bileşenleri de yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

Azure SQL Veritabanında desteklenen **fiyatlandırma katmanı** listesi için bkz. [SQL Veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md). 

Azure Data Factory V2 ve Azure-SSIS Integration Runtime tarafından desteklenen tüm bölgelerin listesi için bkz. [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/). **Veri ve Analiz**’i genişleterek **Data Factory V2** ve **SSIS Integration Runtime** seçeneklerini görün.

```powershell
# Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
$SubscriptionName = "[Azure subscription name]"
$ResourceGroupName = "[Azure resource group name]"
# Data factory name. Must be globally unique
$DataFactoryName = "[Data factory name]"
$DataFactoryLocation = "EastUS"

# Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[Specify a name for your Azure-SSIS IR]"
$AzureSSISDescription = "[Specify a description for your Azure-SSIS IR]"
$AzureSSISLocation = "EastUS"
# In public preview, only Standard_A4_v2, Standard_A8_v2, Standard_D1_v2, Standard_D2_v2, Standard_D3_v2, Standard_D4_v2 are supported
$AzureSSISNodeSize = "Standard_D4_v2"
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, 1-4 parallel executions per node are supported. For other nodes, it's 1-8.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name].database.windows.net" # WARNING: Please ensure that there is no existing SSISDB, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B". For standard/premium/Elastic Pool tiers, specify "S0", "S1", "S2", "S3", etc.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>)]"

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
    
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
    
Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -SetupScriptContainerSasUri $SetupScriptContainerSasUri `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogAdminCredential $serverCreds `
                                           -CatalogPricingTier $SSISDBPricingTier

write-host("##### Starting your Azure-SSIS integration runtime. This command takes 20 to 30 minutes to complete. #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="join-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR’yi bir sanal ağa ekleme
SSISDB'yi barındırmak için bir sanal ağa katılan sanal ağ hizmeti uç noktaları/Yönetilen Örnek (Önizleme) ile Azure SQL Veritabanı kullanıyorsanız, Azure-SSIS tümleştirme çalışma zamanınızı da aynı sanal ağa eklemeniz gerekir. Azure Data Factory, Azure SSIS tümleştirme çalışma zamanını bir sanal ağa eklemenize olanak tanır. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa ekleme](join-azure-ssis-integration-runtime-virtual-network.md).

Bir sanal ağa katılan Azure-SSIS tümleştirme çalışma zamanı oluşturmaya yönelik tam betik için bkz. [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md).

## <a name="monitor-and-manage-azure-ssis-ir"></a>Azure-SSIS IR’yi izleme ve yönetme
Bir Azure-SSIS IR’yi izleme ve yönetme hakkında ayrıntılar için aşağıdaki makalelere bakın. 

- [Azure-SSIS tümleştirme çalışma zamanını izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Azure-SSIS tümleştirme çalışma zamanını yönetme](manage-azure-ssis-integration-runtime.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SSIS tümleştirme çalışma zamanı oluşturma
> * Azure SSIS tümleştirme çalışma zamanını başlatma
> * SSIS paketlerini dağıtma
> * Tam betiği gözden geçirme

Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Buluttaki verileri kopyalama](tutorial-copy-data-dot-net.md)
