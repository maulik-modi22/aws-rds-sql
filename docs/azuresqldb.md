## Backup from Azure SQL database
SqlPackage export performs best for databases under 200GB. For larger databases, you may want to optimize the operation using properties available in this article and tips in Troubleshooting with SqlPackage or alternatively achieve database portability through data in parquet files.


SqlPackage /Action:Export /TargetFile:"C:\AdventureWorksLT.bacpac" \
    /SourceConnectionString:"" \
    /p:Storage=File \
    /p:CommandTimeout=120 \
    /p:TempDirectoryForTableData="d:\temp" \
    /p:CompressionOption=Normal \
    /p:VerifyExtraction=True
