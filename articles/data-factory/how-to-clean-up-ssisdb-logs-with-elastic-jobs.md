---
title: Czyszczenie dzienników bazy danych SSISDB za pomocą zadania Elastic Database platformy Azure | Dokumentacja firmy Microsoft
description: W tym artykule opisano, jak czyszczenie dzienników bazy danych SSISDB, za pomocą zadań elastycznych baz danych platformy Azure do wyzwolenia procedury przechowywanej, która istnieje w tym celu
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/13/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 1afc40bd601c06def57ae59797d31a5edf4095bd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61345567"
---
# <a name="clean-up-ssisdb-logs-with-azure-elastic-database-jobs"></a>Czyszczenie dzienników bazy danych SSISDB za pomocą zadania Elastic Database usługi Azure

W tym artykule opisano, jak wyzwolić procedury przechowywanej, która oczyszcza dzienniki bazy danych wykazu usług SQL Server Integration Services, za pomocą zadania Elastic Database usługi Azure `SSISDB`.

Zadania elastic Database jest usługą platformy Azure, która ułatwia automatyzowaniu i uruchamianiu zadań względem bazy danych lub grupą baz danych. Można zaplanować, uruchamianie i monitorowanie tych zadań za pomocą witryny Azure portal, języka Transact-SQL, programu PowerShell lub interfejsów API REST. Zadanie elastyczne bazy danych należy używać do wyzwalania procedury składowanej Oczyszczanie dziennika jednorazowo lub zgodnie z harmonogramem. Możesz wybrać interwał harmonogramu na podstawie użycia zasobów bazy danych SSISDB, aby uniknąć obciążenia duże bazy danych.

Aby uzyskać więcej informacji, zobacz [Zarządzanie grupami bazami danych za pomocą zadania Elastic Database](../sql-database/elastic-jobs-overview.md).

W poniższych sekcjach opisano sposób włączania procedury składowanej `[internal].[cleanup_server_retention_window_exclusive]`, co spowoduje usunięcie dzienników bazy danych SSISDB, które są poza oknem przechowywania ustawionych przez administratora.

## <a name="clean-up-logs-with-power-shell"></a>Czyszczenie dzienników za pomocą powłoki Power Shell

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

Następujące przykładowe skrypty programu PowerShell utworzyć nowe zadanie elastycznej do wyzwolenia procedurę składowaną dla Oczyszczanie dziennika bazy danych SSISDB. Aby uzyskać więcej informacji, zobacz [Utwórz agenta elastycznych zadań przy użyciu programu PowerShell](../sql-database/elastic-jobs-powershell.md).

### <a name="create-parameters"></a>Utwórz parametry

``` powershell
# Parameters needed to create the Job Database
param(
$ResourceGroupName = $(Read-Host "Please enter an existing resource group name"),
$AgentServerName = $(Read-Host "Please enter the name of an existing Azure SQL server(for example, yhxserver) to hold the SSISDBLogCleanup job database"),
$SSISDBLogCleanupJobDB = $(Read-Host "Please enter a name for the Job Database to be created in the given SQL Server"),
# The Job Database should be a clean,empty,S0 or higher service tier. We set S0 as default.
$PricingTier = "S0",

# Parameters needed to create the Elastic Job agent
$SSISDBLogCleanupAgentName = $(Read-Host "Please enter a name for your new Elastic Job agent"),

# Parameters needed to create the job credential in the Job Database to connect to SSISDB
$PasswordForSSISDBCleanupUser = $(Read-Host "Please provide a new password for SSISDBLogCleanup job user to connect to SSISDB database for log cleanup"),
# Parameters needed to create a login and a user in the SSISDB of the target server
$SSISDBServerEndpoint = $(Read-Host "Please enter the name of the target Azure SQL server which contains SSISDB you need to cleanup, for example, myserver") + '.database.windows.net',
$SSISDBServerAdminUserName = $(Read-Host "Please enter the target server admin username for SQL authentication"),
$SSISDBServerAdminPassword = $(Read-Host "Please enter the target server admin password for SQL authentication"),
$SSISDBName = "SSISDB",

# Parameters needed to set job scheduling to trigger execution of cleanup stored procedure
$RunJobOrNot = $(Read-Host "Please indicate whether you want to run the job to cleanup SSISDB logs outside the log retention window immediately(Y/N). Make sure the retention window is set appropriately before running the following powershell scripts. Those removed SSISDB logs cannot be recoverd"),
$IntervalType = $(Read-Host "Please enter the interval type for the execution schedule of SSISDB log cleanup stored procedure. For the interval type, Year, Month, Day, Hour, Minute, Second can be supported."),
$IntervalCount = $(Read-Host "Please enter the detailed interval value in the given interval type for the execution schedule of SSISDB log cleanup stored procedure"),
# StartTime of the execution schedule is set as the current time as default. 
$StartTime = (Get-Date)
```

### <a name="trigger-the-cleanup-stored-procedure"></a>Procedura czyszczenia przechowywane wyzwalacza

```powershell
# Install the latest PackageManagement powershell package which PowershellGet v1.6.5 is dependent on
Find-Package PackageManagement -RequiredVersion 1.1.7.2 | Install-Package -Force
# You may need to restart the powershell session
# Install the latest PowershellGet module which adds the -AllowPrerelease flag to Install-Module
Find-Package PowerShellGet -RequiredVersion 1.6.5 | Install-Package -Force

# Place AzureRM.Sql preview cmdlets side by side with existing AzureRM.Sql version
Install-Module -Name AzureRM.Sql -AllowPrerelease -Force

# Sign in to your Azure account
Connect-AzureRmAccount

# Create a Job Database which is used for defining jobs of triggering SSISDB log cleanup stored procedure and tracking cleanup history of jobs
Write-Output "Creating a blank SQL database to be used as the SSISDBLogCleanup  Job Database ..."
$JobDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $AgentServerName -DatabaseName $SSISDBLogCleanupJobDB -RequestedServiceObjectiveName $PricingTier
$JobDatabase

# Enable the Elastic Jobs preview in your Azure subscription
Register-AzureRmProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql

# Create the Elastic Job agent
Write-Output "Creating the Elastic Job agent..."
$JobAgent = $JobDatabase | New-AzureRmSqlElasticJobAgent -Name $SSISDBLogCleanupAgentName
$JobAgent

# Create the job credential in the Job Database to connect to SSISDB database in the target server for log cleanup
Write-Output "Creating job credential to connect to SSISDB database..."
$JobCredSecure = ConvertTo-SecureString -String $PasswordForSSISDBCleanupUser -AsPlainText -Force
$JobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "SSISDBLogCleanupUser", $JobCredSecure
$JobCred = $JobAgent | New-AzureRmSqlElasticJobCredential -Name "SSISDBLogCleanupUser" -Credential $JobCred

# In the master database of the target SQL server which contains SSISDB to cleanup 
# - Create the job user login
Write-Output "Grant permissions on the master database of the target server..."
$Params = @{
  'Database' = 'master'
  'ServerInstance' = $SSISDBServerEndpoint
  'Username' = $SSISDBServerAdminUserName
  'Password' = $SSISDBServerAdminPassword
  'OutputSqlErrors' = $true
  'Query' = "CREATE LOGIN SSISDBLogCleanupUser WITH PASSWORD = '" + $PasswordForSSISDBCleanupUser + "'"
}
Invoke-SqlCmd @Params

# For SSISDB database of the target SQL server
# - Create the SSISDBLogCleanup user from the SSISDBlogCleanup user login
# - Grant permissions for the execution of SSISDB log cleanup stored procedure 
Write-Output "Grant appropriate permissions on SSISDB database..."
$TargetDatabase = $SSISDBName
$CreateJobUser = "CREATE USER SSISDBLogCleanupUser FROM LOGIN SSISDBLogCleanupUser"
$GrantStoredProcedureExecution = "GRANT EXECUTE ON internal.cleanup_server_retention_window_exclusive TO SSISDBLogCleanupUser"

$TargetDatabase | % {
  $Params.Database = $_
  $Params.Query = $CreateJobUser
  Invoke-SqlCmd @Params
  $Params.Query = $GrantStoredProcedureExecution
  Invoke-SqlCmd @Params
}

# Create a target group which includes SSISDB database needed to cleanup
Write-Output "Creating the target group including only SSISDB database needed to cleanup ..."
$SSISDBTargetGroup = $JobAgent | New-AzureRmSqlElasticJobTargetGroup -Name "SSISDBTargetGroup"
$SSISDBTargetGroup | Add-AzureRmSqlElasticJobTarget -ServerName $SSISDBServerEndpoint -Database $SSISDBName 

# Create the job to trigger execution of SSISDB log cleanup stored procedure
Write-Output "Creating a new job to trigger execution of the stored procedure for SSISDB log cleanup"
$JobName = "CleanupSSISDBLog"
$Job = $JobAgent | New-AzureRmSqlElasticJob -Name $JobName -RunOnce
$Job

# Add the job step to execute internal.cleanup_server_retention_window_exclusive
Write-Output "Adding the job step for the cleanup stored procedure execution"
$SqlText = "EXEC internal.cleanup_server_retention_window_exclusive"
$Job | Add-AzureRmSqlElasticJobStep -Name "step to execute cleanup stored procedure" -TargetGroupName $SSISDBTargetGroup.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText

# Run the job to immediately start cleanup stored procedure execution for once
IF(($RunJobOrNot = "Y") -Or ($RunJobOrNot = "y"))
{
Write-Output "Start a new execution of the stored procedure for SSISDB log cleanup immediately..."
$JobExecution = $Job | Start-AzureRmSqlElasticJob
$JobExecution
}

# Schedule the job running to trigger stored procedure execution on schedule for removing SSISDB logs outside the retention window
Write-Output "Start the execution schedule of the stored procedure for SSISDB log cleanup..."
$Job | Set-AzureRmSqlElasticJob -IntervalType $IntervalType -IntervalCount $IntervalCount -StartTime $StartTime -Enable
```

## <a name="clean-up-logs-with-transact-sql"></a>Czyszczenie dzienników za pomocą języka Transact-SQL

Następujące przykładowe skrypty języka Transact-SQL utworzyć nowe zadanie elastyczne do wyzwolenia procedurę składowaną dla Oczyszczanie dziennika bazy danych SSISDB. Aby uzyskać więcej informacji, zobacz [użycie języka Transact-SQL (T-SQL) do tworzenia i zarządzania nimi zadania Elastic Database](../sql-database/elastic-jobs-tsql.md).

1. Tworzenie lub identyfikowanie do bazy danych zadania SSISDBCleanup S0 puste lub nowszej usługi Azure SQL Database. Następnie utwórz Agent elastycznych zadań w [witryny Azure portal](https://ms.portal.azure.com/#create/Microsoft.SQLElasticJobAgent).

2. W bazie danych zadania tworzenia poświadczenia dla zadania oczyszczania dziennika bazy danych SSISDB. To poświadczenie jest używane do połączenia z bazą danych SSISDB, aby wyczyścić dzienniki.

    ```sql
    -- Connect to the job database specified when creating the job agent
    -- Create a database master key if one does not already exist, using your own password.  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD= '<EnterStrongPasswordHere>';  

    -- Create a credential for SSISDB log cleanup.  
    CREATE DATABASE SCOPED CREDENTIAL SSISDBLogCleanupCred WITH IDENTITY = 'SSISDBLogCleanupUser', SECRET = '<EnterStrongPasswordHere>'; 
    ```

3. Zdefiniuj grupę docelową, która zawiera bazę danych SSISDB, dla którego chcesz uruchomić procedurę czyszczenia przechowywane.

    ```sql
    -- Connect to the job database 
    -- Add a target group 
    EXEC jobs.sp_add_target_group 'SSISDBTargetGroup'

    -- Add SSISDB database into the target group
    EXEC jobs.sp_add_target_group_member 'SSISDBTargetGroup',
    @target_type = 'SqlDatabase',
    @server_name = '<EnterSSISDBTargetServerName>',
    @database_name = '<EnterSSISDBName>'

    --View the recently created target group and target group members
    SELECT * FROM jobs.target_groups WHERE target_group_name = 'SSISDBTargetGroup';
    SELECT * FROM jobs.target_group_members WHERE target_group_name = 'SSISDBTargetGroup';
    ```
4. Przyznaj odpowiednie uprawnienia dla bazy danych SSISDB. Katalog bazy danych SSISDB musi mieć odpowiednie uprawnienia dla procedury składowanej pomyślnie uruchomić Oczyszczanie dziennika bazy danych SSISDB. Aby uzyskać szczegółowe wskazówki, zobacz [zarządzanie danymi logowania](../sql-database/sql-database-manage-logins.md).

    ```sql
    -- Connect to the master database in the target server including SSISDB 
    CREATE LOGIN SSISDBLogCleanupUser WITH PASSWORD = '<strong_password>';

    -- Connect to SSISDB database in the target server to cleanup logs
    CREATE USER SSISDBLogCleanupUser FROM LOGIN SSISDBLogCleanupUser;
    GRANT EXECUTE ON internal.cleanup_server_retention_window_exclusive TO SSISDBLogCleanupUser
    ```
5. Tworzenie zadania i dodać krok zadania, aby wyzwolić wykonywanie procedury składowanej dla Oczyszczanie dziennika bazy danych SSISDB.

    ```sql
    --Connect to the job database 
    --Add the job for the execution of SSISDB log cleanup stored procedure.
    EXEC jobs.sp_add_job @job_name='CleanupSSISDBLog', @description='Remove SSISDB logs which are outside the retention window'

    --Add a job step to execute internal.cleanup_server_retention_window_exclusive
    EXEC jobs.sp_add_jobstep @job_name='CleanupSSISDBLog',
    @command=N'EXEC internal.cleanup_server_retention_window_exclusive',
    @credential_name='SSISDBLogCleanupCred',
    @target_group_name='SSISDBTargetGroup'
    ```
6. Przed kontynuowaniem upewnij się, że okna przechowywania został odpowiednio ustawiony. Dzienniki bazy danych SSISDB poza oknem zostaną usunięte i nie można go odzyskać.

   Następnie można uruchomić zadania od razu rozpocząć oczyszczanie dziennika bazy danych SSISDB.

    ```sql
    --Connect to the job database 
    --Run the job immediately to execute the stored procedure for SSISDB log cleanup
    declare @je uniqueidentifier
    exec jobs.sp_start_job 'CleanupSSISDBLog', @job_execution_id = @je output

    --Watch the execution results for SSISDB log cleanup 
    select @je
    select * from jobs.job_executions where job_execution_id = @je
    ```
7. Opcjonalnie można zaplanować Liczba wykonań zadań, aby usunąć dzienniki bazy danych SSISDB poza oknem przechowywania, zgodnie z harmonogramem. Aby zaktualizować parametrów zadania, należy użyć podobnych instrukcji.

    ```sql
    --Connect to the job database 
    EXEC jobs.sp_update_job
    @job_name='CleanupSSISDBLog',
    @enabled=1,
    @schedule_interval_type='<EnterIntervalType(Month,Day,Hour,Minute,Second)>',
    @schedule_interval_count='<EnterDetailedIntervalValue>',
    @schedule_start_time='<EnterProperStartTimeForSchedule>',
    @schedule_end_time='<EnterProperEndTimeForSchedule>'
    ```

## <a name="monitor-the-cleanup-job-in-the-azure-portal"></a>Monitorowanie zadania oczyszczania w witrynie Azure portal

Można monitorować wykonywania zadania oczyszczania w witrynie Azure portal. Dla każdego wykonania zostanie wyświetlony stan, czas i czas zakończenia zadania.

![Monitorowanie zadania oczyszczania w witrynie Azure portal](media/how-to-clean-up-ssisdb-logs-with-elastic-jobs/monitor-cleanup-job-portal.png)

## <a name="monitor-the-cleanup-job-with-transact-sql"></a>Monitorowanie zadania czyszczenia za pomocą instrukcji języka Transact-SQL

Aby wyświetlić historię wykonania zadania oczyszczania umożliwia także języka Transact-SQL.

```sql
--Connect to the job database 
--View all execution statuses for the job to cleanup SSISDB logs
SELECT * FROM jobs.job_executions WHERE job_name = 'CleanupSSISDBLog' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions WHERE is_active = 1
ORDER BY start_time DESC
```

## <a name="next-steps"></a>Kolejne kroki

Do zarządzania i monitorowania zadań związanych z Azure-SSIS Integration Runtime zobacz następujące artykuły. Azure-SSIS IR to aparat środowiska uruchomieniowego dla pakietów usług SSIS przechowywane w SSISDB w usłudze Azure SQL Database.

-   [Ponowne konfigurowanie środowiska Azure SSIS Integration Runtime](manage-azure-ssis-integration-runtime.md)

-   [Monitorowanie środowiska Azure-SSIS integration runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime).
