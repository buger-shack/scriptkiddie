---
description: Configuration Audit of SCCM with best practices commented
---

# Configuration Audit

## PowerShell with Admin Priv

```powershell
## Is the host a DC
Import-Module ActiveDirectory
Get-ADDomainController -Filter * | Select-Object HostName, IsGlobalCatalog

## Obtain SCCM's default security groups and check for unexpected changes
Get-ADGroup -Filter 'Name -like "SMS_SiteSystemToSiteServerConnection_*"' | ForEach-Object {
    Get-ADGroupMember -Identity $_
}

## Check ACLs of .PFX certificate files used by SCCM servers
Get-Acl -Path "path\of\the\certificate.pfx" | Format-List

# Get the RootKey
(Get-WmiObject -Namespace root\ccm\locationservices -Class TrustedRootKey).TrustedRootKey
```

## SCCM's PowerShell

```powershell
## Which CM Distribution Point is configured
(Get-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\SMS\DP -Name ManagementPoints).ManagementPoints

## SCCM Share
net view \\sccm /all

## Collect the Site Configuration
Get-CMSite

## Obtain SCCM administrative users and their roles
Get-CMAdministrativeUser | Select-Object Name, RoleName
# or
Get-CMAdministrativeUser

## Retrieve infos about the management point configuration
### SslState : 0 not encrypted
Get-CMManagementPoint

## Collect the Distribution Point Configuration
Get-CMDistributionPoint

## Presence of sensitive data in task sequences
$SensitiveKeywords = @(
    "pass",
    "passwd",
    "password",
    "secret",
    "key",
    "credential",
    "token",
    "apikey",
    "api_key",
    "accesskey",
    "access_key",
    "secretkey",
    "secret_key",
    "privatekey",
    "private_key",
    "passphrase",
    "login",
    "signin",
    "auth",
    "authentication",
    "encryption",
    "certificate",
    "cert"
)
$TaskSequences = Get-CMTaskSequence
foreach ($TS in $TaskSequences) {
    $TSName = $TS.Name
    $TSSteps = Get-CMTaskSequenceStep -TaskSequencePackage $TS.PackageID
    foreach ($Step in $TSSteps) {
        foreach ($Keyword in $SensitiveKeywords) {
            if ($Step.CommandLine -like "*$Keyword*") {
                Write-Output "Potential sensitive data (`"$Keyword`") found in Task Sequence: $TSName, Step: $($Step.StepName)"
            }
        }
    }
}

## Use system-specific authorization for OSD
# Retrieve device collections with OSD task sequences deployed
### CollectionName : should reference a specific collection and not broad groups like "All Systems" to avoid unauthorized access
Get-CMTaskSequenceDeployment | Select-Object CollectionName, DeploymentID, PackageID

# Check PXE parameters on distribution points
### PxeSupport : 'True'
### PxePasswordEnabled : 'True'
Get-CMDistributionPoint | Select-Object ServerName, PxeSupport, PxePasswordEnabled


## Keep WIM up-to-date with the latest security updates
### Version : latest
Get-CMOperatingSystemImage | Select-Object PackageID, Name, Version, ImagePath

# Client Status
### ADRetrievingSchedule   : (depends on the AD)
### CleanUpInterval        : 31
### DDRInactiveInterval    : 7
### HWInactiveInterval     : 7
### NeedADLastLogonTime    : (optional)
### PolicyInactiveInterval : 7
### SettingsID             : 1
### StatusInactiveInterval : 7
### SWInactiveInterval     : 7

Get-CMClientStatusSetting

# Collect the Software Update Point Configuration
### Check SslState
Get-CMSoftwareUpdatePoint
```

## SCCM GUI

```markdown
# Require approval of computers from untrusted domains 
- Console Configuration Manager (SCCM) > Administration > Site configuration > Sites > "Hierarchy settings" in the "Settings" group.
# Only approved Console Extensions 
- Console Configuration Manager (SCCM) > Administration > Site configuration > Sites > "Hierarchy settings" in the "Settings" group > General > (check box Only allow console extension that are approved for the hierarchy)

# Signature & encryption
### PKI (HTTPS) & Signature enabled
- Console Configuration Manager (SCCM) > Administration > Site configuration > Sites > (Select main site) > Properties > Signature and encryption tab
```
