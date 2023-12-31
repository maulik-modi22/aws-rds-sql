## Purpose ##
Provide an accelerator/quickstart/guidance/SoP/confidence to adopt AWS RDS for SQL as a supportable relational database provider for MAS Manage+ Health and related industry solutions, add ons.

## Design goals/Requirements ##
- [x] One click deployment and Rollback using Declarative Infrastructure
- [x] Option for Atomic deployment (everything succeeds or everything fails)
- [x] Notification when deployment completes successfully or fails in the middle
- [x] Have Primitive validations in place for fail-fast/fast feedback
- [x] Recommend sensible defaults
- [x] [Scriptable database setup](docs/post-instance.md)
- [x] [User initiated backup/restore of database](docs/backup-restore.md)
- [ ] Performance monitoring and alerting
- [ ] Auditability (who did what, when)
- [ ] Data Protection (at-rest and in-transit) 
- [x] [Data Recoverability (to err is human!, outages do happen)](docs/data-recovery.md)
- [ ] Reporting/Read only replica
- [ ] Cost Estimation
