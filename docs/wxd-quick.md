# Quick Start

The following sections describe how to get started quickly with the watsonx.data developer system. If you are not familiar with the tools mentioned below, select the details link for more instructions.

* [Requesting an IBM userid](#ibm-userid)
* [Requesting a TechZone image](#requesting-a-techzone-image-individual-use)
* [Accessing the Image](#accessing-the-image-techzone)
* [Accessing the watsonx Challenge](#accessing-the-image-watsonx-challenge)
* [Wireguard Setup](#wireguard-setup)
* [VNC Access](#vnc-access)
* [SSH Access](#ssh-access)
* [Open Ports](#open-ports)
* [Passwords](#passwords)
* [Portainer Console](#portainer)
* [Documentation](#documentation)

## IBM Userid

An IBMid is needed to access IBM Technology Zone. If you do not have an IBMid, click on the following link and request a new IBMid.

<a href="https://techzone.ibm.com target="_blank">https://techzone.ibm.com</a>

More details: [Creating an IBM Userid](wxd-reference-ibmid.md)

## Requesting a TechZone image (Individual Use)

If you are running this lab as part of a workshop or the watsonx challenge, read the section below on [Accessing the Image (watsonx Challenge)](#accessing-the-image-watsonx-challenge).

Log into Techzone (<a href="https://techzone.ibm.com" target="_blank">https://techzone.ibm.com</a>) and search for the watsonx.data
Developer Base Image or use the following link.

[https://techzone.ibm.com/collection/ibm-watsonxdata-developer-base-image](https://techzone.ibm.com/collection/ibm-watsonxdata-developer-base-image)

Make sure to **enable VPN access** in the reservation. 

Problem with reservations failing? Check the TechZone status page at <a href="https://techzone.status.io" target="_blank">https://techzone.status.io</a>.

More details: [Reserving a TechZone image](wxd-reference-techzone.md)

## Accessing the Image (Techzone)

The email from TechZone indicating that the image is ready will contain a link to to your reservations. Click on the link and search for the watsonx.data reservation. Make sure to download the VPN certificate onto your machine. **Do not use the VM Remote Console button on the reservation.**

More details: [Accessing a TechZone image](wxd-reference-access.md)

## Accessing the Image (watsonx Challenge)

You will receive instructions from your team lead on how to access the image. You will be supplied with the VPN certificate required to gain access to the image from your team lead.

## Wireguard Setup

Install Wireguard (or similar utility) that allows a VPN certificate to be imported and used to access the virtual machine. Import the certificate that was downloaded in the previous step and activate the connection. There is no need to turn off IBM VPN when using Wireguard, but we recommend you turn off Wireguard when done with the lab.

More details: [Wireguard Setup](wxd-reference-wireguard.md)

## VNC Access

Once the Wireguard VPN service is enabled, you can access the machine console using the following IP address: <code style="font-size: medium;color:blue;">192.168.252.2:5901</code>. For Mac OSX users, place the following value into the Safari browser to access the console: <code style="font-size: medium;color:blue;">vnc://192.168.252.2:5901</code>.

The password for the VNC connection is <code style="font-size: medium;color:blue;">watsonx.data</code>.

More details: [VNC Access](wxd-reference-vnc.md)

## SSH Access

Open a terminal window and use the following syntax to connect as the <code style="font-size: medium;color:blue;">watsonx</code> userid.

```
ssh watsonx@192.168.252.2
```

To become the root user, issue the following command.
```
sudo su -
```
Password for both users is <code style="color:blue;font-size:medium;">watsonx.data</code>.

You can copy files into and out of the server using the following syntax:

```
scp myfile.txt watsonx@192.168.252.2:/tmp/myfile.txt
scp watsonx@192.168.252.2:/tmp/myfile.txt myfile.txt
```

More details: [SSH Access](wxd-reference-ssh.md)

## Open Ports

The following URLs and Ports are used to access the watsonx.data services.
The ports that are used in the lab are listed below.

   * <a href="https://192.168.252.2:9443" target="_blank">https://192.168.252.2:9443</a> - watsonx.data management console
   * <a href="http://192.168.252.2:8080" target="_blank">http://192.168.252.2:8080</a> - Presto console
   * <a href="http://192.168.252.2:9001" target="_blank">http://192.168.252.2:9001</a> - MinIO console (S3 buckets)
   * <a href="https://192.168.252.2:6443" target="_blank">https://192.168.252.2:6443</a> - Portainer (Docker container management)
   * <a href="http://192.168.252.2:8088" target="_blank">http://192.168.252.2:8088</a> - Apache Superset (Query and Graphing)
   * <code style="font-size: medium;color:blue;">vnc://192.168.252.2:5901</code> - VNC Access (Access to GUI in the machine)
   * <code style="font-size: medium;color:blue;">8443</code> - Presto External Port
   * <code style="font-size: medium;color:blue;">5432</code> - Postgres External Port
   * <code style="font-size: medium;color:blue;">50000</code> - Db2 Database Port   

**The Apache Superset link will not be active until started as part of the lab.**

More details: [Open Ports](wxd-reference-ports.md)

## Passwords

This table lists the passwords for the services that have "fixed" userids and passwords.

|Service|Userid|Password
|-------|------|--------|
|Virtual Machine|watsonx|watsonx.data
|Virtual Machine|root|watsonx.data
|watsonx.data UI|ibmlhadmin|password
|Presto|None|None
|Minio|Generated|Generated
|Postgres|admin|Generated
|Apache Superset|admin|admin
|Portainer|admin|watsonx.data
|Db2|db2inst1|db2inst1

Use the following commands to get the generated userid and password for MinIO.
```
export LH_S3_ACCESS_KEY=$(docker exec ibm-lh-presto printenv | grep LH_S3_ACCESS_KEY | sed "s/.*=//")
export LH_S3_SECRET_KEY=$(docker exec ibm-lh-presto printenv | grep LH_S3_SECRET_KEY | sed 's/.*=//') 
echo "MinIO Userid  : " $LH_S3_ACCESS_KEY
echo "MinIO Password: " $LH_S3_SECRET_KEY
```

Use the following command to get the password for Postgres.
```
export POSTGRES_PASSWORD=$(docker exec ibm-lh-postgres printenv | grep POSTGRES_PASSWORD | sed 's/.*=//')
echo "Postgres Userid   : admin"
echo "Postgres Password : " $POSTGRES_PASSWORD
```

You can get all passwords for the system when you are logged in as the <code style="color:blue;font-size:medium;">watsonx</code> user by using the following command.
```
passwords
```

If you receive the following message:
<pre style="font-size: medium; color: darkgreen; overflow: auto">
(zenity:29252): Gtk-WARNING **: 11:27:32.683: cannot open display:
</pre>

You will need to issue the command with the `text` option.
```
passwords text
```

**Note**: You cannot cut and paste a value into a VNC screen.

More details: [Passwords](wxd-reference-passwords.md)

## Portainer

This lab system has Portainer installed. Portainer provides an administrative interface to the Docker images that are running on this system. You can use this console to check that all the containers are running and see what resources they are using. Open your browser and navigate to:

   * Portainer console - <a href="https://192.168.252.2:6443" target="_blank">https://192.168.252.2:6443</a>
   * Credentials: userid: <code style="font-size: medium;color:blue;">admin</code> password: <code style="font-size: medium;color:blue;">watsonx.data</code>

More details: [Portainer](wxd-reference-portainer.md)   

## Documentation

The following links provide more information on the components in this lab.

* watsonx.data - <a href="https://www.ibm.com/docs/en/watsonxdata/1.0.x" target="_blank">https://www.ibm.com/docs/en/watsonxdata/1.0.x</a>
* Presto SQL - <a href="https://prestodb.io/docs/current/sql.html" target="_blank">https://prestodb.io/docs/current/sql.html</a>
* Presto Console - <a href="https://prestodb.io/docs/current/admin/web-interface.html" target="_blank">https://prestodb.io/docs/current/admin/web-interface.html</a>
* MinIO - <a href="https://min.io/docs/minio/linux/administration/minio-console.html" target="_blank">https://min.io/docs/minio/linux/administration/minio-console.html</a>
* Apache Superset - <a href="https://superset.apache.org/docs/creating-charts-dashboards/exploring-data" target="_blank">https://superset.apache.org/docs/creating-charts-dashboards/exploring-data</a>
* dBeaver - <a href="https://dbeaver.com/docs/wiki/Application-Window-Overview/" target="_blank">https://dbeaver.com/docs/wiki/Application-Window-Overview/</a>
* Db2 SQL - <a href="https://www.ibm.com/docs/en/db2/11.5?topic=queries-select-statement" target="_blank">https://www.ibm.com/docs/en/db2/11.5?topic=queries-select-statement</a>
* PostgreSQL SQL - <a href="https://www.postgresql.org/docs/current/sql.html" target="_blank">https://www.postgresql.org/docs/current/sql.html</a>
