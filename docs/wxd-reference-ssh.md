# SSH Access

You have the choice of using the VNC connection to the virtual machine, or using a local terminal shell (iterm, Hyper, terminal) to run commands against the watsonx.data server.

To connect to the server, open a terminal session and issue the following command to connect as the <code style="color:blue;font-size:medium;">watsonx</code> user:

```
ssh watsonx@192.168.252.2
```

When connected as the <code style="color:blue;font-size:medium;">watsonx</code> user, you can become the root user by entering the following command in the terminal window.
```
sudo su -
```

The password for watsonx and root is <code style="color:blue;font-size:medium;">watsonx.data</code>. 

You can have multiple connections into the machine at any one time. You may find it is easier to cut-and-paste commands into a local terminal shell rather than using the VNC console.

## Copying Files

If you need to move files into or out of the virtual machine, you can use the following commands.

To copy a file into the virtual machine use the following syntax:

```
scp myfile.txt watsonx@192.168.252.2:/tmp
```

The filename `myfile.txt` will be copied to the `/tmp` directory. The temporary directory is useful since you can copy the file to multiple places from within the Linux environment.

Multiple files can be moved by using wildcard characters using the following syntax:

```
scp myfile.* watsonx@192.168.252.2:/tmp
```

To move files from the image back to your local system requires you reverse the file specification.

```
scp watsonx@192.168.252.2:/tmp/myfile.txt /Downloads/myfile.txt
```

You can also use wildcards to select more than one file.