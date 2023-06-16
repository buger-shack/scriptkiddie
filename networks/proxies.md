# Proxies

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption><p>Source : UpGuard</p></figcaption></figure>

{% hint style="info" %}
Most commonly, people use “proxy” to refer to a service they connect to through settings in their web browser. When you connect to a proxy server, **all of your web traffic is routed through the proxy server** instead of going directly to the website you’re visiting. In other words, a proxy acts as a gateway between users and the internet.
{% endhint %}

## Common types of proxies

### SOCKS

#### SOCKS4

* SOCKS4 is an older version of the SOCKS protocol and provides **basic** **proxy** functionality.
* Supports **TCP** **connections** only.
* Do not support authentication, which means they don't require a username or password.
* Can be used for general-purpose proxying but lacks advanced features and security mechanisms.

#### SOCKS5

* **Updated** **version** of the **SOCKS** protocol with additional features and improvements.
* Supports both **TCP** and **UDP** connections, making it suitable for a wider range of applications.
* Offer enhanced security and authentication mechanisms.
* Support various authentication methods, such as username/password authentication and GSS-API (Generic Security Services Application Programming Interface) authentication.
* Provide features like UDP-association, IPv6 support, and hostname resolution.

### HTTP

* HTTP proxies are specifically designed for handling **HTTP** **traffic**.
* Commonly used in web browsers for web surfing and accessing web resources.
* Support caching, content filtering, and other HTTP-specific features.
* Operate at the **application** **layer** and are generally easier to configure and use for web-related activities.

## List of proxies to use

{% embed url="https://github.com/TheSpeedX/PROXY-List" %}
Updated everyday
{% endembed %}

## VPN VS Proxy ?

{% hint style="warning" %}
VPNs hide not only your private IP address but all your **web** **activity**, such as the websites you visit, using **encryption**.&#x20;

Proxy servers, on the other hand, will **only change your IP address**, but they won't encrypt your online activities.

Moreover, free proxy servers are **SLOW**.
{% endhint %}
