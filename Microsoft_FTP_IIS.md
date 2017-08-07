How to install Microsoft FTP Server (IIS) with multiple user accounts, each having their own separate home directories. Should work under any recent windows version.

### Prerequisites / Install components

* Create some local user accounts that will later be used to access the server w/ FTP
* With the Server Manager Tool install the Role "Webserver (IIS)", including Managment Tools
* One the details page, select FTP Server, FTP Service and FTP Extensibility

### Setup the basic FTP Site

* Open IIS Management Console
* Create a new FTP Site named "MainFTP" pointing to "C:\inetpub\ftproot"
* Next, leave IP unassigned and disable SSL (this being a simple manual for using FTP internally)
* Select Basic Authentication and Allow access to Specified users
* For the username you can take your local user w/ read/write permissions for example to be able to proceed
* Finish the wizard

### Adjust IIS FTP Settings

* Under the newly created "MainFTP" Site select user isolation
* Choose "Isolate Users" - "User name directory"
* Under "MainFTP", create a new virtual directory named "LocalUser" pointing to "C:\inetpub\ftproot" (must be named "LocalUser" to work correctly)

### Per-User Steps

* Under LocalUser, create a new virtual directory matching the username
* Point the physical path to where you want
* Double click the "FTP Authorization Rules" in the newly created virtual directory
* Click "Add Allow Rule"
* At specific users, insert the corresponding username and set permissions as wanted
* Make sure the user also has the right NTFS permissions in the file system for this directories

## Sources

http://www.sherweb.com/blog/how-to-set-up-ftp-access-for-multiple-users-with-user-isolation/
