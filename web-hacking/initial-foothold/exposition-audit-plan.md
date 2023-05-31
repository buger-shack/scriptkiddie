# Exposition Audit - Plan

## Aim

> The objective is to define the attack surface of a company, mainly made up of all the elements of its information system exposed on the Internet.

```bash
# scan ips
nmap -sC -sV -iL $target_file 

# gowitness - screenshot web
gowitness # screens
# OR : https://github.com/michenriksen/aquatone

# visualize screens
gowitness server -P screens/ -D db.sqlite3 report

# enum subdomains
python3 theHarvester.py -d $domain -b hackertarget

# check for "forbidden", enumerate with gobuster/wfuzz/feroxbuster/ffuf
wfuzz -w raft-large-directories.txt --hc 404,403 https://$target/FUZZ

# faster
ffuf -u http://$target/FUZZ -w raft-large-directories.txt -fc 401,403,404 -fs 0

# minimal reconnaissance/crawling - finalrecon
python3 finalrecon.py --full https://$target

# gospider - web crawling
gospider -q -c 10 -s "https://$target"
gospider -q -c 10 -S urls.txt -o scan
```

## OSINT

### Get emails

{% embed url="https://hunter.io" %}

```bash
python linkedin2username.py myname@email.com uber-
```

### Leaked passwords

{% embed url="https://search.illicit.services/" %}
Db leaks
{% endembed %}

{% embed url="https://intelx.io/" %}

{% embed url="https://haveibeenpwned.com/" %}
