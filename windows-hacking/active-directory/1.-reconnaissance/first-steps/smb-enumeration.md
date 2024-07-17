# SMB Enumeration

{% hint style="info" %}
**SMB** stand for **S**erver **M**essage **B**lock, and it allows you to share your resources to other computers over the network. There are 3 versions of SMB :

* **SMBv1** version vulnerable to known exploits (Eternal Blue, Wanna Cry), now disabled by default in latest Windows version.
* **SMBv2** reduced “chattiness” of SMB1. Guest access is disabled by default.
* **SMBv3** guest access disabled, uses encryption. **Most secure**.

TCP port **139** is SMB over NetBIOS, TCP port **445** is SMB over IP (latest version of SMB).

List of SMB versions and corresponding Windows versions :

* **SMB1** – Windows 2000, XP, and Windows 2003.
* **SMB2** – Windows Vista SP1 and Windows 2008
* **SMB2.1** – Windows 7 and Windows 2008 R2
* **SMB3** – Windows 8 and Windows 2012.
{% endhint %}

## Connect to share

```bash
smbclient \\\\$ip\\$sharename
```

## Enumeration

```bash
# enum4linux
# default
enum4linux $ip
# runs all options
enum4linux -a $ip
# If you've obtained credentials => pull a full list of users regardless of the RestrictAnonymous option
enum4linux -u '$user' -p '$pass' -a $ip


# nmap
# enumerate smb shares, brute, get infos
nmap --script 'smb-enum-shares,smb-os-discovery,smb-brute,smb-system-info,smb-vuln*' -p 139,445 $ip


# netexec
# only in upgraded Windows servers from 2003 - No auth attempt
# Enumerate user shares anonymously
nxc smb $domainOrIP -u '' -p '' --shares
# to get password policy, minimum length, account lockout threshold
nxc smb $domainOrIP --pass-pol -u '' -p ''	
# The usernames with RID greater than 1000 into a username file
nxc smb $domainOrIP -u robot -p '' --rid-brute | grep SidTypeUser	


# smbmap
python3 smbmap.py --host-file smb-hosts.txt -d $domain -L
```

## smbmap

{% hint style="info" %}
SMBMap allows users to enumerate samba share drives across an entire domain. List share drives, drive permissions, share contents, upload/download functionality, file name auto-download pattern matching, and even execute remote commands.
{% endhint %}

{% embed url="https://github.com/ShawnDEvans/smbmap" %}

```bash
# Default Output
smbmap.py -H 0.0.0.0 -u administrator -p asdf1234

# Default Output, with NTML hash
smbmap.py -u jsmith -p 'aad3b435b51404eeaad3b435b51404ee:da76f2c4c96028b7a6111aef4a50a94d' -H 0.0.0.0

# Command execution
smbmap.py -u ariley -p 'P@$$w0rd1234!' -d ABC -x 'net group "Domain Admins" /domain' -H 0.0.0.0
```

## `rpcclient` | port 445

{% hint style="info" %}
Authenticate '**Userless**' SMB Session with rpcclient
{% endhint %}

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
