# User Initaited Backup-Restore #
As AWS RDS for SQL does not grant file system access to the user, only option we have is to use AWS S3 as the intermediate store.

## Pre-requisites ##
Steps in [Backup-Restore](iam.md) section of IAM has been executed and AWS service role has been created

## Restore database into AWS RDS ##
### Upload database backup into AWS S3 ###
There are multiple ways to get database backup into AWS S3. Appropriate option can be used depending on Size of database backup, Upload internet speed, in-transit data protection requirements.

Simplest options are to use AWS Console or AWS S3 api to upload database backup into AWS S3.

CLI way of uploading into AWS S3 is illustrated here.
![AWS CLI approach ](pics/backup-restore/1-copy-to-s3.png)

Sample code
```
aws s3 cp "maxdb76.bak" "s3://rmaulik-dsdb/manage/" --storage-class ONEZONE_IA
```

Advanced options are to use over VPN, trust establishment between AWS Accounts. Some examples are - AWS Private Link over VPN and Amazon S3 File Gateway.

### Restore to AWS RDS ###
You can run this query to restore database backup into AWS RDS
Sample Query

```
exec msdb.dbo.rds_restore_database
@restore_db_name='maxdb76',
@s3_arn_to_restore_from='arn:aws:s3:::rmaulik-dsdb/manage/maxdb76.bak';
```

Verfiy that `%complete` shows `100`, also observe value in `duration(mins)` to understand time taken to restore

```
exec msdb.dbo.rds_task_status @db_name='maxdb76'; 
```
![Restore to AWS RDS](pics/backup-restore/2-restore-to-rds.png)

Verify that database state is `ONLINE` and recovery model is `FULL`

```
SELECT 
name
,database_id
,state_desc
,recovery_model_desc
FROM sys.databases
WHERE name NOT IN
('master','msdb','tempdb','model','rdsadmin')
```

### Verification ###
Run some queries to verify database is restored correctly, querable and usable.

```

select * from maxvars where varname ='maxupg' or varname like'%db%'order by varname desc

SELECT
      QUOTENAME(SCHEMA_NAME(sOBJ.schema_id)) + '.' + QUOTENAME(sOBJ.name) AS [TableName]
      , SUM(sdmvPTNS.row_count) AS [RowCount]
FROM
      sys.objects AS sOBJ
      INNER JOIN sys.dm_db_partition_stats AS sdmvPTNS
            ON sOBJ.object_id = sdmvPTNS.object_id
WHERE 
      sOBJ.type = 'U'
      AND sOBJ.is_ms_shipped = 0x0
      AND sdmvPTNS.index_id < 2
GROUP BY
      sOBJ.schema_id
      , sOBJ.name
ORDER BY [RowCount] desc, [TableName] asc
GO
```

![Verification](pics/backup-restore/3-verify.png)

## Backup database from AWS RDS ##
In case you want to split the database backup into multiple files, you can leverage ``number_of_files`` parameter to the ``rds_backup_database`` procedure

```
exec msdb.dbo.rds_backup_database
@source_db_name='maxdb76',
@s3_arn_to_backup_to='arn:aws:s3:::rmaulik-dsdb/manage/maxdb76-dataFile*.bak',
@number_of_files=5;
```

Above execution will return `task_id` and `lifecycle` as `IN_PROGRESS` for subsequent verification of backup process completion. Please take a note of `task_id` that will be used in next query

To ensure your backup is fully complete, run query substituting `task_id` returned from previous query and verify that `lifecycle` shows `SUCCESS` and `%complete` shows `100`
```
exec msdb.dbo.rds_task_status @task_id=<<task_id>>; 
```