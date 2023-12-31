## Security Group Verification ##
Verify that Security Group has been successfully created white listing provided CIDR range with SQL Server Port. This allows making connection from management PC.

![CIDR Range](pics/post-instance/1-securitygroup.png)

Also, add necessary inbound rules if you are connecting over VPN
![VPN CIDR Range](pics/post-instance/1-1-vpn-inbound-rules.png)

## Database Connection ##
Download [Azure Data Studio](https://learn.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver16&tabs=redhat-install%2Credhat-uninstall) for connecting to AWS RDS Instance

Extract Endpoint, Port and establish connection using Admin username and Password provided
![ConnectionScreenshot](pics/post-instance/2-rds-connection.png)

## Host Operating system ##
To find out host operating system, run this SQL Query:
```
SELECT host_platform
,host_distribution
,host_sku
,os_language_version
,host_release 
,host_architecture
FROM sys.dm_os_host_info;
```
![OperatingSystem](pics/post-instance/8-hostos.png)

## MS-SQL Database Engine ##
To find out  MS SQL Server installed version and edition, run this SQL Query. 

```
SELECT SERVERPROPERTY('productversion') as version
,SERVERPROPERTY ('productlevel') as level
,SERVERPROPERTY ('edition') as edition
,SERVERPROPERTY('ProductBuild') as build;
```
It would be similar to this:
![MS SQL Version and Edition](pics/post-instance/3-verify-version.png)

[Microsoft - SQL Server releases](https://learn.microsoft.com/en-us/troubleshoot/sql/releases/download-and-install-latest-updates) and [AWS RDS - SQL Server version availability](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SQLServer.html)

[All SQl Server 2019 build versions](https://learn.microsoft.com/en-us/troubleshoot/sql/releases/sqlserver-2019/build-versions)

[All SQL Server 2022 build versions](https://learn.microsoft.com/en-us/troubleshoot/sql/releases/sqlserver-2022/build-versions)

## Verify encrypted connection ##
```
select
client_net_address
,encrypt_option
,auth_scheme
,local_net_address
,local_tcp_port
,session_id
FROM SYS.DM_EXEC_CONNECTIONS where SESSION_ID = @@SPID
```
It would be similar to this:
![Encryppted connection](pics/post-instance/13-encrypt.png)

## Verify Instance level collation, timezone and language ##
```
SELECT
SERVERPROPERTY('collation')  as collation
,CURRENT_TIMEZONE() as timezone;
,@@LANGUAGE AS [Language]
,SERVERPROPERTY('SqlCharSetName') AS charset
,SERVERPROPERTY('SqlSortOrderName') AS sortorder
```
It would be similar to this:
![MS SQL Version and Edition](pics/post-instance/10-instance-collation.png)



## Verify location of Data file, log file and backup file ##
```
SELECT
  SERVERPROPERTY('MachineName') as MachineName
 ,SERVERPROPERTY('InstanceDefaultDataPath') as DatafileLoc
 ,SERVERPROPERTY('InstanceDefaultBackupPath') as BackupfileLoc
 ,SERVERPROPERTY('InstanceDefaultLogPath') as LogfileLoc;
```
It would be similar to this:
![MS SQL Version and Edition](pics/post-instance/12-filelocation.png)

## Verify Full text search is Enabled at Instance Level ##
```
select FULLTEXTSERVICEPROPERTY ( 'IsFulltextInstalled' )
```
It would be similar to this:
![FullTextsearch at instance level](pics/post-instance/4-verify-full-text-search.png)

## List Languages ##
This will be used at the time of specifying `DEFAULT_FULLTEXT_LANGUAGE` or `DEFAULT_LANGUAGE` when creating a database
```
EXEC sp_helplanguage
```
![Instance level languages](pics/post-instance/9-languages.png)

## Create Login ##
Create Server Login using this query, you can use any of the languages available in alias column
```
CREATE LOGIN maxdbowner
WITH PASSWORD = 'apjklPPmn!e23@1234567890',
DEFAULT_LANGUAGE = English;
```

It would be similar to this:
![Login](pics/post-instance/5-create-dbinstance-login.png)

## Create Database ##
Create Database using preferred settings and configuration based on business requirement.
For development, test environment setup, this query can be used

```

CREATE DATABASE [maximo86]
 CONTAINMENT = NONE
 ON  PRIMARY 
( NAME = N'maximo86', FILENAME = N'D:\rdsdbdata\DATA\maximo86.mdf' , SIZE = 1GB , FILEGROWTH = 10%)
 LOG ON 
( NAME = N'maximo86_log', FILENAME = N'D:\rdsdbdata\DATA\maximo86_log.ldf' , SIZE = 512MB , FILEGROWTH = 10%)
GO
ALTER DATABASE [maximo86] SET ANSI_NULL_DEFAULT OFF 
GO
ALTER DATABASE [maximo86] SET ANSI_NULLS OFF 
GO
ALTER DATABASE [maximo86] SET ANSI_PADDING OFF 
GO
ALTER DATABASE [maximo86] SET ANSI_WARNINGS OFF 
GO
ALTER DATABASE [maximo86] SET ARITHABORT OFF 
GO
ALTER DATABASE [maximo86] SET AUTO_CLOSE OFF 
GO
ALTER DATABASE [maximo86] SET AUTO_SHRINK OFF 
GO
ALTER DATABASE [maximo86] SET AUTO_CREATE_STATISTICS ON(INCREMENTAL = OFF)
GO
ALTER DATABASE [maximo86] SET AUTO_UPDATE_STATISTICS ON 
GO
ALTER DATABASE [maximo86] SET CURSOR_CLOSE_ON_COMMIT OFF 
GO
ALTER DATABASE [maximo86] SET CURSOR_DEFAULT  GLOBAL 
GO
ALTER DATABASE [maximo86] SET CONCAT_NULL_YIELDS_NULL OFF 
GO
ALTER DATABASE [maximo86] SET NUMERIC_ROUNDABORT OFF 
GO
ALTER DATABASE [maximo86] SET QUOTED_IDENTIFIER OFF 
GO
ALTER DATABASE [maximo86] SET RECURSIVE_TRIGGERS OFF 
GO
ALTER DATABASE [maximo86] SET  DISABLE_BROKER 
GO
ALTER DATABASE [maximo86] SET AUTO_UPDATE_STATISTICS_ASYNC OFF 
GO
ALTER DATABASE [maximo86] SET DATE_CORRELATION_OPTIMIZATION OFF 
GO
ALTER DATABASE [maximo86] SET PARAMETERIZATION SIMPLE 
GO
ALTER DATABASE [maximo86] SET READ_COMMITTED_SNAPSHOT OFF 
GO
ALTER DATABASE [maximo86] SET  READ_WRITE 
GO
ALTER DATABASE [maximo86] SET RECOVERY FULL 
GO
ALTER DATABASE [maximo86] SET  MULTI_USER 
GO
ALTER DATABASE [maximo86] SET PAGE_VERIFY CHECKSUM  
GO
ALTER DATABASE [maximo86] SET TARGET_RECOVERY_TIME = 0 SECONDS 
GO
ALTER DATABASE [maximo86] SET DELAYED_DURABILITY = DISABLED 
GO
USE [maximo86]
GO
ALTER DATABASE SCOPED CONFIGURATION SET LEGACY_CARDINALITY_ESTIMATION = Off;
GO
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = Primary;
GO
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 0;
GO
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY;
GO
ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SNIFFING = On;
GO
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = Primary;
GO
ALTER DATABASE SCOPED CONFIGURATION SET QUERY_OPTIMIZER_HOTFIXES = Off;
GO
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET QUERY_OPTIMIZER_HOTFIXES = Primary;
GO
USE [maximo86]
GO
IF NOT EXISTS (SELECT name FROM sys.filegroups WHERE is_default=1 AND name = N'PRIMARY') ALTER DATABASE [maximo86] MODIFY FILEGROUP [PRIMARY] DEFAULT
GO

```

## Verify Database scoped properties ##
```
SELECT 
sd.database_id
,sd.name
,sd.collation_name DbCollation
,sd.is_fulltext_enabled
,sd.default_fulltext_language_name
,sd.recovery_model_desc
,sd.default_language_name
,sd.compatibility_level
,sd.is_encrypted
,*
FROM sys.databases sd
WHERE name NOT IN ('master', 'model', 'tempdb', 'msdb', 'Resource','rdsadmin');
```

## Assign User to The Database ##
Use this query to assign user to the database
```
use [maximo86]
CREATE USER maxdbowner
FROM LOGIN maxdbowner
WITH DEFAULT_SCHEMA=dbo;
```

## Grant User Db_Owner rights ##
```
USE eamdb76;
ALTER ROLE [db_owner] ADD MEMBER [maxdbowner];
```

## CLI Approach ##
Download [Mature SQLCmd for Linux](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-setup-tools?view=sql-server-ver16&tabs=redhat-install) or [Cross Platform SQLCmd](https://learn.microsoft.com/en-us/sql/tools/sqlcmd/go-sqlcmd-utility?view=sql-server-ver16&tabs=mac)

### Option 1 ###
```
export SQLCMDPASSWORD=<<PASSWORD>>
export SQLCMDFORMAT=vertical
sqlcmd -S 172.174.188.115 -d maxe2e -U maxe2e
1> SELECT top 1 varname from maxvars
2> go

1> exit
```

TODO:
Refer [Script variables](https://learn.microsoft.com/en-us/sql/tools/sqlcmd/sqlcmd-use-scripting-variables?view=sql-server-ver16)

### Option 2 ###
```
sqlcmd config current-context
sqlcmd config add-user --name sqlsysadmin --username maxe2e --password-encryption none
sqlcmd config add-endpoint --name epawsrds1 --address 172.174.188.115 --port 1433
sqlcmd config add-context --name mssql1434 --user sqlsysadmin --endpoint epawsrds1

sqlcmd config current-context
sqlcmd config use-context mssql1434

sqlcmd -q "SELECT * from maxvars" -d maxe2e

sqlcmd config delete-context --name mssql1434
```