# HTTP Methods

## Identify Methods Used

### httpmethods

{% embed url="https://github.com/ShutdownRepo/httpmethods" %}

```bash
# install
git clone https://github.com/ShutdownRepo/httpmethods
cd httpmethods
python3 setup.py install

# usage
httpmethods -u http://www.example.com/
```

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

<table data-full-width="false"><thead><tr><th align="center">GET</th><th align="center">HEAD</th><th align="center">POST</th><th align="center">PUT</th><th align="center">DELETE</th><th align="center">CONNECT</th><th align="center">OPTIONS</th><th align="center">TRACE</th><th align="center">PATCH</th></tr></thead><tbody><tr><td align="center">Retrieves data using a given URI</td><td align="center">Same as GET but only transfers the status line and header section</td><td align="center">Sends data the the server</td><td align="center">Replaces all current representations of the target resource with the uploaded content</td><td align="center">Deletes a specified resource</td><td align="center">Establishes a tunnel to the server</td><td align="center">Describes the communication options for the target resource</td><td align="center">Performs message-loop-back test along the path to the target resource</td><td align="center">Applies partial modifications to a resource</td></tr></tbody></table>

## TRACE method

The HTTP TRACE method is **designed for diagnostic purposes**. If enabled, the web server will respond to requests that use the TRACE method by echoing in its response the exact request that was received.

This behavior is often harmless, but occasionally leads to the **disclosure of sensitive information** such as internal authentication headers appended by reverse proxies. This functionality could historically be used to bypass the HttpOnly cookie flag on cookies, but this is no longer possible in modern web browsers.

### Remediation

{% hint style="danger" %}
**The TRACE method should be disabled on production web servers.**
{% endhint %}
