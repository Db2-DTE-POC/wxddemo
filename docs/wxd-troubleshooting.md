# Troubleshooting watsonx.data

Although we have tried to make the lab as error-free as possible, occasionally things will go wrong. Here is a list of common questions, problems, and potential solutions.

   * [What are the passwords for the serviers](#what-are-the-passwords-for-the-services)
   * [I Can't Open up a Terminal Window with VNC or Guacamole](#i-cant-open-up-a-terminal-window-with-vnc-or-guacamole)
   * [A SQL Statement failed but there are no error messages](#a-sql-statement-failed-but-there-are-no-error-messages)
   * [Apache Superset isn't Starting](#apache-superset-isnt-starting)
   * [Apache Superset screens differ from the lab](#apache-superset-screens-differ-from-the-lab)
   * [My VPN doesn't work](#my-vpn-doesnt-work)

### What are the passwords for the services?

See the section on [Passwords](wxd-reference-passwords#passwords).

You can get all passwords for the system when you are logged in as the <code style="color:blue;font-size:medium;">watsonx</code> user by using the following command.
```
passwords
```

If you are logged in as the root user, the syntax is slightly different:
```
SSH_TTY=true SSH_CLIENT=true passwords
```

### I Can't Open up a Terminal Window with VNC or Guacamole

First thing to remember is that you can't use VNC and the TechZone Guacamole interface at the same time. Only one can be active at a time. If you start with VNC, you need to fully shut it down if you want to switch to the TechZone Guacamole version.

Commands to stop the VNC service are (as `root`):

```
systemctl stop vncserver@:1
systemctl disable vncserver@:1
```

#### A SQL Statement failed but there are no error messages

You need to use the Presto console <a href="http://192.168.252.2:8080" target="presto">http://192.168.252.2:8080</a> and search for the SQL statement. Click on the Query ID to find more details of the statement execution and scroll to the bottom of the web page to see any error details. 

### Apache Superset isn't Starting

If Superset doesn't start for some reason, you will need to reset it completely to try it again. First make sure you are connected as the `watsonx` user **not** `root`.

Make sure you have stopped the terminal session that is running Apache Superset. Next remove the Apache Superset directory.

```
sudo rm -rf /home/watsonx/superset
```

We remove the docker images associated with Apache Superset. If no containers or volumes exist you will get an error message.

```
docker ps -a -q --filter "name=superset" | xargs docker container rm --force
docker volume list -q  --filter "name=superset" | xargs docker volume rm --force
```

Download the superset code again.

```
git clone https://github.com/apache/superset.git
```

The `docker-compose-non-dev.yml` file needs to be updated so that Apache Superset can access the same network that watsonx.data is using. 

```
cd ./superset
cp docker-compose-non-dev.yml docker-compose-non-dev-backup.yml

sed '/version: "3.7"/q' docker-compose-non-dev.yml > yamlfix.txt
cat <<EOF >> yamlfix.txt
networks:
  default:
    external: True
    name: ibm-lh-network
EOF
sed -e '1,/version: "3.7"/ d' docker-compose-non-dev.yml  >> yamlfix.txt
```

We update the Apache Superset code to version `2.1.0`.
```
sed 's/\${TAG:-latest-dev}/2.1.0/' yamlfix.txt > docker-compose-non-dev.yml
```

Use docker-compose to start Apache Superset.
```
docker compose -f docker-compose-non-dev.yml up
```

### Apache Superset screens differ from the lab

The Apache Superset project makes frequent changes to the types of charts that are available. In some cases they remove or merge charts. Since these charts changes are dynamic, we are not able to guarantee that our examples will look the same as what you might have on your system.

### My VPN doesn't work

If you downloaded a VPN certificate and it doesn't appear to work, locate the file on your file system and attempt to view it. If the contents of the file contains the word `disabled`, this indicates that you did not request the VPN certificate to be enabled. You will need to request another image in order to connect with VPN. The other option is to use the Virtual console (guacamole) provided with the reservation. This requires that all exercises in this lab be done in that machine environment.


