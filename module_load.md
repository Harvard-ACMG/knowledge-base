# Transferring files between your Unix environment and your PC

**NOTE: The Odyssey cluster was renamed to Cannon in September 2019.**

There are several options available for copying data to and from your PC
from AS or Cannon.

1.  Copying data using scp
2.  SFTP transfer using Filezilla
3.  WinSCP file transfer for Windows users

## Copying data using scp

You can transfer data from a terminal window, like the Mac OSX Terminal
application or a Linux system, using the `scp` command.

``` 
  scp YOUR_LOGIN_ID@REMOTE_HOST:REMOTE_FILE_PATH LOCAL_FILE_PATH
```

## SFTP transfer using Filezilla

FileZilla is a free and open source SFTP client that is available for
Mac, Windows, and Linux. It provides a graphical interface for
transferring files between Unix environments and your PC.

### Download Filezilla

Download and install Filezilla by visiting
<https://filezilla-project.org/>.

### Create a new site

**1.** Launch Filezilla and click on the **Site Manager** icon.

**2.** Click **New Site**.

**3.** With the General tab selected, enter the following information:

    *For AS
      * **Host:** ''YOUR_LOGIN_ID.as.harvard.edu'' or ''login.as.harvard.edu''
      * **Protocol:** SFTP - SSH File Transfer Protocol
      * **Login Type:** Interactive
      * **User:** enter your AS username

    *For Odyssey
      * **Host:** ''cannon.rc.fas.harvard.edu'' or ''login.rc.fas.harvard.edu''
      * **Protocol:** SFTP - SSH File Transfer Protocol
      * **Login Type:** Interactive
      * **User:** enter your Research Computing username

**4.** In the Advanced tab, specify the **Default Local Directory** you
want to use on your PC. You can click the Browse button to find a
specific directory. Leave the **Default remote directory** blank to open
the remote connection in your home directory.

**5.** In the Transfer tab, check **Limit number of simultaneous
connections** and set **Maximum number of connections** to 1.

**6.** Click **Connect** to connect to the remote Unix environment. If
this is the first time connecting, check **Always trust this host, add
this key to the cache** in the Unknown host key window.

**7.** Enter your password when prompted and check **Remember password
until FileZilla is closed**.

**8.** If connecting to Cannon, you will also be prompted for your
verification code. Enter the code shown in OpenAuth or Google
Authenticator and click OK.

### Transferring files

You should now be connected to the remote Unix environment (AS or
Odyssey). Your local PC files are located in the left pane and the
remote files are located in the right pane. To transfer files, you can
drag and drop them from one pane to another. You can also drag and drop
files directly to your desktop or local folder.

### Closing the connection

Click the red X icon in the toolbar to close the connection.

## WinSCP file transfer for Windows users

For more information on WinSCP, see <http://winscp.net>.

\--- *GEOS-Chem Support Team 2019/09/16 18:56*
