# 🔷 WordPress

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
