## Pre-requisites ##
Read replica requires meeting several pre-requisites on the source Db instance
![screenshot](pics/read-replica/1-pre-req.png)

![screenshot](pics/read-replica/1.2-auto-backup.png)

 1. Engine MUST BE SQL Server Enteprise Edition
 2. Multi-AZ MUST BE Yes
 3. Automated backup MUST BE ON

## Creating Read replica instance ##
![screenshot](pics/read-replica/2-actions-rr.png)

Specify necessary options, It will show it as **creating** and **Replica** as role below **Primary** instance
![screenshot](pics/read-replica/4-creating.png)

## Checking read replica instance  ##
After some time, Read replica instance will show up as **created**
![Screenshot](pics/read-replica/5-created.png)

Also, we can check **Replication** tab showing **Replication Source** and **Replication State**
![screenshot](pics/read-replica/6-replicating.png)