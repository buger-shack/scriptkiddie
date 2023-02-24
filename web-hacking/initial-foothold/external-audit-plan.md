# External Audit - Plan

{% embed url="https://owasp.org/www-project-web-security-testing-guide/stable" %}
OWASP Testing Guide - Web
{% endembed %}

## External Audit - Plan

```bash
# reco
nmap -sC -sV -p- -Pn $target -oN scan
rustscan $target -- -sC -sV -Pn
# if firewall blocks, check : Firewall > Evasion

nikto -h $target
./testssl.sh $target

# wappalyzer > analyze js libraries versions
# get HTTP responses to check for infos leak, bad implementation of security headers
curl --head $target
python3 NextGen-HeadersScanner/h_scan.py -u $target

# identify http methods
use auxiliary/scanner/http/options
set rhosts $target
set rport $port # if https use 443
# if https
set ssl true
exploit

# cookies
# check for httponly, secure & samesite parameters
# analyze value of cookie

# footprint
godspider -q -c 10 -s "$target"

# get directories
feroxbuster -u $target -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
feroxbuster -u $target -w /SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -C 401,404,403,502 -x zip tar gz tgz rar java txt pdf docx bak old
wfuzz -w  /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --hc 404,403 $target/FUZZ

# retrieve parameters
arjun -u $target

# refresh page > web dev tool > network > check all sites it communicates with
# read source code > look for comments

# check for : OWASP TOP 10
# GET/POST Parameters :
## - SQLI
## - LFI / RFI

# FORMS :
## - XSS / HTML Injection
## - CSRF

# Server side : SSTI, SSRF
# Clickjacking
# Files upload
# IDOR

```
