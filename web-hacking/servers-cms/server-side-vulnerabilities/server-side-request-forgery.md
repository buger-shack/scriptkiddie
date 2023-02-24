# Server-Side Request Forgery

{% hint style="info" %}
Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make requests to an unintended location.

In a typical SSRF attack, the **attacker might cause the server to make a connection to internal-only services** within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems, potentially leaking sensitive data such as authorization credentials.
{% endhint %}

## Exploitation

### Against the server itself

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1

# change to :

POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
```

Here, the server will fetch the contents of the /admin URL and return it to the user.

### Against other back-end systems

Another type of trust relationship that often arises with server-side request forgery is where the application server is able to interact with other back-end systems that are not directly reachable by users.

In the preceding example, suppose there is an **administrative interface** at the back-end URL https://192.168.0.68/admin. Here, an attacker can exploit the SSRF vulnerability to access the administrative interface by submitting the following request:

```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://192.168.0.68/admin
```

## Bypass Defenses

### Blacklist-based input filters

Some applications block input containing hostnames like 127.0.0.1 and localhost, or sensitive URLs like /admin. In this situation, you can often circumvent the filter using various techniques:

* Using an alternative IP representation of 127.0.0.1, such as **2130706433**, **017700000001**, or **127.1**.
* Registering your own domain name that resolves to 127.0.0.1. You can use **spoofed.burpcollaborator.net** for this purpose.
* Obfuscating blocked strings using **URL encoding** or case variation.

### Whitelist-based input filters

Some applications only allow input that matches, begins with, or contains, a whitelist of permitted values. In this situation, you can sometimes circumvent the filter by exploiting inconsistencies in URL parsing.

The URL specification contains a number of features that are liable to be overlooked when implementing ad hoc parsing and validation of URLs:

* You can **embed credentials** in a URL before the hostname, using the @ character. For example:
  * https://expected-host@evil-host
* You can use the **# character** to indicate a URL fragment. For example:
  * https://evil-host#expected-host
* You can leverage the **DNS naming** hierarchy to place required input into a fully-qualified DNS name that you control. For example:
  * https://expected-host.evil-host
* You can **URL-encode** characters to confuse the URL-parsing code. This is particularly useful if the code that implements the filter handles URL-encoded characters differently than the code that performs the back-end HTTP request.
* You can use **combinations** of these techniques together.
