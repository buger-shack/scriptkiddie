# HTTP Methods

## Identify Methods Used

### Script

### Metasploit

```bash
use auxiliary/scanner/http/options
set rhosts $target
set rport $port # if https use 443
# if https
set ssl true
exploit
```

## List

|                GET               |                                HEAD                               |            POST           |                                          PUT                                          |            DELETE            |               CONNECT              |                           OPTIONS                           |                                 TRACE                                 |                    PATCH                    |
| :------------------------------: | :---------------------------------------------------------------: | :-----------------------: | :-----------------------------------------------------------------------------------: | :--------------------------: | :--------------------------------: | :---------------------------------------------------------: | :-------------------------------------------------------------------: | :-----------------------------------------: |
| Retrieves data using a given URI | Same as GET but only transfers the status line and header section | Sends data the the server | Replaces all current representations of the target resource with the uploaded content | Deletes a specified resource | Establishes a tunnel to the server | Describes the communication options for the target resource | Performs message-loop-back test along the path to the target resource | Applies partial modifications to a resource |

## TRACE method

The HTTP TRACE method is **designed for diagnostic purposes**. If enabled, the web server will respond to requests that use the TRACE method by echoing in its response the exact request that was received.

This behavior is often harmless, but occasionally leads to the **disclosure of sensitive information** such as internal authentication headers appended by reverse proxies. This functionality could historically be used to bypass the HttpOnly cookie flag on cookies, but this is no longer possible in modern web browsers.

### Remediation

{% hint style="danger" %}
**The TRACE method should be disabled on production web servers.**
{% endhint %}
