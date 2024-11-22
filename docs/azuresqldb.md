## Install SqlPackage on windows
```
winget install Microsoft.DotNet.SDK.8
```
```
dotnet tool install -g microsoft.sqlpackage
```

## Export a BACPAC file - Azure SQL Database
https://learn.microsoft.com/en-us/azure/azure-sql/database/database-export?view=azuresql

## Install SqlPackage on Mac
Refer steps here - https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage-download?view=sql-server-ver16#macos

## Backup from Azure SQL database
SqlPackage export performs best for databases under 200GB. For larger databases, you may want to optimize the operation using properties available in this article and tips in Troubleshooting with SqlPackage or alternatively achieve database portability through data in parquet files.

### If your database is using windows authentication
```
SqlPackage /Action:Export /TargetFile:"d:\maxdb76.bacpac" /SourceConnectionString:"Server=localhost\SQLEXPRESS;Database=maxdb76;Trusted_Connection=True;TrustServerCertificate=True" /p:CommandTim
eout=120 /p:TempDirectoryForTableData="d:\temp" /p:CompressionOption=Maximum /p:VerifyExtraction=True
```

## Restore BACPAC in AWS RDS
https://learn.microsoft.com/en-us/sql/tools/sqlpackage/sqlpackage?view=sql-server-ver16

```
export SOURCE_BACPAC_FILE=
export TARGET_RDS_ENDPOINT=
export TARGET_RDS_DATABASE=
export TARGET_DB_USER=
export TARGET_DB_PASSWORD=
```
```
sqlpackage.exe /Action:Import /sf:"${SOURCE_BACPAC_FILE}" /tsn:"${TARGET_RDS_ENDPOINT}" /tdn:"${TARGET_RDS_DATABASE}" /tu:"${TARGET_DB_USER}" /tp:"${TARGET_DB_PASSWORD}" /TargetTrustServerCertificate:true 
```

## Restore BACPAC in powershell
```
$SOURCE_BACPAC_FILE= "C:\Users\Administrator\Downloads\maxdb76.bacpac"
$TARGET_RDS_ENDPOINT= "rdsmssql01.czo3pe0z5oxn.us-east-1.rds.amazonaws.com"
$TARGET_RDS_DATABASE= "maximo86"
$TARGET_DB_USER= ""
$TARGET_DB_PASSWORD= ""

sqlpackage /Action:Import /sf:$SOURCE_BACPAC_FILE /tsn:$TARGET_RDS_ENDPOINT /tdn:$TARGET_RDS_DATABASE /tu:$TARGET_DB_USER /tp:$TARGET_DB_PASSWORD /TargetTrustServerCertificate:true 

```


