# Web Scanners

## nikto

{% hint style="info" %}
<mark style="color:blue;">**Website vulnerability Scanner**</mark>
{% endhint %}

{% embed url="https://github.com/sullo/nikto" %}

### :mouse\_three\_button: Commands

```bash
# Default
nikto -h http://0.0.0.0

# scan domain with ssl enabled
nikto -h https://0.0.0.0 -ssl
```

## Whatweb

{% hint style="info" %}
<mark style="color:blue;">**WhatWeb**</mark> <mark style="color:blue;">identifies websites</mark><mark style="color:blue;">**.**</mark> <mark style="color:blue;">It</mark> <mark style="color:blue;">**recognizes web technologies**</mark> <mark style="color:blue;">including content management systems (</mark><mark style="color:blue;">**CMS**</mark><mark style="color:blue;">),</mark> <mark style="color:blue;">**blogging platforms**</mark><mark style="color:blue;">,</mark> <mark style="color:blue;">**statistic/analytics packages**</mark><mark style="color:blue;">,</mark> <mark style="color:blue;">**JavaScript libraries**</mark><mark style="color:blue;">,</mark> <mark style="color:blue;">**web servers**</mark><mark style="color:blue;">, and</mark> <mark style="color:blue;">**embedded devices**</mark><mark style="color:blue;">.</mark>
{% endhint %}

{% embed url="https://github.com/urbanadventurer/WhatWeb" %}

### :gear: Installation

```bash
git clone https://github.com/urbanadventurer/WhatWeb.git
cd WhatWeb
make install
```

### :mouse\_three\_button: Commands

```bash
# Default scan
whatweb $ip

# Scan the local network quickly and suppress errors
whatweb --no-errors $network

whatweb --aggression=Stealthy/Aggressive/Heavy --verbose 

# Scan reddit.com slashdot.org with verbose plugin descriptions
whatweb -v reddit.com slashdot.org

# An aggressive scan of wired.com detects the exact version of WordPress.
whatweb -a 3 www.wired.com

# Scan the local network for https websites
whatweb --no-errors --url-prefix https:// $network
```

## BurpSuite Pro

{% embed url="https://github.com/rng70/Hacking-Resources/blob/master/Burp%20Suite/Burp%20Suite%20Cookbook.pdf" %}
BurpSuite Cookbook
{% endembed %}

## OWASP ZAP

{% embed url="https://owasp.org/www-project-zap" %}
