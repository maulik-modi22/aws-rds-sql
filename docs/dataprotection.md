# AWS RDS KMS Key Approach #
- Only supports Symmetric encryption
- Can only be used within AWS environments, customer must be having AWS account
- AWS KMS Encrypted backup cannot be restored to non-AWS environment

## AWS Managed keys ##
- Starts with aws/ prefix
- Exclusively used by AWS Managed services
- Cannot be used for your own operations 
- Per AWS Account per Region
- Can only be used for Entire RDS instance Snapshot Encryption
- Cannot be used for Individual Encrypted database backup

## Customer Managed keys ##

https://repost.aws/knowledge-center/s3-object-encryption-keys
https://repost.aws/knowledge-center/rds-sql-server-restore-kms-encrypted-backup
https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html
https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.TDE.html

MAS Manage only 