# Configuration

## Monkey365

{% embed url="https://github.com/silverhack/monkey365" %}

```powershell
# install
wget https://github.com/silverhack/monkey365/archive/refs/heads/main.zip -O monkey365
cd monkey365

Get-ChildItem -Recurse c:\monkey365 | Unblock-File
Import-Module monkey365
Import-Module C:\temp\monkey365
Import-Module C:\temp\monkey365 -Force


# usage
Get-Help Invoke-Monkey365
Get-Help Invoke-Monkey365 -Examples
Get-Help Invoke-Monkey365 -Detailed
```

## Recon

{% embed url="https://github.com/nyxgeek/o365recon" %}
