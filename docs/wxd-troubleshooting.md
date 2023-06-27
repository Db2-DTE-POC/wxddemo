# Troubleshooting IBM watsonx.data

Although we have tried to make the lab as error-free as possible, occasionally things will go wrong. Here is a list of common questions, problems, and potential solutions.

#### I tried to access the IBM watsonx.data console and it isn't working

The base image does not start the IBM watsonx.data services. You need to start all the services first before you can access the IBM watsonx.data console, along with the Presto and MinIO consoles. Make sure that the `LH_RUN_MODE` environment variable has been
set before starting the services. Without this setting, the non-SSL ports will be blocked and you will be unable to reach many of the services in the system.
<code class="language-bash"><pre>
export LH_RUN_MODE=diag
./stop.sh
./start.sh
./checkpresto.sh
</pre></code>

#### The command I typed in says "not found" or parameter unknown

1.	This error often occurs when you copy a command from Word (written with an English keyboard) and paste it into a "local" terminal window that is using a different code page. We have seen this with the "-" character which Windows sometimes turns into a "dash" rather than a "minus" sign. If you use the Brower-based SSH window, this cut and paste error normally does not appear. However, copying this into a terminal window using your local operating system can sometime cause problems. Typing the command "manually" will  work.

2.	Cut and Paste into some fields may cause errors if the text contains an extra white space which is not a blank. For instance, the dBeaver program does not strip these whitespace characters from a parameter name. For instance, if you placed an extra "tab" character at the end of the SSH parameter, the program would issue an error message that says it can't find the parameter.

3.	For all Presto and IBM watsonx.data commands, you must be the root user and be in the `/root/ibm-lh-data/bin directory` to issue these commands.
 
#### I have started IBM watsonx.data but no services are reachable

Mostly likely reason is that the system was started without the run mode setting changed to diagnostic mode. The setting is required to open all non-SSL ports (MinIO, Presto, etc...). Run the following command in the terminal window where you started the program. 
<code class="language-bash"><pre>
env | grep LH_RUN_MODE
</pre></code>

If the results are empty, then you should stop and restart the service with the following commands.
<code class="language-bash"><pre>
export LH_RUN_MODE=diag
./stop.sh
./start.sh
./checkpresto.sh
</pre></code>

#### Unable to connect to server (dBeaver)

Double-check the server address that you are using in the configuration. If you are using dBeaver "inside" the IBM watsonx.data virtual machine, the server is `localhost`. If you are connecting with your own copy of dBeaver, you need to use `ibm-lh-presto-svc` and update your host command to point to your TechZone server location.

#### What are the passwords for the services?

This table lists the passwords for the services that have "fixed" userids and passwords.

|Service|Userid|Password
|-------|------|--------|
|Virtual Machine|watsonx|watsonx.data
|Virtual Machine|root|watsonx.data
|IBM watsonx.data UI|ibmlhadmin|password
|Presto|None|None
|Minio|Generated|Generated
|Postgres|admin|Generated
|Apache Superset|admin|admin
|Portainer|admin|watsonx.data

Use the following commands to get the generated userid and password for MinIO.
<code class="language-bash"><pre>
export LH_S3_ACCESS_KEY=$(docker exec ibm-lh-presto printenv | grep LH_S3_ACCESS_KEY | sed 's/.*=//')
export LH_S3_SECRET_KEY=$(docker exec ibm-lh-presto printenv | grep LH_S3_SECRET_KEY | sed 's/.*=//')
echo "MinIO Userid  : " $LH_S3_ACCESS_KEY
echo "MinIO Password: " $LH_S3_SECRET_KEY
</pre></code>

Use the following command to get the password for Postgres.
<code class="language-bash"><pre>
export POSTGRES_PASSWORD=$(docker exec ibm-lh-postgres printenv | grep POSTGRES_PASSWORD | sed 's/.*=//')
echo "Postgres Userid   : admin"
echo "Postgres Password : " $POSTGRES_PASSWORD
</pre></code>

#### I Can't Open up a Terminal Window with VNC or Guacamole

First thing to remember is that you can't use VNC and the TechZone Guacamole interface at the same time. Only one can be active at a time. If you start with VNC, you need to fully shut it down if you want to switch to the TechZone Guacamole version.

If you find that you cannot start a terminal session (a window never gets displayed), you will need to log out as the watsonx user. This problem is a result of the SSH Browser having started before the Gnome UI. The fix requires that you click the power button at the top of the display and then selecting log out. This will reset the UI and allow you to use the terminal window again.

#### I deleted a bucket and I can't seem to add it back

If you delete a bucket from the IBM watsonx.data system, the bucket name remains in the catalog (for now) which prevents you from re-creating it. Use a different name for the bucket when cataloging it. This limitation will be removed in a future build.