# Monitoring / Auditing #

## Monitoring Engine errors / logs ##
Publishing trace and dump files isn't supported

1. Agent log - Supported
2. Error log - Supported
3. Trace - TBD
4. Dump - TBD

For more information [Logs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.SQLServer.html)

## Audit Trail ##

[RDS API Actions](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_Operations.html) Can be audited using [CloudTrail](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/logging-using-cloudtrail.html)

## RDS DB Instance Auditing ##
### Key Events of Interest ###
creation 
- Db instance created(RDS-EVENT-0005)

deletion
- Db instance deletion(RDS-EVENT-0003)

maintenance
- minor upgrade version available (RDS-EVENT-0155)
- engine version upgrade started (RDS-EVENT-0267)
- engine version upgrade finished (RDS-EVENT-0268)
- engine version upgrade failed (RDS-EVENT-0270)
- downtime started (RDS-EVENT-0266)

read replica
- Replication has stopped (RDS-EVENT-0045	)
- Replication for the Read Replica resumed (RDS-EVENT-0046)

snapshot events (creation)
- manaul snapshot created (RDS-EVENT-0042)
- automated snapshot created (RDS-EVENT-0091)

notification
- RDS-EVENT-0196
- RDS-EVENT-0197	


availability 
- instance start
- stop
- storage full

backup 
- backing up Db instance & finish
- configuration change - Db instance class change, allocation storage change, 

[More information](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Events.Messages.html#USER_Events.Messages.instance)

## SQL Server Auditing ##
### Key Audit Events Of Interest ###
[When Backup/Restore command is issued](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-backup-and-restore-event-class?view=sql-server-ver16) USE ``BACKUP_RESTORE_GROUP``

[When Server login is created](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-addlogin-event-class?view=sql-server-ver16) USE ``SERVER_PRINCIPAL_CHANGE_GROUP``

[When Server is started/stopped](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-server-starts-and-stops-event-class?view=sql-server-ver16) USE ``SERVER_STATE_CHANGE_GROUP``

[Server level GDR Event](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-server-scope-gdr-event-class?view=sql-server-ver16) USE ``SERVER_OBJECT_PERMISSION_CHANGE_GROUP``

[When principles are created, altered or dropped from a database](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-database-principal-management-event-class?view=sql-server-ver16) USE ``DATABASE_PRINCIPAL_CHANGE_GROUP``

[When user successfully logs in](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-login-event-class?view=sql-server-ver16) USE ``SUCCESSFUL_LOGIN_GROUP``

[When user login fails](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-login-failed-event-class?view=sql-server-ver16) USE ``FAILED_LOGIN_GROUP``



[When Audit specification is changed](https://learn.microsoft.com/en-us/sql/relational-databases/event-classes/audit-change-audit-event-class?view=sql-server-ver16), USE ``AUDIT_CHANGE_GROUP``

### Capturing Audit Events ###

[Sample Audit policy for SQL Server](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/DBActivityStreams.configuring-auditing-SQLServer.html)

[Monitoring Activity Streams](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/DBActivityStreams.Monitoring.html)