# PowerView

## Brief

Download [PowerView](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1) to the host you compromised.

## Steps

```powershell
# launch ps and bypass the execution policy
powershell -ep bypass

# load powerview
. .\PowerView.ps1

# Get infos
Get-NetDomain
Get-NetDomainController
(Get-DomainPolicy)."system access"
# look for shares
Invoke-ShareFinder
# gpo
Get-NetGPO
```

### Cheatsheet of commands

{% embed url="https://gist.githubusercontent.com/HarmJ0y/184f9822b195c52dd50c379ed3117993/raw/e5e30c942adb2347917563ef0dafa7054882535a/PowerView-3.0-tricks.ps1" %}
