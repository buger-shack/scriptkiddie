# External Audit - Plan

{% embed url="https://owasp.org/www-project-web-security-testing-guide/stable" %}
OWASP Testing Guide - Web
{% endembed %}

## External Audit - Plan

```bash
'''
Black Box
'''
# Network
nmap -A -p- -Pn -f $target -oN scan
nmap -sC -sV -p- $target -oN scan
# or
rustscan -a $target -- -sC -sV -oN scan

# Google Dorking (infos leak)
site:$target filetype:txt
site:$target filetype:pdf

# Accounts Leaks : intelx.io 

# Reconnaissance
gospider -q -c 10 -s "http://$target"
# wappalyzer / identify versions
# read source code of webpages / finds keys / hidden endpoints

# WEB APP
nikto -h $target

# Fuzzing
feroxbuster -t 10 -u https://0.0.0.0 -k -w /usr/share/seclists/Discovery/Web-Content/common.txt -o feroxbuster
# or
wfuzz -w /usr/share/SecLists/Discovery/Web-Content/raft-large-directories-lowercase.txt  -u https://0.0.0.0/FUZZ --hw $hw -p $proxy

# TLS / HSTS
./testssl.sh $target
# burp suite > server response > hsts ? 
# info leak in headers ?

# HTTP parameters
arjun -u $url

'''
Grey Box
'''
# COOKIES
## Secure, HTTPOnly flags
## session fixation

# IDOR  / Improper Isolation or Compartmentalization :
#	access URI functionalities with no auth / no privileges

# Uploads : eicar / file uploads bypass / lfi ?
# zipslip / CSV injection

# Inputs : SQLi / XSS / CSRF / SSRF / SSTI / OS Injection

# FUNCTIONALITIES 
#	password change, weak pass ?
```
