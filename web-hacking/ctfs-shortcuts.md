# 🚩 CTFs shortcuts

<figure><img src="../.gitbook/assets/ctf.gif" alt="" width="360"><figcaption></figcaption></figure>

## Find flag

```bash
# if the flag looks like : flag{*****}
grep -irl flag{ $path

# if the flag is a .txt file
find / -iname "*.txt" 2>/dev/null
find / -iname "config.php" 2>/dev/null
find / -iname "flag.txt" 2>/dev/null

# find presence of chrootkit
find / -type f -name chkrootkit 2>/dev/null
```

## Web Dev

### Mozilla Firefox

`CTRL + SHIFT + I`

## HTTP Headers Response

```bash
curl -sSL -D - $ip -o /dev/null
curl -s -I -X POST http://$ip
```

## Stabilize Reverse shell

### use CTRL + C

```bash
# In reverse shell
python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z
# In OS
stty raw -echo
fg
```

There's three popular ways I use to stabilize a reverse shell;

* _Python_, as mentioned above.
* _riwrap_, which prepends to a netcat shell for additional terminal features.
* _Socat_, which is a step above netcat but must be manually transferred over and launched on the target machine.
