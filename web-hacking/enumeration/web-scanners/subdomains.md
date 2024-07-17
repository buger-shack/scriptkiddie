# Subdomains

## Enumeration

{% tabs %}
{% tab title="Dorks" %}
### Google

```
site:*.wikipedia.org -www -store -jobs -uk

site:*.*.example.com
site:*.*.*.example.com 
```
{% endtab %}

{% tab title="Tools" %}
```bash
# wfuzz
wfuzz -c -f sub-fighter -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u "http://$target.com" -H "Host: FUZZ.$target.com"

# ffuf
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.target.com" -u $target.com

# amass
amass enum -d $domain

# sublistr |
sublist3r -d $domain -v

# dnsrecon
dnsrecon -t brt -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -v -d $domain

# dnsenum
dnsenum $target.com

```

#### Online Tools

{% embed url="https://dnsdumpster.com/" %}
DNS and host enumeration
{% endembed %}

{% embed url="https://www.virustotal.com/gui/home/search" %}
VirusTotal Search
{% endembed %}
{% endtab %}
{% endtabs %}

## Takeover

{% embed url="https://www.hackerone.com/blog/Guide-Subdomain-Takeovers" %}

### Subjack

{% embed url="https://github.com/haccer/subjack" %}

```bash
./subjack -w subdomains.txt -t 100 -timeout 30 -o results.txt -ssl
```
