# Forensics

## Image analysis

### Aperi'Solve

{% embed url="https://www.aperisolve.com/" %}
performs layer analysis on image ; uses exiftool, binwalk, zsteg etc
{% endembed %}

```bash
# usage on cli
sudo sh -c "$(curl -fs https://www.aperisolve.com/install.sh)"
aperisolve <image>
```

### Ghiro

> Ghiro is a fully automated tool designed to run forensics analysis over a massive amount of images, just using an user friendly and fancy web application.

{% embed url="https://www.getghiro.org" %}

#### Set Up

[Download Ghiro](https://www.getghiro.org/appliances/0.2.1/Ghiro\_Appliance\_0.2.1-1.zip)

#### Usage

{% embed url="https://www.hackingarticles.in/forensic-investigation-ghiro-for-image-analysis" %}

> Username: ghiro
>
> Password: ghiromanager

### Stereograms

> An autostereogram (also known as Magic Eye) is a 2D image designed to create the illusion of 3D. In each image, there is a 3D object that can only be viewed by looking at the image a certain way, as if the screen was transparent and you looked at the wall behind it.

{% embed url="https://piellardj.github.io/stereogram-solver/" %}
if you can't do it with your eyes... works perfectly
{% endembed %}

## Disk Image Analysis

### Autopsy Tool

> Autopsy is an open-source tool that is used to perform forensic operations on the disk image of the evidence.

#### Set Up

{% embed url="https://www.autopsy.com/download" %}

#### Usage

![](<../.gitbook/assets/image (148).png>)

## Memory Analysis

### Volatility

> Volatility is a tool that can be used to analyze a volatile memory of a system. You can inspect processes, look at command history, and even pull files and passwords from a system without even being on the system.

#### Installation

```bash
# volatility3
git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
python3 setup.py install
python3 vol.py —h

# volatility2
# Download the executable from https://www.volatilityfoundation.org/26
git clone https://github.com/volatilityfoundation/volatility.git
cd volatility
python setup.py install
```

#### Useful commands

```bash
# image infos
volatility -f file.mem imageinfo

# Hive and Registry key values
volatility -f file.mem --profile=MyProfile hivelist
volatility -f file.mem --profile=MyProfile printkey -K "MyPath"

# Analyzing processes
volatility -f file.mem --profile=Win7SP1x64 pslist
# list parent-child relations processes
volatility -f file.mem --profile=Win7SP1x64 pstree

# list all app running
volatility-f file.mem --profile=Win7SP1x64 shimcache > shimcache.txt

# analyze network connections
volatility -f file.mem --profile=Win7SP1x64 netscan > output_netscan.txt
# running sockets & open connections
volatility -f file.mem --profile=Win7SP1x64 connscan
volatility -f file.mem --profile=Win7SP1x64 sockets

# commandline history
volatility -f file.mem --profile=Win7SP1x64 cmdline
volatility -f file.mem --profile=Win7SP1x64 consoles
```

#### Detect malicious files

In volatility, there exists an attribute named malfind. This is actually an inbuilt plugin and can be used for malicious process detection.

```bash
volatility -f file.mem --profile=Win7SP1x64 -D <Output_Location> -p $PID malfind
# dump infected process
volatility -f file.mem --profile=Win7SP1x64 procdump -p 3496 --dump-dir $dumpfolder
```

## USB Forensics

> Universal Serial Bus flash drives, commonly known as USB flash drives are the most common storage devices which can be found as evidence in Digital Forensics Investigation. The digital forensic investigation involves following a defined procedure for investigation which needs to be performed in such a manner that the evidence isn’t destroyed.

### Tutorials

{% embed url="https://www.hackingarticles.in/usb-forensics-detection-investigation" %}
