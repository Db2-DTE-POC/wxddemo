# Troubleshooting watsonx.data

Although we have tried to make the lab as error-free as possible, occasionally things will go wrong. Here is a list of common questions, problems, and potential solutions.

   * [What are the passwords for the serviers](#what-are-the-passwords-for-the-services)
   * [I Can't Open up a Terminal Window with VNC or Guacamole](#i-cant-open-up-a-terminal-window-with-vnc-or-guacamole)
   * [A SQL Statement failed but there are no error messages](#a-sql-statement-failed-but-there-are-no-error-messages)
   * [Apache Superset isn't Starting](#apache-superset-isnt-starting)
   * [Apache Superset screens differ from the lab](#apache-superset-screens-differ-from-the-lab)
   * [My VPN doesn't work](#my-vpn-doesnt-work)
   * [I am unable to use VPN](#i-am-unable-to-use-a-vpn)

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

If you downloaded a VPN certificate, and it doesn't appear to work, locate the file on your file system and attempt to view it. If the contents of the file contains the word `disabled`, this indicates that you did not request the VPN certificate to be enabled. You will need to request another image in order to connect with VPN. The other option is to use the Virtual console (guacamole) provided with the reservation. This requires that all exercises in this lab be done in that machine environment.

### I am unable to use a VPN

If you are blocked from using a VPN tunnel, you must use the guacamole interface provided in the reservation details (VM Remote Console). In this case, all URLs will need to be accessed using the Firefox browser that is in the image. Cut-and-paste only works inside the virtual machine so you must use the lab documentation inside the virtual machine.

![Browser](wxd-images/console-remote.png)

Once you click on the VM Remote Console button, the login screen for the image will be shown. Do not log into the `watsonx` userid yet!

![Browser](wxd-images/console-main.png)

You will not be able to log on as `watsonx` due to the VNC service that is currently running in the machine. If you try to login you will lock up the screen! If this happens you should reboot the server. 

Once you have the login screen, select `Techzone` from the list of users. Enter `IBMDem0s!` as the password. Once you have gained access to the system, click on Activities at the top of the screen and search for Terminal. Once a terminal screen has opened, issue the following instructions.

```
sudo su -
```
No password should be required.

```
systemctl stop vncserver@:1
systemctl disable vncserver@:1
exit
```

Log out of the Techzone userid and log into the `watsonx` user. At this point you can run the lab using this environment. Note that all URLs will continue to work in this image with IP addresses of `192.168.252.2`.

### Presto doesn't appear to be working

If you find that the watsonx.data UI is generating error messages that suggest that queries are not running, or that the Presto service is dead, you can force a restart of Presto with the following command:

```
restart_presto
```

This will stop the Presto server and restart it again. The command will also wait until the service is running before exiting.

