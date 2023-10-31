# Password Management #
## Managing password with AWS Secret manager ##
According to this reference,
[In Read Replica configuration, AWS RDS for SQL Server does not support credential management with Secret Manager](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-secrets-manager.html)

## Password policy ##
According to this reference,[When AWS RDS for SQL Server is not joined to Active directory, it allows <code style="color : red"> minimum password length:0 </code>](https://repost.aws/knowledge-center/rds-sql-server-configure-password-policy)

Password policy is Enforced only when `Windows Authentication` is used for RDS for SQL Server connected with `Active Directory`