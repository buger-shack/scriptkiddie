# Find Users - Enumeration

```bash
# netexec
nxc smb $ip -u anonymous -p "" --rid-brute 10000

# kerbrute
kerbrute -domain $domain -dc-ip $ip -users /tools/payloads/SecLists/Usernames/xato-net-10-million-usernames.txt
```
