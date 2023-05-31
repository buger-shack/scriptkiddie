# Cookies

<figure><img src="https://media0.giphy.com/media/xT0xeMA62E1XIlup68/giphy.gif?cid=ecf05e47hzwww0fov1vtcj7c08nu9al2s4h4uil0vdhpf9si&#x26;ep=v1_gifs_search&#x26;rid=giphy.gif&#x26;ct=g" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Cookies are small bits of data that are stored in your browser. **Each browser will store them separately, so cookies in Chrome won't be available in Firefox.** They have a huge number of uses, but the most common are either session management or advertising (tracking cookies). Cookies are normally sent with every HTTP request made to a server.

You can check your cookies security attributes with **ZAP** or **Nikto**.
{% endhint %}

## Brief

![Cookies can be used for many purposes but are most commonly used for website authentication. The cookie value won't usually be a clear-text string where you can see the password, but a token (unique secret code that isn't easily humanly guessable).](https://static-labs.tryhackme.cloud/sites/howhttpworks/cookie\_flow.png)

### Define Cookies attributes

{% embed url="https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes" %}
Testing for cookies attributes - OWASP
{% endembed %}

#### Restrict access to cookies

* **Secure**
  * Only sent to the server with an encrypted request over HTTPS, never sent with HTTP.
* **HTTPOnly**
  * Inacessible to Javascript document.cookie API; only sent to the server, helps mitigate XSS attacks.
* **Path**
  * Limits the scope of a cookie to a specific path on the server and can therefore be used to prevent unauthorized access to it from other applications on the same host.

#### Define where cookies are sent

* **SameSite**
  * The SameSite attribute lets servers specify whether/when cookies are sent with cross-site requests (where Site is defined by the registrable domain). This provides some protection against cross-site request forgery attacks (CSRF).
  * It takes three possible values: **Strict**, **Lax**, and **None**.

### Cookies prefixes

{% embed url="https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#define_where_cookies_are_sent" %}
Define where cookies are sent
{% endembed %}
