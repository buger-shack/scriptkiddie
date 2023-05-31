# Useful AD Commands

## AD Commands

```powershell
'''
BASICS
'''
# Get Execution Policy
get-executionpolicy

# Show PowerShell Version
$PSVersionTable

'''
USERS
'''
# User list
Get-ADUser -Filter *

# Get User and List All Properties (attributes)
Get-ADUser username -Properties *

# Get All Users From a Specific  OU
Get-ADUser -SearchBase “OU=ADPRO Users,dc=ad,dc=activedirectorypro.com” -Filter *

# Get AD Users by Name
get-Aduser -Filter {name -like "*robert*"}

# Find All Locked User Accounts
Search-ADAccount -LockedOut

# create a new user
net user john abc123!

# create a domain user
net user john abc123! /add /domain

# add the created user to a group
net group "Exchange Windows Permissions" john /add

# add the created user to a localgroup
net localgroup "Remote Management Users" john /add

# to verify
net user john

'''
GROUPS
'''
# Get All Security Groups
Get-ADGroup -filter *

# Get All Computers by Name
Get-ADComputer -filter * | select name

# Get all Windows 10 Computers
Get-ADComputer -filter {OperatingSystem -Like '*Windows 10*'} -property * | select name, operatingsystem

'''
COMPUTER
'''
# Get All Computers
Get-AdComputer -filter *

'''
GPOs
'''
# Get all GPOs by status
get-GPO -all | select DisplayName, gpostatus

'''
DOMAIN
'''
# all Active Directory commands
get-command -Module ActiveDirectory

# Basic Domain Information
Get-ADDomain

# Get all DC 
Get-ADDomainController -filter * | select hostname, operatingsystem

'''
PASSWORDS
'''
# Get all Fine Grained Password Policies
Get-ADFineGrainedPasswordPolicy -filter *

# Get Domain Default Password Policy
Get-ADDefaultDomainPasswordPolicy
```
