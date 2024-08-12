---
description: >-
  Both allow us to connect through one of the proxies. When creating a proxy we
  open up a port on our own attacking machine which is linked to the compromised
  server, giving us access to the target netw
---

# Proxychains / FoxyProxy

{% hint style="info" %}
Think of this as being something like a tunnel created between a port on our attacking box that comes out inside the target network -- like a secret tunnel from a fantasy story, hidden beneath the floorboards of the local bar and exiting in the palace treasure chamber.

Proxychains and FoxyProxy can be used to direct our traffic through this port and into our target network.
{% endhint %}

### Proxychains

Proxychains can often slow down a connection : performing an nmap scan through it is especially hellish. Ideally you should try to use **static tools where possible**, and route traffic through proxychains **only when required**.

```bash
# to use proxychain
proxychain <application -options>

# eg : use netcat with traffic passing through proxychian
proxychains nc $ip $port
```

The master config file is located at _/etc/proxychains.conf_

However, it's actually the last location where proxychains will look. The locations (in order) are:

- The current directory (i.e. ./proxychains.conf)
- ~/.proxychains/proxychains.conf
- /etc/proxychains.conf

**When performing Nmap scan through proxychains :**

- Comment out the proxy_dns

- Can only use TCP scans -- so no UDP or SYN scans. ICMP Echo packets (Ping requests) will also not work through the proxy, so use the  -Pn  switch to prevent Nmap from trying it.

- It will be extremely slow. Try to only use Nmap through a proxy when using the NSE (i.e. use a static binary to see where the open ports/hosts are before proxying a local copy of nmap to use the scripts library).

### FoxyProxy

Proxychains is an **acceptable** option when working with **CLI tools**, but if working in a **web browser** to access a **webapp** through a proxy, there is a better option available, namely: **FoxyProxy**!

![](<../../.gitbook/assets/image (75).png>)
