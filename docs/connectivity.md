## Checklist ##
- [x] Verify that Remote connections are enabled
- [x] Verify that SQL Server and Windows authentication is enabled
- [x] Verify that TCP/IP is enabled (Sql server network configuration)
- [x] Verify that instance is listening on specific port e.g. 1433 (TCP/IP Properties > IPAll > remove dynamic port)
- [x] Verify that port is allowed in Inbound firewall rule (use wf.msc in RUN)
- [x] Verify that hyperscaler VM > Security group has SQL Server specific port opened
- [x] Verify that credentials are correct and user has access to the database
- [x] Verify connectivity using client tool from the same network if possible e.g.
`tnc <<IP | DNS>> -port <<MSSQL Specific Port>>`
- [x] Verify that database is ONLINE

https://www.redhat.com/en/blog/running-microsoft-sql-server-2019-openshift-using-red-hat-openshift-container-storage
