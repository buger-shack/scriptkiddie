# 2. Initial Attack Vectors

## linWinPwn

{% hint style="info" %}
linWinPwn is a bash script that automates a number of Active Directory Enumeration and Vulnerability checks. The script uses a number of tools and serves as wrapper of them. Tools include: impacket, bloodhound, crackmapexec, enum4linux-ng, ldapdomaindump, lsassy, smbmap, kerbrute, adidnsdump, certipy, silenthound, and others.
{% endhint %}

{% embed url="https://github.com/lefayjey/linWinPwn" %}

```bash
# INSTALL
git clone https://github.com/lefayjey/linWinPwn
cd linWinPwn; chmod +x linWinPwn.sh
chmod +x install.sh
./install.sh
./linWinPwn.sh -h

# USAGE
# on the Windows host, run using PowerShell:
ssh kali@$ip_attacker -R 1080 -NCqf

# On the Linux machine, first update /etc/proxychains4.conf to include socks5 127.0.0.1 1080, then run:
proxychains ./linWinPwn.sh -t $DC_IP
```
