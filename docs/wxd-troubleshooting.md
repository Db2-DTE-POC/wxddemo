# Troubleshooting watsonx.data

Although we have tried to make the lab as error-free as possible, occasionally things will go wrong. Here is a list of common questions, problems, and potential solutions.

#### What are the passwords for the services?

See the section on [Passwords](wxd-reference-passwords#passwords).

You can get all passwords for the system when you are logged in as the <code style="color:blue;font-size:medium;">watsonx</code> user by using the following command.
```
passwords
```

If you are logged in as the root user, the syntax is slightly different:
```
SSH_TTY=true SSH_CLIENT=true passwords
```

#### I Can't Open up a Terminal Window with VNC or Guacamole

First thing to remember is that you can't use VNC and the TechZone Guacamole interface at the same time. Only one can be active at a time. If you start with VNC, you need to fully shut it down if you want to switch to the TechZone Guacamole version.

Commands to stop the VNC service are (as `root`):

```
systemctl stop vncserver@:1
systemctl disable vncserver@:1
```

#### A SQL Statement failed but there are no error messages

You need to use the Presto console <a href="http://192.168.252.2:8080" target="presto">https://192.168.252.2:8080</a> and search for the SQL statement. Click on the Query ID to find more details of the statement execution and scroll to the bottom of the web page to see any error details. 