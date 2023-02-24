# ➕ Services Enumeration

## 53 - DNS

```bash
nmap -T4 -sS -p 53 $IP/24

# Enumerate ALL DNS records! Maybe hidden hosts in network recon

dig -t all target1 target2 target3 @$DNSSERVER

# DNS recon (brute force subdomains):
dnsrecon -d $IP -t brt -D /usr/share/wordlists/dnsmap.txt

dnsenum $DOMAIN

fierce -dns $DOMAIN -wordlist dictionary.txt

# DNS subdomains enum with ffuf
./ffuf -u http://$ip -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.test.lo"
```

{% embed url="https://github.com/rbsec/dnscan" %}
Wordlists
{% endembed %}

### Amplification

A Domain Name Server (DNS) amplification attack is a popular form of distributed denial of service (DDoS) that relies on the use of publically accessible open DNS servers to overwhelm a victim system with DNS response traffic.

```bash
auxiliary/scanner/dns/dns_amp
services -p 53 -u -R
```

## 135,137,138,139 - NetBIOS

```bash
nbtscan -r $IP/24
enum4linux -a $IP
nmblookup -A $IP
```

## 139,445 - SMB / SAMBA

```bash
nmap --script smb-os-discovery --open -p 139 $IP
nmap --script smb-os-discovery -p 139 --open $IP/24 -oX smb.xml
smbmap.py -H $IP
smbmap.py -H $IP -u Guest -R
smbmap.py -H $IP --upload $FILE $SHARE

# Recursive download:
smbget -a smb://$IP/$FILE -R

# Enumerate Users:
python /usr/share/doc/python-impacket-doc/examples/samrdump.py $IP

# Enumerate shares:
crackmapexec --shares $IP/24

# To list shares:
smbclient -L $IP
# or,
smbmap -H $IP

# To connect to a share (shell style):
smbclient //$IP/wwwroot
```

## RPC over DC

```bash
rpcclient -U "" -c enumdomusers $IP
rpcclient -U "" $IP -N -c "lsaquery"
rpcclient -U "" $IP -N -c "lookupnames Guest"
rpcclient -U "" $IP -N -c "lookupnames Administrator"
```

## 111 - RPC

```bash
# Look for port 111 rpcbind
rpcinfo $IP
rpcinfo -p $IP
```

## 3268 - DC Enumeration

```bash
nmap -sS -T4 -p 3268 --open $IP/24
```

### How to recognize a DC in a windows environment

* **DC Method 1**: Netbios If port **137** (TCP-UDP) open, a DC uses as a netbios suffixes:
  * For unique names: <1B> Domain Master Browser (PDC)
  * For group names: <1C> Domain Controllers for a domain
* **DC Method 2**: Global Catalog Service
  * Use nmap
  * As a Active Directory Server open ports 3268 and 3269 (SSL) for the Global Catalog Service (LDAP protocol).
  * Attention: LDAP protocol uses 389 and 636 (SSL).
* **DC Method #3** From the Windows machine:

```bash
echo %logonserver%
nltest /dclist:$DOMAIN
```

* **DC Method #4**

```bash
msf> use post/windows/gather/enum_domain
msf> set SESSION 1
msf> run
```

## 80,8080,443,8000 - HTTP

```bash
nmap --open -sV -p 80,8080,443,8000 $IP/24

# Virtual domains
nmap --open --script=hostmap -p 80 $IP

# TRACE method:
nmap --open --script=http-trace -p 80 $IP

# Enumerate userdir:
nmap --open --script=http-userdir-enum $IP

# Nikto scanner:
nikto -host http://$IP

# Dirb scanner:
dirb http://$IP

# For WordPress (wpscan):
docker pull wpscanteam/wpscan
docker run -it --rm wpscanteam/wpscan --stealthy --url https://$DOMAIN/
docker run -it --rm wpscanteam/wpscan --url https://$DOMAIN/ --enumerate u

# For Joomla:
joomscan http://$IP

# Gobuster (https://github.com/OJ/gobuster):
gobuster -u https://$DOMAIN -w /usr/share/dirb/wordlists/common.txt
gobuster -u https://$DOMAIN -c 'session=123456' -t 50 -w /usr/share/dirb/wordlists/common.txt -x .php,.html 
```

## WebDAV

```bash
davtest -cleanup -url http://$IP
cadaver http://$IP
    dav:/> put webshell.txt
    dav:/> copy webshell.txt ws.asp
```

## 161 - SNMP

```bash
nmap -p 161 --script snmp-enum $IP
snmp-check $IP

# Very useful:
snmp-check -v2c -c public $IP
python /usr/share/doc/python-impacket-doc/examples/samrdump.py SNMP $IP
onesixtone -w 0 $IP

# For scanning:
onesixtyone -c $COMMUNITY -i $IP_LIST_FILE

# For enumeration low level (MIB):
snmpwalk -c public -v1 $IP

# SNMP on different port:
snmpwalk -v 2c -c public $IP:666
snmp-check -p 6492 $IP
```

## 22 - SSH

```bash
TOOLS/enumSSH
nmap --script ssh-hostkey -p 22 --open -sS $IP/24
ssh-keyscan $IP
./TOOLS/ssh-vulnkey $IP TOOLS/ssh-blacklist/blacklist.all
```

## 21 - FTP

```bash
# metasploit
services -p 21 -c info -o /tmp/ftpinfo
cat /tmp/ftpinfo | cut -d , -f2 | sort | uniq

# or
use auxiliary/scanner/ftp/ftp_version
services -p 21 -R

# nmap
nmap -sV --script ftp-* -p 21 $IP
```

## 25 - SMTP

### Detect version

```bash
# metasploit
use auxiliary/scanner/smtp/smtp_version
services -p 25 -u -R
```

### Open Relays

Tests if an SMTP server will accept (via a code 250) an e-mail by using a variation of testing methods

```bash
# metasploit
use auxiliary/scanner/smtp/smtp_relay
services -p 25 -u -R
```

### User Enumeration Utility

Allows the enumeration of users: VRFY (confirming the names of valid users) and EXPN (which reveals the actual address of users aliases and lists of e-mail (mailing lists)). Through the implementation of these SMTP commands can reveal a list of valid users. User files contains only Unix usernames so it skips the Microsoft based Email SMTP Server. This can be changed using UNIXONLY option and custom user list can also be provided.

```bash
use auxiliary/scanner/smtp/smtp_enum
services -p 25 -u -R
```

## 69 - TFTP

```bash
nmap --open -sU -p 69 $IP/24
```

## 88 - KERBEROS

### Users enumeration

The script should work against Active Directory. It needs a valid Kerberos REALM in order to operate.

```bash
# metasploit
nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm='XX-XXXT'" 10.74.251.24
```

## NFS

```bash
showmount -e $IP
showmount -a $IP
mount.nfs $IP:$DIR $LOCALDIR
```

## 123 - NTP

```bash
# Show clients that have queried this server:
ntpdc -n -c monlist $IP
nmap -sU -p 123 --script=ntp-info $IP
```

## 389 - LDAP

### LDAP-rootdse

ldap-rootdse.nse : Retrieves the LDAP root DSA-specific Entry (DSE)

```bash
nmap -p 389 --script ldap-rootdse $ip
```

### LDAPsearch

```bash
nmap -p 389 --script ldap-search --script-args 'ldap.username="cn=ldaptest,cn=users,dc=cqure,dc=net",ldap.password=ldaptest,
ldap.qfilter=users,ldap.attrib=sAMAccountName' $ip

ldapsearch -H ldap://$IP/
ldapsearch -x -h $IP -s base
```

```bash
ldapwhoami
```

## 443 - SSL/TLS

```bash
sslscan $IP
./testssl.sh $IP
nmap -sV --script ssl-enum-ciphers -p 443 $IP
```

check :&#x20;

{% embed url="https://pentestwiki.org/attacks-on-ssl-tls-protocols/#tools" %}

## 1433 - MSSQL

### Metasploit modules

```bash
auxiliary/admin/mssql/mssql_enum                                           normal     Microsoft SQL Server Configuration Enumerator
auxiliary/admin/mssql/mssql_enum_domain_accounts                           normal     Microsoft SQL Server SUSER_SNAME Windows Domain Account Enumeration
auxiliary/admin/mssql/mssql_enum_domain_accounts_sqli                      normal     Microsoft SQL Server SQLi SUSER_SNAME Windows Domain Account Enumeration
auxiliary/admin/mssql/mssql_enum_sql_logins                                normal     Microsoft SQL Server SUSER_SNAME SQL Logins Enumeration
auxiliary/admin/mssql/mssql_escalate_dbowner                               normal     Microsoft SQL Server Escalate Db_Owner
auxiliary/admin/mssql/mssql_escalate_dbowner_sqli                          normal     Microsoft SQL Server SQLi Escalate Db_Owner
auxiliary/admin/mssql/mssql_escalate_execute_as                            normal     Microsoft SQL Server Escalate EXECUTE AS
auxiliary/admin/mssql/mssql_escalate_execute_as_sqli                       normal     Microsoft SQL Server SQLi Escalate Execute AS
auxiliary/admin/mssql/mssql_exec                                           normal     Microsoft SQL Server xp_cmdshell Command Execution
auxiliary/admin/mssql/mssql_findandsampledata                              normal     Microsoft SQL Server Find and Sample Data
auxiliary/admin/mssql/mssql_idf                                            normal     Microsoft SQL Server Interesting Data Finder
auxiliary/admin/mssql/mssql_ntlm_stealer                                   normal     Microsoft SQL Server NTLM Stealer
auxiliary/admin/mssql/mssql_ntlm_stealer_sqli                              normal     Microsoft SQL Server SQLi NTLM Stealer
auxiliary/admin/mssql/mssql_sql                                            normal     Microsoft SQL Server Generic Query
auxiliary/admin/mssql/mssql_sql_file                                       normal     Microsoft SQL Server Generic Query from File
auxiliary/analyze/jtr_mssql_fast                                           normal     John the Ripper MS SQL Password Cracker (Fast Mode)
auxiliary/gather/lansweeper_collector                                      normal     Lansweeper Credential Collector
auxiliary/scanner/mssql/mssql_hashdump                                     normal     MSSQL Password Hashdump
auxiliary/scanner/mssql/mssql_login                                        normal     MSSQL Login Utility
auxiliary/scanner/mssql/mssql_ping                                         normal     MSSQL Ping Utility
auxiliary/scanner/mssql/mssql_schemadump
```

### Info gathering

**MSSQL Ping Utility**

> Queries the MSSQL instance for information. This will also provide if any ms-sql is running on different ports.

```bash
use auxiliary/scanner/mssql/mssql_ping
services -p 1433 -R
```

## 1521 - ORACLE

### N.B.

We need 4 things to connect to an Oracle DB.

* IP.
* Port.
* Service Identifier (SID).
* Username/ Password.

### Detect version

```bash
# metasploit
use auxiliary/scanner/oracle/tnslsnr_version
services -p 1521 -u -R
```

### Get SID

Oracle Service Identifier: By querying the TNS Listener directly, brute force for default SID’s or query other components that may contain it.

```bash
# Oracle TNS Listener SID Enumeration: This module simply queries the TNS listner for the Oracle SID. With Oracle 9.2.0.8 and above the listener will be protected and the SID will have to be bruteforced or guessed.
use auxiliary/scanner/oracle/sid_enum

# Oracle TNS Listener SID Bruteforce: This module queries the TNS listner for a valid Oracle database instance name (also known as a SID). Any response other than a “reject” will be considered a success. If a specific SID is provided, that SID will be attempted. Otherwise, SIDs read from the named file will be attempted in sequence instead.
use auxiliary/scanner/oracle/sid_brute
```

### Bruteforce

```bash
use auxiliary/scanner/oracle/oracle_login
```

## 5432 - POSTGRESQL

### Detect version

```bash
# metasploit
use auxiliary/scanner/postgres/postgres_version
```

### Login utility

```bash
# metasploit
use auxiliary/scanner/postgres/postgres_login
```

### Flag injection

```bash
# metasploit
# Identify PostgreSQL 9.0, 9.1, and 9.2 servers that are vulnerable to command-line flag injection through CVE-2013-1899. This can lead to denial of service, privilege escalation, or even arbitrary code execution
use auxiliary/scanner/postgres/postgres_dbname_flag_injection
```

## 6379 - Redis-server

```bash
(printf "info\r\n"; sleep 1) | netcat $IP 6379
```

## SSDP server

```bash
tcpdump -n -A host $IP & perl -e 'print "M-SEARCH * HTTP/1.1\r\nHost:239.255.255.250:1900\r\nST:upnp:rootdevice\r\nMan:\"ssdp:discover\"\r\nMX:3\r\n\r\n"' > /dev/udp/$IP/1900
```

## 11211 - memcached

```bash
echo "stats" | netcat $IP 11211
echo -en "\x00\x00\x00\x00\x00\x01\x00\x00stats\r\n" | netcat -u $IP 11211
```

## 9200 - elasticsearch

```bash
echo -ne "GET / HTTP/1.0\r\n\r\n" | netcat $IP 9200
```

## 5353 - avahi-daemon / mDNS

```bash
dig +short -p 5353 -t ptr _services._dns-sd._udp.local @$IP
```

## 27017,27018,27019,27020 - MongoDB

```bash
mongo --host $IP
```
