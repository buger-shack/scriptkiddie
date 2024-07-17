# Files to look for

<figure><img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExMjNjNDBmOTJlNzRiMDhkYjk0OWFhZDMyZWY3MzVkYzE4NjU5NTgxMyZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/3o6MblAa0uE3a77V5K/giphy.gif" alt="" width="375"><figcaption></figcaption></figure>

## Linux

{% embed url="https://github.com/tennc/fuzzdb/blob/master/dict/BURP-PayLoad/LFI/LFI-LogFileCheck.txt" %}
LogFile check
{% endembed %}

* `/etc/issue` : contains a message or system identification to be printed before the login prompt.
* `/etc/profile` : controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived
* `/proc/version` : specifies the version of the Linux kernel
* `/proc/self/environ`
* `/etc/passwd` : has all registered user that has access to a system
* `/etc/shadow` : contains information about the system's users' passwords
* `/root/.bash_history` : contains the history commands for _root_ user
* `/var/log/dmessage` : contains global system messages, including the messages that are logged during system startup
* `/var/log/auth.log` : contains all ssh logs, (**rce** and **log poisoning**)
* `/var/mail/root` : all emails for _root_ user
* `/home/<user>/.ssh/id_rsa` : Private SSH keys for a root or any known valid user on the server
* `/var/log/apache2/access.log` : the accessed requests for _Apache_ web server
* `/proc/cmdline`
* `/etc/hosts`
* `/etc/issue`
* `C:\boot.ini` : contains the boot options for computers with BIOS firmware

## Windows

```
php://input
C:\boot.ini
C:\WINDOWS\win.ini
C:\WINDOWS\php.ini
C:\WINDOWS\System32\Config\SAM
C:\WINNT\php.ini
C:\xampp\phpMyAdmin\config.inc
C:\xampp\phpMyAdmin\phpinfo.php
C:\xampp\phpmyadmin\config.inc
C:\xampp\phpmyadmin\phpinfo.php
C:\xampp\phpmyadmin\config.inc.php
C:\xampp\phpMyAdmin\config.inc.php
C:\xampp\apache\conf\httpd.conf
C:\xampp\FileZillaFTP\FileZilla Server.xml
C:\xampp\MercuryMail\mercury.ini
C:\mysql\bin\my.ini
C:\xampp\php\php.ini
C:\xampp\phpMyAdmin\config.inc.php
C:\xampp\tomcat\conf\tomcat-users.xml
C:\xampp\tomcat\conf\web.xml
C:\xampp\sendmail\sendmail.ini
C:\xampp\webalizer\webalizer.conf
C:\xampp\webdav\webdav.txt
C:\xampp\apache\logs\error.log
C:\xampp\apache\logs\access.log
C:\xampp\FileZillaFTP\Logs
C:\xampp\FileZillaFTP\Logs\error.log
C:\xampp\FileZillaFTP\Logs\access.log
C:\xampp\MercuryMail\LOGS\error.log
C:\xampp\MercuryMail\LOGS\access.log
C:\xampp\mysql\data\mysql.err
C:\xampp\sendmail\sendmail.log
C:\apache\log\error.log
C:\apache\log\access.log
C:\apache\log\error_log
C:\apache\log\access_log
C:\apache2\log\error.log
C:\apache2\log\access.log
C:\apache2\log\error_log
C:\apache2\log\access_log
C:\log\error.log
C:\log\access.log
C:\log\error_log
C:\log\access_log
C:\apache\logs\error.log
C:\apache\logs\access.log
C:\apache\logs\error_log
C:\apache\logs\access_log
C:\apache2\logs\error.log
C:\apache2\logs\access.log
C:\apache2\logs\error_log
C:\apache2\logs\access_log
C:\logs\error.log
C:\logs\access.log
C:\logs\error_log
C:\logs\access_log
C:\log\httpd\access_log
C:\log\httpd\error_log
C:\logs\httpd\access_log
C:\logs\httpd\error_log
C:\opt\xampp\logs\access_log
C:\opt\xampp\logs\error_log
C:\opt\xampp\logs\access.log
C:\opt\xampp\logs\error.log
C:\Program Files\Apache Group\Apache\logs\access.log
C:\Program Files\Apache Group\Apache\logs\error.log
C:\Program Files\Apache Group\Apache\conf\httpd.conf
C:\Program Files\Apache Group\Apache2\conf\httpd.conf
C:\Program Files\xampp\apache\conf\httpd.conf
```
