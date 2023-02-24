# 🗃 Directory/Files Scanners

## gobuster

{% hint style="info" %}
<mark style="color:blue;">A tool used to</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**brute-force**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">URIs (directories and files), DNS subdomains and virtual host names.</mark>
{% endhint %}

{% embed url="https://github.com/OJ/gobuster" %}

### :blue\_circle: Modules

GoBuster has a couple of modules and each module has its own flags :

```bash
dir # uses directory/file enumeration mode
dns # uses dns subdomain enumeration mode
fuzz # uses fuzzing mode
help # help about any command
s3 # uses aws bucket enumeration mode
version # shows the current version
vhost # uses vhost enumeration mode
```

### :flag\_white: Flags

```bash
--delay <duration> # Time each thread waits between requests (e.g. 1500ms)
-h # help for gobuster
--no-error # Don't display errors
-z # Don't display progress
-o <string> # Output file to write results
-p <string> # File containing replacement patterns
-q # Don't print the banner and other noise
-t <int> # Number of concurrent threads (default 10)
-v # Verbose output (errors)
-w <string> # Path to the wordlist
```

#### Examples

```bash
# discover txt,html,js,json,php,py files
gobuster dir -u http://0.0.0.0 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,html,js,json,php,py

# exclude 403,404 codes
gobuster dir -u http://0.0.0.0 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -b 403 404

# discover 0.0.0.0 subdomains 
gobuster dns -d http://0.0.0.0 -w /usr/share/SecLists/Discovery/DNS/namelist.txt

# discover txt,html,js,json,php,py files using a proxy connection
gobuster dir -u http://0.0.0.0 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,html,js,json,php,py --proxy http://127.0.0.1:8081
```



## Feroxbuster

{% hint style="info" %}
<mark style="color:blue;">A simple, fast,</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**recursive**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">content discovery tool written in Rust.</mark>
{% endhint %}

{% embed url="https://github.com/xmendez/wfuzz" %}

### HTTP

```bash
#seclist
feroxbuster -t 10 -u http://0.0.0.0 -w /usr/share/seclists/Discovery/Web-Content/common.txt -o feroxbuster

#dirbuster
feroxbuster -t 10 -u http://0.0.0.0 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o feroxbuster

feroxbuster -t 10 -u http://0.0.0.0 -w /usr/share/seclists/Discovery/Web-Content/common.txt -x py,html,txt -o feroxbuster
```

### HTTPS

```bash
feroxbuster -t 10 -u https://0.0.0.0 -k -w /usr/share/seclists/Discovery/Web-Content/common.txt -o feroxbuster

feroxbuster -t 10 -u https://0.0.0.0 -k -w /usr/share/seclists/Discovery/Web-Content/common.txt -x py,html,txt -o feroxbuster
```

## Wfuzz

{% hint style="info" %}
<mark style="color:blue;">**The web bruteforcer**</mark>
{% endhint %}

{% embed url="https://github.com/xmendez/wfuzz" %}

### :mouse\_three\_button: Commands

```bash
# search for directories & put 404 responses away
wfuzz -w  /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  --hc 404 http://0.0.0.0/FUZZ

# search for php files
wfuzz -w wordlist/general/common.txt http://0.0.0.0/FUZZ.php

# use 2 wordlists for user & pass & put 302 responses away
wfuzz -z file,/usr/share/wordlists/rockyou.txt -d "uname=FUZZ&pass=FUZZ"  --hc 302 http://0.0.0.0/userinfo.php	
```
