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

https://www.redhat.com/en/blog/running-microsoft-sql-server-2019-openshift-using-red-hat-openshift-container-storage

## Screenshots ##
### Security group - Inbound rules ###
![route tables](pics/connectivity/2-route-tables.png)

### Connectivity from the cluster ###
![connectivity test](pics/connectivity/1-connectivity-test.png)

### SSL Certificate related ###


### Bounce Maxinst ###
 oc scale --replicas=0 deployment/awstrn-rel-main-manage-maxinst

 oc scale --replicas=1 deployment/awstrn-rel-main-manage-maxinst

 ### Validation ##
#### Maxinst ####
```
Calling isValidDBConnection ...
Validating DB connection from script /opt/IBM/SMP/maximo/run-db.sh using the querycount utility to check for the maxvars table.
DB Connection return code = 0
Note: A return code of 0 (table found) or 1 (table not found) indicates a successful query via a valid DB connection and that an exception has not occurred. Otherwise, there's a problem connecting to the database.
isvalidconn.log

==================
DEBUG***:queryType=table
Found
==================
DB Validation is successful
status:dbconnex:success
Calling prepareDBC ...
Preparing DBC's
delserversession.dbc
```

```
BMXAA6818I - ValidateCryptoKey started for schema dbo, connected to database jdbc:sqlserver://sas05neuce2.cux6dwui9qfk.eu-central-1.rds.amazonaws.com:1433;encrypt=true;databaseName=eamdb76; Thu Jan 04 13:25:33 GMT 2024
...... Start to validate CRYPTO...... with key SP*****sa Thu Jan 04 13:25:33 GMT 2024
New crypto = SPoxAXzoSGbblqFwGVdpFdsa false Thu Jan 04 13:25:34 GMT 2024
Validating CRONTASKPARAM.CRYPTOVALUE Thu Jan 04 13:25:34 GMT 2024
Validating DDUSERAUTH.KEYID Thu Jan 04 13:25:34 GMT 2024
Validating DMPKGDSTTRGT.PASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.EMAILPASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHACCESSTOKEN Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHCLIENTID Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHCLIENTSECRET Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHREFRESHTOKEN Thu Jan 04 13:25:34 GMT 2024
Validating MAXENDPOINTDTL.PASSWORD Thu Jan 04 13:25:34 GMT 2024
/**** system#major A major exception has occurred. Check the system log to see if there are any companion errors logged. Report this error to your system administrator. ****/
psdi.util.MXApplicationException: system#major
at psdi.tools.ValidateCryptoKey.validate(ValidateCryptoKey.java:182)

...... Start to validate CRYPTO...... with key SP*****sa Thu Jan 04 13:25:33 GMT 2024
...... Start to validate CRYPTO...... with key SP*****sa Thu Jan 04 13:25:33 GMT 2024
New crypto = SPoxAXzoSGbblqFwGVdpFdsa false Thu Jan 04 13:25:34 GMT 2024
New crypto = SPoxAXzoSGbblqFwGVdpFdsa false Thu Jan 04 13:25:34 GMT 2024
Validating CRONTASKPARAM.CRYPTOVALUE Thu Jan 04 13:25:34 GMT 2024
Validating CRONTASKPARAM.CRYPTOVALUE Thu Jan 04 13:25:34 GMT 2024
Validating DDUSERAUTH.KEYID Thu Jan 04 13:25:34 GMT 2024
Validating DDUSERAUTH.KEYID Thu Jan 04 13:25:34 GMT 2024
Validating DMPKGDSTTRGT.PASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating DMPKGDSTTRGT.PASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.EMAILPASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.EMAILPASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHACCESSTOKEN Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHACCESSTOKEN Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHCLIENTID Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHCLIENTID Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHCLIENTSECRET Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHCLIENTSECRET Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHREFRESHTOKEN Thu Jan 04 13:25:34 GMT 2024
Validating INBOUNDCOMMCFG.OAUTHREFRESHTOKEN Thu Jan 04 13:25:34 GMT 2024
Validating MAXENDPOINTDTL.PASSWORD Thu Jan 04 13:25:34 GMT 2024
Validating MAXENDPOINTDTL.PASSWORD Thu Jan 04 13:25:34 GMT 2024
system#major A major exception has occurred. Check the system log to see if there are any companion errors logged. Report this error to your system administrator.
psdi.util.MXApplicationException: system#major
at psdi.tools.ValidateCryptoKey.validate(ValidateCryptoKey.java:182)
at psdi.tools.ValidateCryptoKey.validateKey(ValidateCryptoKey.java:119)
at psdi.tools.ValidateCryptoKey.process(ValidateCryptoKey.java:82)
at psdi.tools.ValidateCryptoKey.main(ValidateCryptoKey.java:271)
Caused by: java.lang.Exception
... 4 more
/**** system#major A major exception has occurred. Check the system log to see if there are any companion errors logged. Report this error to your system administrator. ****/
system#major Thu Jan 04 13:25:34 GMT 2024
system#major Thu Jan 04 13:25:34 GMT 2024
BMXAA6819I - ValidateCryptoKey completed with errors. Thu Jan 04 13:25:34 GMT 2024
BMXAA6819I - ValidateCryptoKey completed with errors. Thu Jan 04 13:25:34 GMT 2024
Failed: Crypto Keys do not match what database uses. UpdateDB will not run.
status: invalid keys
```

