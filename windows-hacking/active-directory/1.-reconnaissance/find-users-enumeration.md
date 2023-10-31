# Find Users - Enumeration

```bash
# cme
cme smb 10.10.11.236 -u anonymous -p "" --rid-brute 10000

# kerbrute
kerbrute -domain manager.htb -dc-ip 10.10.11.236 -users /tools/payloads/SecLists/Usernames/xato-net-10-million-usernames.txt
```
