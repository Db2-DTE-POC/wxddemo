# Database Connections

There are three database systems that can be accessed inside and outside the virtual machine environment:

   * watsonx.data Presto engine
   * Db2 LUW
   * PostgreSQL

In order to access these images outside the Virtual machine image, you must extract the certificates and add a host name to your workstation.

## watsonx.data Presto Access

When connecting to the Presto engine, choose the PrestoDB driver.

### Presto Internal Access

For local access the following credentials are used:

   * Hostname: <code style="color:blue;font-size:medium;">localhost</code>
   * Port: <code style="color:blue;font-size:medium;">8443</code>
   * Username: <code style="color:blue;font-size:medium;">ibmlhadmin</code>
   * Password: <code style="color:blue;font-size:medium;">password</code>
   * Database: <code style="color:blue;font-size:medium;">tpch</code>

In addition, you need to set the following driver properties:

   * <code style="color:blue;font-size:medium;">SSL</code> <code style="color:blue;font-size:medium;">True</code>
   * <code style="color:blue;font-size:medium;">SSLTrustStorePath</code> <code style="color:blue;font-size:medium;">/certs/presto-key.jks</code>
   * <code style="color:blue;font-size:medium;">SSLTrustStorePassword</code> <code style="color:blue;font-size:medium;">watsonx.data</code>

### Presto External Access

The watsonx.data Presto database requires that the certificate be extracted from the image and a host name be added to your workstation. 

The first step is to extract the Presto certificate onto your local file system.

```
scp watsonx@192.168.252.2:/certs/presto-key.jks /Users/myname/Downloads
```

Change the target directory to a location that you can remember! Next add the following host information to your local hosts file. You may need to authenticate on your workstation in order to change the file.

```
echo '192.168.252.2' watsonxdata | sudo tee -a /etc/hosts
```

The database connection settings are:

   * Hostname: <code style="color:blue;font-size:medium;">watsonxdata</code>
   * Port: <code style="color:blue;font-size:medium;">8443</code>
   * Username: <code style="color:blue;font-size:medium;">ibmlhadmin</code>
   * Password: <code style="color:blue;font-size:medium;">password</code>
   * Database: <code style="color:blue;font-size:medium;">tpch</code>

In addition, you need to set the following driver properties:

   * <code style="color:blue;font-size:medium;">SSL</code> <code style="color:blue;font-size:medium;">True</code>
   * <code style="color:blue;font-size:medium;">SSLTrustStorePath</code> <code style="color:blue;font-size:medium;">/mydownload/presto-key.jks</code>
   * <code style="color:blue;font-size:medium;">SSLTrustStorePassword</code> <code style="color:blue;font-size:medium;">watsonx.data</code>

**Note**: The `/mydownload/presto-key.jks` value needs to be replaced with the location that you copied the key in the earlier step.

## Db2 Access

When connecting to the Db2 engine, select the Db2 LUW driver.

### Db2 Internal Access

The Db2 server can be accessed on port 50000 inside the virtual machine using the following credentials:

   * Hostname - <code style="color:blue;font-size:medium;">watsonxdata</code>
   * Port - <code style="color:blue;font-size:medium;">50000</code>
   * Username - <code style="color:blue;font-size:medium;">db2inst1</code>
   * Password - <code style="color:blue;font-size:medium;">db2inst1</code>
   * Database - <code style="color:blue;font-size:medium;">gosales</code>
   * SSL - <code style="color:blue;font-size:medium;">off</code>

### Db2 External Access

When accessing the database outside the virtual machine, you must change the host to 192.168.252.2. All of the other settings remain the same.

   * Hostname - <code style="color:blue;font-size:medium;">192.168.252.2</code>
   * Port - <code style="color:blue;font-size:medium;">50000</code>
   * Username - <code style="color:blue;font-size:medium;">db2inst1</code>
   * Password - <code style="color:blue;font-size:medium;">db2inst1</code>
   * Database - <code style="color:blue;font-size:medium;">gosales</code>
   * SSL - <code style="color:blue;font-size:medium;">off</code>

## PostgreSQL Access

When connecting to the PostgreSQL engine, select the PostgreSQL driver. In order to connect to the PostgreSQL system, you will need to extract the admin password using the following command when connected to the watsonx.data system.

```
export POSTGRES_PASSWORD=$(docker exec ibm-lh-postgres printenv | grep POSTGRES_PASSWORD | sed 's/.*=//')
echo "Postgres Userid   : admin"
echo "Postgres Password : " $POSTGRES_PASSWORD
echo $POSTGRES_PASSWORD > /tmp/postgres.pw
```

### PostgreSQL Internal Access

When accessing the PostgreSQL database in the system, use the following settings.

   * Hostname –<code style="color:blue;font-size:medium;">ibm-lh-postgres</code>
   * Port – <code style="color:blue;font-size:medium;">5432</code>
   * Username – <code style="color:blue;font-size:medium;">admin</code>
   * Password – The value that was extracted in the earlier step
   * Database – <code style="color:blue;font-size:medium;">gosales</code>   

### PostgreSQL External Access

The following credentials are used for remote access.

   * Hostname: <code style="color:blue;font-size:medium;">192.168.252.2</code>
   * Port: <code style="color:blue;font-size:medium;">5432</code>
   * Username: <code style="color:blue;font-size:medium;">admin</code>
   * Password: The value that was extracted in the earlier step
   * Database name: <code style="color:blue;font-size:medium;">gosales</code>

