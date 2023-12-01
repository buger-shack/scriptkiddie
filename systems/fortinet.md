# 🛡 Fortinet

## Ports

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### Scan for ports

```bash
nmap -sV -sC -iL targets.txt -v -p514,541,666,701,1003,8008,8010,8888,8890,9443,10443,13000,13001,13002,13003,13004,13005,13006,13007,13030,13031,13032,13033,13034,13035,13036,13037,13038,13039 -oN fortigate
```

## 443 - Fortinet

> Default credentials :&#x20;
>
> admin:

## 541 - Fortinet SSL/VPN

```bash
# INTERACT
openssl s_client -connect 50.247.206.121:541

# or nc

nc 50.247.206.121 541
```

## 10443/8443 - Fortigate SSL/VPN

### Check for CVE

{% embed url="https://github.com/BishopFox/CVE-2023-27997-check" %}

<pre class="language-bash"><code class="lang-bash">git clone https://github.com/BishopFox/CVE-2023-27997-check
<strong>cd CVE-2023-27997-check
</strong>python3 -m venv venv
source venv/bin/activate
python3 -m pip install -r requirements.txt

# usage 
python3 CVE-2023-27997-check.py &#x3C;IP> &#x3C;PORT>
</code></pre>
