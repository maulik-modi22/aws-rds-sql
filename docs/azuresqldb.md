## Install SqlPackage on windows
```
winget install Microsoft.DotNet.Runtime.8
```
```
dotnet tool install -g microsoft.sqlpackage
```

## Backup from Azure SQL database
SqlPackage export performs best for databases under 200GB. For larger databases, you may want to optimize the operation using properties available in this article and tips in Troubleshooting with SqlPackage or alternatively achieve database portability through data in parquet files.

### If your database is using windows authentication
```
SqlPackage /Action:Export /TargetFile:"d:\maxdb76.bacpac" /SourceConnectionString:"Server=localhost\SQLEXPRESS;Database=maxdb76;Trusted_Connection=True;TrustServerCertificate=True" /p:CommandTim
eout=120 /p:TempDirectoryForTableData="d:\temp" /p:CompressionOption=Maximum /p:VerifyExtraction=True
```

