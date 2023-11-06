# Data Recoverability #
## Purpose ##
Data corruption can happen due to many reasons e.g. accidentially deletion/manipulation records from important tables. Also, hardware failure/outage may cause data loss. Hence, strong data recoverability strategy is required.

These settings require alignment with Service level agreemnts.

## Automated Backup ##
Configuring automated backup requires defining,
1. Retention period 
2. Backup window(30 minutes) in UTC 
3. (Optional) Replicating region

When Automated Backup is configured, screenshot may look like this
![Automated backup](pics/data-recovery/1-auto-backup-config.png)

Automated backups for Current region can be seen in this screen
![Current region](pics/data-recovery/2-current-region-backup.png)


`Daily`: Instance Level full backup during defined backup window

`Every 5 minutes`: Transaction log backup 

When automated backups are turned on for your DB instance, Amazon RDS automatically performs a `full daily snapshot of your data`. The snapshot occurs during your preferred backup window. It also captures `transaction logs to Amazon S3 every 5 minutes (as updates to your DB instance are made)`. Archiving the transaction logs is an important part of your DR process and PITR. 

When you initiate a point-in-time recovery, transactional logs are applied to the most appropriate daily backup in order to restore your DB instance to the specific requested time.

## PITR (Point in time recovery)
AWS RDS allows recovery upto last 30 minutes from current timestamp, refer this screenshot
![PITR](pics/data-recovery/3-pitr.png)

[More details](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIT.html)

