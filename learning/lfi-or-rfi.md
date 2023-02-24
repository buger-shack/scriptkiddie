# LFI | RFI

{% embed url="https://secf00tprint.github.io/blog/payload-tester/lfirfi/en" %}
Source
{% endembed %}

## What is LFI / RFI ?

The individual steps in detail:

* First a client sends a request to the server, e.g. someone types in the URL in the browser and presses Enter.
* The server then sends back a response.
* The request URL consists of several parts, the protocol :// the domain of the server / and query parameters.
* The response can be some html code.
* The query parameters of the URL are structured by key-value pairs.
* You could try to change a value of a key …
* … and see what happens with the response. Does the content change?

{% hint style="info" %}
* **Local File Inclusion** is it if you could change that file to another file that then will be loaded not intended by the application.
* **Remote File Inclusion** is it if you could change the value to an url which then would be loaded as file into the server.
{% endhint %}

### Why is it so dangerous ?

![](<../.gitbook/assets/image (38).png>)

* 1st level: Read access LFI
* 2nd level: Write access LFI
* 3rd level: RFI

```
# Read access LFI
If LFI is possible, the attacker can read files from the server. This affects files within the current directory or even across directories.

# Write access LFI
Does the attacker have :
- the ability to upload files or manipulate files on the server and
- do these files are located in an accessible LFI directory
 : then attacker can execute arbitrary code on the server.
```

## RFI

If RFI is possible it’s easiest to attack. The attacker has just to include the malicious code into the url and the payload will be executed onto the victim machine.

#### Path Traversal

Also known as _Directory traversal_, a web security vulnerability allows an attacker to **read operating system resources**, such as local files on the server running an application. The attacker exploits this vulnerability by manipulating and abusing the web application's URL to locate and access files or directories stored outside the application's root directory.

**When are they occuring ?**

Path traversal vulnerabilities occur when the user's input is passed to a function such as **file\_get\_contents** in PHP. It's important to note that the function is not the main contributor to the vulnerability. Often poor input validation or filtering is the cause of the vulnerability.

The following graph shows how a web application stores files in **/var/www/app**. The happy path would be the user requesting the contents of _userCV.pdf_ from a defined path _/var/www/app/CVs_.
