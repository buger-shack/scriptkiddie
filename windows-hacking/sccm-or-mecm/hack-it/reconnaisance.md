# Reconnaisance

* Identify SCCM Environment
* Identify PXE Boot Media

## Scan

### Ports

* TCP Port : **8530**, **8531**, **10123** (Site Server, Management Point) ; **49152**-**49159** (Distribution Point)
* UDP Port : **4011** (Operating System Deployment OSD)
* Nessus Plugin: **Microsoft System Center Configuration Manager Management Point Detection**

### Tools

#### Windows

* SCCM native client (Control Panel, Configuration Manager)

```powershell
([ADSISearcher]("objectClass=mSSMSManagementPoint")).FindAll() | % {$_.Properties}

Get-WmiObject -Class SMS_Authority -Namespace root\CCM (need enrolled SCCM client)

.\SharpSCCM.exe local site-info
```

#### Linux

```bash
# PXEThief 
git clone https://github.com/MWR-CyberSec/PXEThief.git
cd PXEThief
pip3 install -r requirements.txt
# autodiscover via DHCP
python3 pxethief.py explore -i $interface
# or
python3 pxethief 2 sscm2.root.local

# SCCMHunter
git clone https://github.com/garrettfoster13/sccmhunter.git
cd sccmhunter
pip3 install -r requirements.txt
python3 sccmhunter.py find -u low -p 'Alphatango999!' -d root.local -dc-ip dc1.root.local
python3 sccmhunter.py smb -u low -p 'Alphatango999!' -d root.local -dc-ip dc1.root.local
python3 sccmhunter.py show -users
python3 sccmhunter.py show -computers
```

## Resources

[SCCM Exploitation, the First cred Is the deepest - BlackHillsInfoSec](https://www.blackhillsinfosec.com/wp-content/uploads/2023/08/SLIDES\_SCCM-Exploitation-The-First-Cred-Is-The-Deepest-II-Gabriel-Prudhomme-BHIS.pdf)
