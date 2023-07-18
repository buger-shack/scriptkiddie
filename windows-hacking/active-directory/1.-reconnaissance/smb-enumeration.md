# SMB Enumeration

SMB stand for Server Message Block and it allows you to share your resources to other computers over the network. There are 3 versions of SMB :

* SMBv1 version vulnerable to known exploits (Eternal Blue, WannaCry), now disabled by default in latest Windows version.
* SMBv2 reduced "chattiness" of SMB1. Guest access is disabled by default.
* SMBv3 guest access disabled, uses encryption. Most secure.

TCP port **139** is SMB over NetBios TCP port **445** is SMB over IP (latest version of SMB).

List of SMB versions and corresponding Windows versions :

* SMB1 – Windows 2000, XP and Windows 2003.
* SMB2 – Windows Vista SP1 and Windows 2008
* SMB2.1 – Windows 7 and Windows 2008 R2
* SMB3 – Windows 8 and Windows 2012.

## Synthax

```bash
# connecting to share
smbclient \\\\$ip\\C$
```

## Enumeration

```bash
# enum4linux
# default
enum4linux $ip

# runs all options
enum4linux -a $ip

# If you've obtained credentials, you can pull a full list of users regardless of the RestrictAnonymous option
enum4linux -u '$user' -p '$pass' -a $ip


# crackmapexec
# display shares and their permissions
crackmapexec smb $ip -u '$user' -p '$pass' --shares


# nmap
# enumerate smb shares 
nmap --script smb-enum-shares -p 139,445 $ip

nmap --script smb-os-discovery $ip

# perfect bruteforce password auditing against smb servers
nmap --script smb-brute $ip

# collects system info through smb/netbios
nmap --script smb-system-info $ip

# check smb vulnerabilities
nmap --script smb-vuln* $ip
```

## smbmap

https://github.com/ShawnDEvans/smbmap

SMBMap allows users to enumerate samba share drives across an entire domain. List share drives, drive permissions, share contents, upload/download functionality, file name auto-download pattern matching, and even execute remote commands.



```bash
# Default Output
smbmap.py -H 0.0.0.0 -u administrator -p asdf1234

# Default Output, with NTML hash
smbmap.py -u jsmith -p 'aad3b435b51404eeaad3b435b51404ee:da76f2c4c96028b7a6111aef4a50a94d' -H 0.0.0.0

# Command execution
smbmap.py -u ariley -p 'P@$$w0rd1234!' -d ABC -x 'net group "Domain Admins" /domain' -H 0.0.0.0
```

## `rpcclient` | port 445

Authenticate 'Userless' SMB Session with rpcclient

```bash
rpcclient -U% $ip
rpcclient -U '' $ip
```

**Sub commands**

```bash
enumdomusers
enumdomains
enumprivs
netshareenum
netsessenum
getdompwinfo
lookupnames administrator
```

## `rpcdump` | MSRPC - port 135

_RPC - Remote procedure call_

## Microsoft RPC

```bash
rpcdump.py -port 135 $ip
```

## NFS Shares

```bash
# is there any nfs shares ?
showmount -e $ip
# mount it
mount -t nfs -o rw,vers=2 $ip:$remote_path $local_path -o nolock
```
