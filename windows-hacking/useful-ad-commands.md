# Useful AD Commands

## AD Commands

{% embed url="https://activedirectorypro.com/powershell-commands/" %}

{% tabs %}
{% tab title="Basics" %}
```powershell
# Get Execution Policy
get-executionpolicy

# Show PowerShell Version
$PSVersionTable
```
{% endtab %}

{% tab title="Users" %}
```powershell
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
```
{% endtab %}

{% tab title="Groups" %}
{% code fullWidth="true" %}
```powershell
# Get All Security Groups
Get-ADGroup -filter *

# Get All Computers by Name
Get-ADComputer -filter * | select name

# Get all Windows 10 Computers
Get-ADComputer -filter {OperatingSystem -Like '*Windows 10*'} -property * | select name, operatingsystem
```
{% endcode %}
{% endtab %}

{% tab title="Computer" %}
```powershell
# Get All Computers
Get-AdComputer -filter *
```
{% endtab %}

{% tab title="GPO" %}
```powershell
# Get all GPOs by status
get-GPO -all | select DisplayName, gpostatus
```
{% endtab %}

{% tab title="Domain" %}
```powershell
# all Active Directory commands
get-command -Module ActiveDirectory

# Basic Domain Information
Get-ADDomain

# Get all DC 
Get-ADDomainController -filter * | select hostname, operatingsystem
```
{% endtab %}

{% tab title="Passwords" %}
```powershell
# Get all Fine Grained Password Policies
Get-ADFineGrainedPasswordPolicy -filter *

# Get Domain Default Password Policy
Get-ADDefaultDomainPasswordPolicy
```
{% endtab %}
{% endtabs %}
