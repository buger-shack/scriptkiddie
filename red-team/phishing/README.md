# 🎣 Phishing

## Find domain name

{% embed url="https://github.com/elceef/dnstwist" %}

```bash
git clone https://github.com/elceef/dnstwist.git
cd dnstwist
pip install .

# usage, find domains
dnstwist $domain
```

## Find person to impersonate

{% hint style="info" %}
OSINT :&#x20;

* hunter.io
* LinkedIn
{% endhint %}

{% embed url="https://pentestmonkey.net/tools/user-enumeration/smtp-user-enum" %}

```bash
smtp-user-enum -U /usr/share/seclists/Usernames/top-usernames-shortlist.txt "10.129.87.12" "25" -m RCPT
smtp-user-enum -U ./test-users.txt "10.129.87.12" "25" -m RCPT
```
TEST
