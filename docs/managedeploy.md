jdbc:db2://c-awstrn-reldb2umanage-db2u-engn-svc.mas-awstrn-rel-db2u-manage.svc:50001/BLUDB:sslConnection=true;sslVersion=TLSv1.2;

jdbc:sqlserver://sas05neuce2.cux6dwui9qfk.eu-central-1.rds.amazonaws.com:1433;database=eamdb76;encrypt=true;trustServerCertificate=false

maxdbowner
apjklPPmn!e23@1234567890

##
https://truststore.pki.rds.amazonaws.com/eu-central-1/eu-central-1-bundle.pem

## Bring your own database ###
export MAS_INSTANCE_ID=inst1
export MAS_CONFIG_DIR=~/masconfig
export IBM_ENTITLEMENT_KEY=xxx

export CONFIGURE_EXTERNAL_DB=true
export DB_INSTANCE_ID=maxdbxx
export MAS_JDBC_USER=maxdbowner
export MAS_JDBC_PASSWORD=apjklPPmn!e23@1234567890
export MAS_JDBC_URL=xxx

export MAS_APP_SETTINGS_DB2_SCHEMA=maximo
export MAS_APP_SETTINGS_TABLESPACE=maxdata
export MAS_APP_SETTINGS_INDEXSPACE=maxindex

## Additional variables
export MAS_CONFIG_SCOPE=
export MAS_JDBC_CERT_LOCAL_FILE=xxx
export SSL_ENABLED=true

oc login --token=xxxx --server=https://myocpserver
ansible-playbook ibm.mas_devops.oneclick_add_manage
