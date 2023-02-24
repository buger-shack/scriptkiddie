# 🖥 Network Scanners

## nmap

{% hint style="info" %}
<mark style="color:blue;">Nmap is a</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**network scanner**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">created by</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**Gordon Lyon**</mark><mark style="color:blue;">. Nmap is used to</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**discover hosts**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">and</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**services**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">on a</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**computer network**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">by sending packets and analyzing the responses. Nmap provides a number of features for probing computer networks, including host discovery and service and operating system detection.</mark>
{% endhint %}

{% embed url="https://github.com/nmap/nmap" %}

### :mouse\_three\_button: Basic commands

```bash
# Default nmap script scan
sudo nmap -sV -sC -p- 0.0.0.0
```

#### Banner Grabbing

```bash
nmap -sV --script=banner -p21 0.0.0.0/24.
nc -nv 0.0.0.0
netcat 0.0.0.0 port
```

#### Protocols

```bash
# TCP
nmap –Pn –sT -sC –sV –p0-65535 0.0.0.0

# FTP
nmap -sC -sV -p21 0.0.0.0

# SMB
nmap --script smb-os-discovery.nse -p445 0.0.0.0
```

### :flag\_white: Flags

```bash
-sV # attempts to determine the version of the services running
-p <x> # port scan for port x or scan all ports
-p- # scan all ports
-Pn # Disable host discovery and just scan for open ports
-A # Enables OS and version detection, executes in-build scripts for further enumeration 
-sC # Scan with the default nmap scripts
-oN # Output scan in normal
-oA # Output in the three major formats at once
-v # Verbose mode
-sU # UDP port scan
-sS # TCP SYN port scan
-T1-4 # Set timing template (higher is faster)

-iL <file> # scan targets list (separated by comas, space, tab or new line)
--open <IP/HostRange> # find open ports on a specified IP range (172.16.0.*)
--exclude <IP/Host> # exclude IP addresses or hosts from scans
-e <interface> # specify a network interface (e.g eth0, wlan0, enp2s0,etc), useful if we are connected both through our wired and wireless cards

```

#### :fire: Firewall Evasion

```bash
-Pn # disables the ping command and only scans ports
-f # used to fragment the packets (i.e. split them into smaller pieces) making it less likely that the packets will be detected by a firewall or IDS.
# ALTERNATIVES TO -f, but providing more control over the size of the packets: 
--mtu <number> # accepts a maximum transmission unit size to use for the packets sent. This must be a multiple of 8.
--scan-delay <time> # in ms, used to add a delay between packets sent. This is very useful if the network is unstable, but also for evading any time-based firewall/IDS triggers which may be in place.
--badsum # this is used to generate in invalid checksum for packets. Any real TCP/IP stack would drop this packet, however, firewalls may potentially respond automatically, without bothering to check the checksum of the packet. As such, this switch can be used to determine the presence of a firewall/IDS.

# Scan from spoofed IP
nmap 192.168.1.1 -D 192.168.1.2

# Scan Facebook from Microsoft
nmap -S www.microsoft.com www.facebook.com

# Use a specific source port
nmap 192.168.1.1 -g 53
```

#### :receipt: Scripts

```bash
-sC # Scan with default NSE scripts. Considered useful for discovery and safe
--script default # Scan with default NSE scripts. Considered useful for discovery and safe
--script=banner # Scan with a single script. Example banner
--script=http* # Scan with a wildcard. Example http
--script=http,banner # Scan with two scripts. Example http and banner
--script "not intrusive" # Scan default, but remove intrusive scripts
--script-args snmpcommunity=admin # NSE script with arguments
```

### Examples

```bash
# http site map generator
nmap -Pn --script=http-sitemap-generator scanme.nmap.org 

# Fast search for random web servers
nmap -n -Pn -p 80 --open -sV -vvv --script banner,http-title -iR 1000 

# Brute forces DNS hostnames guessing subdomains
nmap -Pn --script=dns-brute domain.com

# Safe SMB scripts to run
nmap -n -Pn -vv -O -sV --script smb-enum*,smb-ls,smb-mbenum,smb-os-discovery,smb-s*,smb-vuln*,smbv2* -vv 192.168.1.1 

# Whois query
nmap --script whois* domain.com 

# Detect cross site scripting vulnerabilities.
nmap -p80 --script http-unsafe-output-escaping scanme.nmap.org 

# Check for SQL injections
nmap -p80 --script http-sql-injection scanme.nmap.org
```

## rustscan&#x20;

{% hint style="info" %}
<mark style="color:blue;">**Faster**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">than nmap</mark>
{% endhint %}

{% embed url="https://github.com/RustScan/RustScan" %}

### :mouse\_three\_button: Basic commands

```bash
# Use in most cases : Noisy AF
rustscan -a 0.0.0.0 -- -A -sC -sV -oN initial.log

# SYN "Stealth" scan
sudo rustscan -a 0.0.0.0 -- -vv -oN Initial-SYN-Scan

# Service Scan
sudo rustscan -a 0.0.0.0 -p 22,53,80,443 -- -sV -Pn -vv

# Multiple IP Scanning
rustscan -a 0.0.0.0,1.1.1.1

# CIDR support
rustscan -a 192.168.0.0/30

# Selected port scanning
rustscan -a 0.0.0.0 -p 53,80,121,65535

# Ranges of ports
rustscan -a 0.0.0.0 --range 1-1000

# UDP scan
rustscan -a 0.0.0.0 -sU -p ports
```
