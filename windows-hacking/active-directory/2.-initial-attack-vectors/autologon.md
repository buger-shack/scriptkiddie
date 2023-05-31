# Autologon

> Instead of waiting for a user to enter their name and password, Windows uses the credentials you enter with Autologon, which are encrypted in the Registry, to log on the specified user automatically.

```powershell
# get the password
reg.exe query "HKLM\software\microsoft\windows nt\currentversion\winlogon"
```

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
