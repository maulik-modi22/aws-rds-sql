# SQL Server to Postgres Migration #
## Find supported database engines ##
More info is https://docs.aws.amazon.com/cli/latest/reference/dms/describe-endpoint-types.html
```
aws dms describe-endpoint-types --filters "Name=engine-name,Values=db2"
```
```
{
    "SupportedEndpointTypes": [
        {
            "EngineName": "db2",
            "SupportsCDC": true,
            "EndpointType": "source",
            "EngineDisplayName": "IBM Db2 LUW"
        }
    ]
}
```

```
aws dms describe-endpoint-types --filters "Name=engine-name,Values=sqlserver"
```
```
{
    "SupportedEndpointTypes": [
        {
            "EngineName": "sqlserver",
            "SupportsCDC": true,
            "EndpointType": "source",
            "EngineDisplayName": "Microsoft SQL Server"
        },
        {
            "EngineName": "sqlserver",
            "SupportsCDC": true,
            "EndpointType": "target",
            "EngineDisplayName": "Microsoft SQL Server"
        }
    ]
}
```

## Infrastructure layout ##
![Infra layout](pics/migrate/infra.png)

## Describe endpoints ##
![Source Endpoint configuration](pics/migrate/endpoint/source-ms-sql.png)
Observe source engine and server name

![Destination Endpoint configuration](pics/migrate/endpoint/destination-postgres.png)
Observe target engine, server name and schema

```
aws dms describe-endpoints
```

```

    "Endpoints": [
        {
            "EndpointIdentifier": "source-mssql-vm",
            "EndpointType": "SOURCE",
            "EngineName": "sqlserver",
            "EngineDisplayName": "Microsoft SQL Server",
            "Username": "maulik",
            "ServerName": "10.0.1.175\\sqlexpress",
            "Port": 1433,
            "DatabaseName": "maxdb76",
            "Status": "active",
            "KmsKeyId": "arn:aws:kms:us-east-2:698108604540:key/da1d66a1-0237-4d69-a4a2-979e7059406d",
            "EndpointArn": "arn:aws:dms:us-east-2:698108604540:endpoint:YIMVQSTQRHDU3C2F74TPSSL5GXOG6ILGLHPOLYI",
            "SslMode": "none",
            "MicrosoftSQLServerSettings": {
                "Port": 1433,
                "DatabaseName": "maxdb76",
                "ServerName": "10.0.1.175\\sqlexpress",
                "Username": "maulik"
            }
        },
        {
            "EndpointIdentifier": "target-postgres",
            "EndpointType": "TARGET",
            "EngineName": "postgres",
            "EngineDisplayName": "PostgreSQL",
            "Username": "yemerawatsonx",
            "ServerName": "maximosaastier.ch5aesgs69g5.us-east-2.rds.amazonaws.com",
            "Port": 5678,
            "DatabaseName": "eam76",
            "Status": "active",
            "KmsKeyId": "arn:aws:kms:us-east-2:698108604540:key/da1d66a1-0237-4d69-a4a2-979e7059406d",
            "EndpointArn": "arn:aws:dms:us-east-2:698108604540:endpoint:5KOC27HAARVBY7W3CXAQXRHFOT5KZPLQ6RH5ENY",
            "SslMode": "require",
            "PostgreSQLSettings": {
                "DatabaseName": "eam76",
                "DdlArtifactsSchema": "dbo",
                "Port": 5678,
                "ServerName": "maximosaastier.ch5aesgs69g5.us-east-2.rds.amazonaws.com",
                "Username": "yemerawatsonx"
            }
        }
    ]
```

## Describe Subnet group ##
![subnet group](pics/migrate/subnet-group/subnet-group.png)

Refer https://awscli.amazonaws.com/v2/documentation/api/latest/reference/dms/describe-replication-subnet-groups.html
```
aws dms describe-replication-subnet-groups
```

```
{
    "ReplicationSubnetGroups": [
        {
            "ReplicationSubnetGroupIdentifier": "devsubnetgroup",
            "ReplicationSubnetGroupDescription": "devsubnetgroup",
            "VpcId": "vpc-0c4f0baaa0402b2fa",
            "SubnetGroupStatus": "Complete",
            "Subnets": [
                {
                    "SubnetIdentifier": "subnet-0fd8464a91ea0d6b9",
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-2a"
                    },
                    "SubnetStatus": "Active"
                },
                {
                    "SubnetIdentifier": "subnet-08df376d117fa5cb5",
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-2b"
                    },
                    "SubnetStatus": "Active"
                },
                {
                    "SubnetIdentifier": "subnet-073ba5de0ce0b230d",
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-2b"
                    },
                    "SubnetStatus": "Active"
                },
                {
                    "SubnetIdentifier": "subnet-0ccc2bf815714de09",
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-2a"
                    },
                    "SubnetStatus": "Active"
                }
            ],
            "SupportedNetworkTypes": [
                "IPV4"
            ]
        }
    ]
}
```

## Replication Instance Security Group ##
Inbound rule allows VPC CIDR
![Security group](pics/migrate/security/replication-instance.png)


## Describe replication instances ##
Observer instance class, engine version and security group
![Replication Instance](pics/migrate/replication-instance/instance-settings.png)
https://docs.aws.amazon.com/cli/latest/reference/dms/describe-replication-instances.html

```
 aws dms describe-replication-instances
```

```
{
    "ReplicationInstances": [
        {
            "ReplicationInstanceIdentifier": "mssqltopostgres",
            "ReplicationInstanceClass": "dms.t3.medium",
            "ReplicationInstanceStatus": "available",
            "AllocatedStorage": 50,
            "InstanceCreateTime": "2023-08-31T12:15:35.090000+00:00",
            "VpcSecurityGroups": [
                {
                    "VpcSecurityGroupId": "sg-03c78e375bd146688",
                    "Status": "active"
                }
            ],
            "AvailabilityZone": "us-east-2a",
            "ReplicationSubnetGroup": {
                "ReplicationSubnetGroupIdentifier": "devsubnetgroup",
                "ReplicationSubnetGroupDescription": "devsubnetgroup",
                "VpcId": "vpc-0c4f0baaa0402b2fa",
                "SubnetGroupStatus": "Complete",
                "Subnets": [
                    {
                        "SubnetIdentifier": "subnet-0fd8464a91ea0d6b9",
                        "SubnetAvailabilityZone": {
                            "Name": "us-east-2a"
                        },
                        "SubnetStatus": "Active"
                    },
                    {
                        "SubnetIdentifier": "subnet-08df376d117fa5cb5",
                        "SubnetAvailabilityZone": {
                            "Name": "us-east-2b"
                        },
                        "SubnetStatus": "Active"
                    },
                    {
                        "SubnetIdentifier": "subnet-073ba5de0ce0b230d",
                        "SubnetAvailabilityZone": {
                            "Name": "us-east-2b"
                        },
                        "SubnetStatus": "Active"
                    },
                    {
                        "SubnetIdentifier": "subnet-0ccc2bf815714de09",
                        "SubnetAvailabilityZone": {
                            "Name": "us-east-2a"
                        },
                        "SubnetStatus": "Active"
                    }
                ]
            },
            "PreferredMaintenanceWindow": "sat:03:53-sat:04:23",
            "PendingModifiedValues": {},
            "MultiAZ": false,
            "EngineVersion": "3.4.7",
            "AutoMinorVersionUpgrade": true,
            "KmsKeyId": "arn:aws:kms:us-east-2:698108604540:key/da1d66a1-0237-4d69-a4a2-979e7059406d",
            "ReplicationInstanceArn": "arn:aws:dms:us-east-2:698108604540:rep:mssqltopostgres",
            "ReplicationInstancePrivateIpAddress": "10.0.134.68",
            "ReplicationInstancePublicIpAddresses": [
                null
            ],
            "ReplicationInstancePrivateIpAddresses": [
                "10.0.134.68"
            ],
            "ReplicationInstanceIpv6Addresses": [],
            "PubliclyAccessible": false,
            "NetworkType": "IPV4"
        }
    ]
}
```

## Migration Task ##
### Task configuration ###

![Task configuration](pics/migrate/task/task-1-config.png)
```
"ReplicationTaskIdentifier": "maxattribute-maxattributecfg-ticket",
            "SourceEndpointArn": "arn:aws:dms:us-east-2:698108604540:endpoint:YIMVQSTQRHDU3C2F74TPSSL5GXOG6ILGLHPOLYI",
            "TargetEndpointArn": "arn:aws:dms:us-east-2:698108604540:endpoint:5KOC27HAARVBY7W3CXAQXRHFOT5KZPLQ6RH5ENY",
            "ReplicationInstanceArn": "arn:aws:dms:us-east-2:698108604540:rep:mssqltopostgres",
            "MigrationType": "full-load",
```


### Task settings ###

![Task settings](pics/migrate/task/task-2-settings.png)

```
{
  "Logging": {
    "EnableLogging": false,
    "EnableLogContext": false,
    "LogComponents": [
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "TRANSFORMATION"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "SOURCE_UNLOAD"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "IO"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "TARGET_LOAD"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "PERFORMANCE"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "SOURCE_CAPTURE"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "SORTER"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "REST_SERVER"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "VALIDATOR_EXT"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "TARGET_APPLY"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "TASK_MANAGER"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "TABLES_MANAGER"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "METADATA_MANAGER"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "FILE_FACTORY"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "COMMON"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "ADDONS"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "DATA_STRUCTURE"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "COMMUNICATION"
      },
      {
        "Severity": "LOGGER_SEVERITY_DEFAULT",
        "Id": "FILE_TRANSFER"
      }
    ],
    "CloudWatchLogGroup": null,
    "CloudWatchLogStream": null
  },
  "StreamBufferSettings": {
    "StreamBufferCount": 3,
    "CtrlStreamBufferSizeInMB": 5,
    "StreamBufferSizeInMB": 8
  },
  "ErrorBehavior": {
    "FailOnNoTablesCaptured": true,
    "ApplyErrorUpdatePolicy": "LOG_ERROR",
    "FailOnTransactionConsistencyBreached": false,
    "RecoverableErrorThrottlingMax": 1800,
    "DataErrorEscalationPolicy": "SUSPEND_TABLE",
    "ApplyErrorEscalationCount": 0,
    "RecoverableErrorStopRetryAfterThrottlingMax": true,
    "RecoverableErrorThrottling": true,
    "ApplyErrorFailOnTruncationDdl": false,
    "DataTruncationErrorPolicy": "LOG_ERROR",
    "ApplyErrorInsertPolicy": "LOG_ERROR",
    "EventErrorPolicy": "IGNORE",
    "ApplyErrorEscalationPolicy": "LOG_ERROR",
    "RecoverableErrorCount": -1,
    "DataErrorEscalationCount": 0,
    "TableErrorEscalationPolicy": "STOP_TASK",
    "RecoverableErrorInterval": 5,
    "ApplyErrorDeletePolicy": "IGNORE_RECORD",
    "TableErrorEscalationCount": 0,
    "FullLoadIgnoreConflicts": true,
    "DataErrorPolicy": "LOG_ERROR",
    "TableErrorPolicy": "SUSPEND_TABLE"
  },
  "ValidationSettings": {
    "ValidationPartialLobSize": 0,
    "PartitionSize": 10000,
    "RecordFailureDelayLimitInMinutes": 0,
    "SkipLobColumns": false,
    "FailureMaxCount": 10000,
    "HandleCollationDiff": false,
    "ValidationQueryCdcDelaySeconds": 0,
    "ValidationMode": "ROW_LEVEL",
    "TableFailureMaxCount": 1000,
    "RecordFailureDelayInMinutes": 5,
    "MaxKeyColumnSize": 8096,
    "EnableValidation": true,
    "ThreadCount": 5,
    "RecordSuspendDelayInMinutes": 30,
    "ValidationOnly": false
  },
  "TTSettings": null,
  "FullLoadSettings": {
    "CommitRate": 10000,
    "StopTaskCachedChangesApplied": false,
    "StopTaskCachedChangesNotApplied": false,
    "MaxFullLoadSubTasks": 8,
    "TransactionConsistencyTimeout": 600,
    "CreatePkAfterFullLoad": false,
    "TargetTablePrepMode": "TRUNCATE_BEFORE_LOAD"
  },
  "TargetMetadata": {
    "ParallelApplyBufferSize": 0,
    "ParallelApplyQueuesPerThread": 0,
    "ParallelApplyThreads": 0,
    "TargetSchema": "",
    "InlineLobMaxSize": 0,
    "ParallelLoadQueuesPerThread": 0,
    "SupportLobs": true,
    "LobChunkSize": 0,
    "TaskRecoveryTableEnabled": false,
    "ParallelLoadThreads": 0,
    "LobMaxSize": 32,
    "BatchApplyEnabled": false,
    "FullLobMode": false,
    "LimitedSizeLobMode": true,
    "LoadMaxFileSize": 0,
    "ParallelLoadBufferSize": 0
  },
  "BeforeImageSettings": null,
  "ControlTablesSettings": {
    "historyTimeslotInMinutes": 5,
    "HistoryTimeslotInMinutes": 5,
    "StatusTableEnabled": false,
    "SuspendedTablesTableEnabled": false,
    "HistoryTableEnabled": false,
    "ControlSchema": "",
    "FullLoadExceptionTableEnabled": false
  },
  "LoopbackPreventionSettings": null,
  "CharacterSetSettings": null,
  "FailTaskWhenCleanTaskResourceFailed": false,
  "ChangeProcessingTuning": {
    "StatementCacheSize": 50,
    "CommitTimeout": 1,
    "BatchApplyPreserveTransaction": true,
    "BatchApplyTimeoutMin": 1,
    "BatchSplitSize": 0,
    "BatchApplyTimeoutMax": 30,
    "MinTransactionSize": 1000,
    "MemoryKeepTime": 60,
    "BatchApplyMemoryLimit": 500,
    "MemoryLimitTotal": 1024
  },
  "ChangeProcessingDdlHandlingPolicy": {
    "HandleSourceTableDropped": true,
    "HandleSourceTableTruncated": true,
    "HandleSourceTableAltered": true
  },
  "PostProcessingRules": null
}
```

### Table mappings ###
![Table mappings](pics/migrate/task/task-3-table-mappings.png)

```
{
  "rules": [
    {
      "rule-type": "selection",
      "rule-name": "maxattribute-rule",
      "rule-id": "486202928",
      "object-locator": {
        "schema-name": "dbo",
        "table-name": "maxattribute"
      },
      "rule-action": "include",
      "load-order": "1",
      "filters": []
    },
    {
      "rule-type": "selection",
      "rule-id": "486202929",
      "rule-name": "maxattributecfg",
      "object-locator": {
        "schema-name": "dbo",
        "table-name": "maxattributecfg"
      },
      "rule-action": "include",
      "load-order": "2",
      "filters": []
    },
    {
      "rule-type": "selection",
      "rule-id": "486202930",
      "rule-name": "ticket",
      "object-locator": {
        "schema-name": "dbo",
        "table-name": "ticket"
      },
      "rule-action": "include",
      "load-order": "3",
      "filters": []
    }
  ]
}
```
