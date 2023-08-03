# Data Recoverability #
## Purpose ##
Data corruption can happen due to many reasons e.g. accidentially deletion/manipulation records from important tables. Hence, strong data recoverability strategy is required.

## Automated Backup ##
Configuring automated backup requires defining,
1. Retention period 
2. Backup window(30 minutes) in UTC 
3. (Optional) Replicating region

When Automated Backup is configured, screenshot may look like this
![Automated backup]('pics/data-recovery/1-auto-backup-config.png')

Automated backups for Current region can be seen in this screen
![Automated backup]('pics/data-recovery/2-current-region-backup.png')

## PITR (Point in time recovery)
AWS RDS allows recovery upto last 5 minutes from current timestamp, refer this screenshot
![Automated backup]('pics/data-recovery/3-pitr.png')
