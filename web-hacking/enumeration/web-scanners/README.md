# Web Scanners

## nikto

{% embed url="https://github.com/sullo/nikto" %}

### Usage

```bash
# Default
nikto -h http://0.0.0.0

# scan domain with ssl enabled
nikto -h https://0.0.0.0 -ssl
```

## Whatweb

{% embed url="https://github.com/urbanadventurer/WhatWeb" %}
hatWeb identifies websites. It recognizes web technologies including content management systems (CMS), blogging platforms, statistic/analytics packages, JavaScript libraries, web servers, and embedded devices.
{% endembed %}

```bash
# install
git clone https://github.com/urbanadventurer/WhatWeb.git
cd WhatWeb
make install

# usage
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

## PortSwigger's Burp Suite

{% embed url="https://github.com/rng70/Hacking-Resources/blob/master/Burp%20Suite/Burp%20Suite%20Cookbook.pdf" %}
BurpSuite Cookbook
{% endembed %}

## OWASP ZAP

{% embed url="https://owasp.org/www-project-zap" %}
Meh...
{% endembed %}
