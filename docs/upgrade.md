# Upgrade #
## Major Version Support Dates ##
|SQL Server Release|Mainstream End Date|Major Version|
|-----|------|-----|
|2019|Feb 28, 2025|15.0
|2022|Jan 11, 2028|16.0

[SQL Server 2019 Support cycle](https://learn.microsoft.com/en-us/lifecycle/products/sql-server-2019)

[SQL Server 2022 Support cycle](https://learn.microsoft.com/en-us/lifecycle/products/sql-server-2022)

## Microsoft SQl Server licensing limits  ##
For MAS SaaS, We are considering only `Standard` and `Enterprise` editions

|Edition|Read only Replica|Online indexing|Passive Failover Replica|Max CPU core|Max Memory(GB)|Max Db Size(PB)|
|-----|------|-----|-----|-----|-----|----|
|Standard|NO|NO|YES|24|128|524
|Enterprise|YES|YES|YES|OS maximum|OS maximum|524

Refer this link for [full comparison of features](https://learn.microsoft.com/en-us/sql/sql-server/editions-and-components-of-sql-server-2019?view=sql-server-ver15&preserve-view=true)

Refer similar link for [AWS RDS SQL Standard/Enterprise features](https://docs.aws.amazon.com/prescriptive-guidance/latest/evaluate-downgrading-sql-server-edition/compare.html)

Refer this link for [AWS RDS SQL Instance types](https://aws.amazon.com/rds/sqlserver/pricing/?nc=sn&loc=4)

## Minor Version ##
|SQL Server Release|Build version|Major Version|
|-----|------|-----|
|2019|Feb 28, 2025|15.0
|2022|Jan 11, 2028|16.0

## Minor Version Upgrade ##
### Knowing