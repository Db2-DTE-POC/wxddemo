# watsonx.data Ports

The following URLs and Ports are used to access the watsonx.data services.
The ports that are used in the lab are listed below.

   * <a href="https://192.168.252.2:9443" target="_blank">https://192.168.252.2:9443</a> - watsonx.data management console
   * <a href="http://192.168.252.2:8080" target="_blank">http://192.168.252.2:8080</a> - Presto console
   * <a href="http://192.168.252.2:9001" target="_blank">http://192.168.252.2:9001</a> - MinIO console (S3 buckets)
   * <a href="https://192.168.252.2:6443" target="_blank">https://192.168.252.2:6443</a> - Portainer (Docker container management)
   * <a href="http://192.168.252.2:8088" target="_blank">https://192.168.252.2:8088</a> - Apache Superset (Query and Graphing)
   * <code style="color:blue;font-size:medium;">vnc://192.168.252.2:5901</code> - VNC Access (Access to GUI in the machine)
   * <code style="color:blue;font-size:medium;">8443</code> - Presto External Port
   * <code style="color:blue;font-size:medium;">5432</code> - Postgres External Port
   * <code style="color:blue;font-size:medium;">50000</code> - Db2 Database Port

**The Apache Superset link will not be active until started as part of the lab.**

Some of the links will result in a Certificate error in Firefox:

![Browser](wxd-images/browser-warning-1.png)
 
Select Advanced.

![Browser](wxd-images/browser-warning-2.png)
 
Choose “Accept the Risk and Continue”. If you are using Google Chrome, you can bypass the error message by typing in “thisisunsafe” or clicking on the "Proceed to 192.168.252.2 (unsafe)" link.

![Browser](wxd-images/chrome-browser.png)