# Shell Stabilizing

### Python shell

```bash
#On REV shell
which python
# python3
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm

# python2.7
python -c 'import pty; pty.spawn("/bin/bash")'
```

* **On Attacker PC** : <mark style="color:blue;">--</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**Ctrl + Z**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">--</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**Enter**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">--</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**stty raw -echo**</mark> in attacking terminal and note down the values for rows and columns.

```bash
stty raw -echo; fg
stty rows NUMBER cols NUMBER
```

### Script shell

```bash
# On REV shell
which script 
/usr/bin/script -qc /bin/bash /dev/null
export TERM=xterm
```
