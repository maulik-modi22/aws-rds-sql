# IAM Role #
## Backup-Restore ##
### Purpose ###
To restore on-premise database in AWS RDS Database Isntance OR To take user initiated database backup of an existing database on AWS RDS Database Instance.

By default, AWS RDS Database Instance does not have access to any object storage resources in Customer's AWS account. Hence, Cloud Engineer/Administrator needs to create/designate an intermediate S3 Bucket, Prefix that acts as a staging location for database backup and associated IAM Service role that has permission to read/write from S3 bucket > Prefix.

### Steps ###
Create S3 Bucket & Prefix
Create IAM Service Role with policy shown below replacing MY-MAS-DB-BACKUP-BUCKET and MY-PREFIX
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::<<MY-MAS-DB-BACKUP-BUCKET>>"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObjectMetaData",
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListMultipartUploadParts",
                "s3:AbortMultipartUpload"
            ],
            "Resource": [
                "arn:aws:s3:::<<MY-MAS-DB-BACKUP-BUCKET>>/<<MY-PREFIX>>/*"
            ]
        }
    ]
}
```

## Auditing ##
Coming Soon!

## Granting permissions to Users ##
To start with, use built-in policies e.g. AWS managed policy: AmazonRDSReadOnlyAccess can be assigned so users can observe the infrastructure but cannot modify anything that affects the infrastructure.

[](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-security-iam-awsmanpol.html#rds-security-iam-awsmanpol-AmazonRDSReadOnlyAccess)
