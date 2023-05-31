# Cover tracks

## Log files

```bash
/etc/syslog.conf

# in this file, you can read all the logs that Syslog logs.
# on linux/unix, a lot of systems logs are stored : 
/var/logs
# i.e. 
/var/log/messages
/var/log/auth.log # ssh, sudo attempts

# APACHE
/var/log/apache2/access.log
/var/log/apache2/error.log

# remove your ip :
grep -v '$src-ip-address' /path/to/access_log > a && mv a /path/to/access_log
grep -v <entry-to-remove> <logfile> > /tmp/a ; mv /tmp/a <logfile> ; rm -f /tmp/a

# utmp / wtmp
who
last
lastlog

# COMMAND HISTORY
echo $HISTFILE
# You can set your file size like this to zero, to avoid storing commands.
export HISTSIZE=0

# SHRED FILES
# lets you remove files in a more secure way
shred -zu $filename
```

## MoonWalk :man\_dancing:

<figure><img src="https://media4.giphy.com/media/12cpBxBl4WqlHO/giphy.gif?cid=ecf05e47rtrh31z2kel5j3sifm58o43i91cxklvkhd6sv2fc&#x26;ep=v1_gifs_search&#x26;rid=giphy.gif&#x26;ct=g" alt="" width="188"><figcaption></figcaption></figure>

{% hint style="info" %}
MoonWalk is a 400 KB single-binary executable that can clear your traces while penetration testing a Unix machine. It saves the state of system logs pre-exploitation and reverts that state including the filesystem timestamps post-exploitation leaving zero traces of a ghost in the shell.
{% endhint %}

### Installation

```bash
curl -L https://github.com/mufeedvh/moonwalk/releases/download/v1.0.0/moonwalk_linux -o moonwalk
# or
cargo install --git https://github.com/mufeedvh/moonwalk.git
# from source
git clone https://github.com/mufeedvh/moonwalk.git
cd moonwalk/
cargo build --release
```

### Usage

```bash
# once we get a shell into the target unix machine, start moonwalk session
moonwalk start
# clear our traces 
moonwalk finish
```
