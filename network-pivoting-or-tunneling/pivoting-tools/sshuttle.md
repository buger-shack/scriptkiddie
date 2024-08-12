---
description: >-
  It uses an SSH connection to create a tunnelled proxy that acts like a new
  interface. In short, it simulates a VPN, allowing us to route our traffic
  through the proxy without the use of proxychains.
---

# Sshuttle

{% embed url="https://github.com/sshuttle/sshuttle" %}

`Documentation :` https://sshuttle.readthedocs.org/

* We can just directly connect to devices in the **target network** as we would normally connect to networked devices. As it creates a **tunnel through SSH**, anything we send through the tunnel is also **encrypted**.

```
Drawbacks : sshuttle only works on Linux targets. Requires access to the compromised server via SSH, and Python also needs to be installed on the server.
```

```bash
# install sshuttle
sudo apt install sshuttle

# base command (pwd)
sshuttle -r username@address subnet

# use -N option to automatically detect the subnet (pwd)
sshuttle -r username@address -N

# base command (key-based)
sshuttle -r user@address --ssh-cmd "ssh -i KEYFILE" SUBNET
```

```bash
# Note !!
#***
# client: Connected.
# client_loop: send disconnect: Broken pipe
# client: fatal: server died with error code 255
#***

# May occur when the compromised machine you're connecting to is part of the subnet you're attempting to gain access to. For instance, if you were connecting to 172.16.0.5 and trying to forward 172.16.0.0/24, then you would be including the compromised server inside the newly forwarded subnet, thus disrupting the connection and causing the tool to die.

# a simple workaround, exclude the compromised machine :
sshuttle -r user@172.16.0.5 172.16.0.0/24 -x 172.16.0.5
```
