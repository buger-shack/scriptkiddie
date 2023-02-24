# 🔏 BitLocker

## Bypassing Bitlocker

Bitlocker uses 2 passwords. The one used by the user, and the recovery password (48 digits). If you are lucky and inside the current session of Windows exists the file C:\Windows\MEMORY.DMP (It is a memory dump) you could try to search inside of it the recovery password. You can get this file and a copy of the filesytem and then use Elcomsoft Forensic Disk Decryptor to get the content (this will only work if the password is inside the memory dump). You could also force the memory dump using NotMyFault of Sysinternals, but this will reboot the system and has to be executed as Administrator. You could also try a bruteforce attack using Passware Kit Forensic.

### Check if enabled

```powershell
manage-bde -status
# powershell
Get-BitLockerVolume
```

### Social Engineering

Finally, you could make the user add a new recovery password making him executed as administrator:

```powershell
schtasks /create /SC ONLOGON /tr "c:/windows/system32/manage-bde.exe -protectors -add c: -rp 000000-000000-000000-000000-000000-000000-000000-000000" /tn tarea /RU SYSTEM /f
```

This will add a new recovery key (composed of 48 zeros) in the next login. To check the valid recovery keys you can execute:

```bash
manage-bde -protectors -get c:
```
