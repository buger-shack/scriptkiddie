# 🔓 AppLocker

## Brief

{% hint style="info" %}
**AppLocker** can help you :

* Define **rules** based on file attributes that persist across app updates, such as the publisher name (derived from the digital signature), product name, file name, and file version. You can also create rules based on the file path and hash.
* Assign a rule to a security group or an individual user.
* Create **exceptions** to rules. For example, you can create a rule that allows all users to run all Windows binaries, except the Registry Editor (regedit.exe).
* Use audit-only mode to deploy the policy and understand its impact before enforcing it.
* Create rules on a staging server, test them, then export them to your production environment and import them into a Group Policy Object.
* Simplify creating and managing AppLocker rules by using Windows PowerShell.
{% endhint %}

## Check

```powershell
# check which files/extensions are blacklisted/whitelisted:
Get-ApplockerPolicy -Effective -xml

Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections

$a = Get-ApplockerPolicy -effective
$a.rulecollections
```

## ByPass

If AppLocker is configured with default AppLocker rules, we can bypass it by placing our executable in the following directory: _C:\Windows\System32\spool\drivers\color_ - Whitelisted by default.

{% embed url="https://github.com/api0cradle/UltimateAppLockerByPassList" %}
Check this
{% endembed %}

Useful **Writable folders** to bypass AppLocker Policy: If AppLocker is allowing to execute anything inside `C:\Windows\System32` or `C:\Windows` there are **writable folders** you can use to **bypass this**.

```
C:\Windows\System32\Microsoft\Crypto\RSA\MachineKeys
C:\Windows\System32\spool\drivers\color
C:\Windows\Tasks
C:\windows\tracing
```
