# 🔥 Evasion

<figure><img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExODVmNWU2NTM4NmFkYTZlNDNlOWE1MjU2YThmOTUzYTIyZDcwMDM1NyZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/xT5LMxyx0Hj5zHuPgQ/giphy.gif" alt="" width="375"><figcaption></figcaption></figure>

{% embed url="https://tryhackme.com/room/redteamfirewalls" %}
Source
{% endembed %}

## Controlling the source IP/MAC/PORT

**Nmap** allows you to hide or spoof the source as you can use :

* Decoy(s)
* Proxy
* Spoofed MAC Address
* Spoofed Source IP Address
* Fixed Source Port Number

### Decoy(s)

Hide your scan with decoys. Using decoys makes your IP address mix with other “decoy” IP addresses. Consequently, it will be difficult for the firewall and target host to know where the port scan is coming from. Moreover, this can exhaust the blue team investigating each source IP address.

Using the `-D` option, you can add decoy source IP addresses to confuse the target :

```bash
nmap -sS -Pn -D 10.10.10.1,10.10.10.2,ME -F MACHINE_IP
```

You can also set Nmap to use random source IP addresses instead of explicitly specifying them. By running :

```bash
nmap -sS -Pn -D RND,RND,ME -F MACHINE_IP
```

Nmap will choose two random source IP addresses to use as decoys.

### **Proxy**

**Use an HTTP/SOCKS4 proxy.** Relaying the port scan via a proxy helps keep your IP address unknown to the target host. This technique allows you to keep your IP address hidden while the target logs the IP address of the proxy server.

You can go this route using the Nmap option `--proxies PROXY_URL`. For example :

```bash
nmap -sS -Pn --proxies PROXY_URL -F MACHINE_IP
```

This will send all its packets via the proxy server you specify. _Note that you can chain proxies using a comma-separated list._

### **Spoofed MAC Address**

Spoof the source MAC address. Nmap allows you to **spoof your MAC address** using the option `--spoof-mac MAC_ADDRESS`. This technique is tricky; spoofing the MAC address works only if your system is on the **same network segment as the target** host.

The target system is going to reply to a spoofed MAC address. It allows you to exploit any trust relationship based on MAC addresses. Moreover, you can use this technique to hide your scanning activities on the network. For example, you can make your scans appear as if coming from a network printer.

### **Spoofed IP Address**

Spoof the source IP address. Nmap lets you **spoof your IP address** using `-S IP_ADDRESS`. Spoofing the IP address is useful if your system is on the **same subnetwork as the target** host; otherwise, you won’t be able to read the replies sent back. The reason is that the target host will reply to the spoofed IP address, and unless you can capture the responses, you won’t benefit from this technique.

Another use for spoofing your IP address is when you control the system that has that particular IP address. Consequently, if you notice that the target started to block the spoofed IP address, you can switch to a different spoofed IP address that belongs to a system that you also control. This scanning technique can help you maintain stealthy existence; moreover, you can use this technique to exploit trust relationships on the network based on IP addresses.

### **Fixed Source Port Number**

Use a specific **source port number**. Scanning from one particular source port number can be **helpful if you discover that the firewalls allow incoming packets from particular source port** numbers, such as port **53** or **80**. Without inspecting the packet contents, packets from source TCP port 80 or 443 look like packets from a web server, while packets from UDP port 53 look like responses to DNS queries.

You can set your port number using `-g` or `--source-port` options.

### Summary

```bash
# Hide a scan with decoys 	
-D DECOY1_IP1,DECOY_IP2,ME
# Hide a scan with random decoys 	
-D RND,RND,ME
# Use an HTTP/SOCKS4 proxy to relay connections 	
--proxies PROXY_URL
# Spoof source MAC address 	
--spoof-mac MAC_ADDRESS
# Spoof source IP address 	
-S IP_ADDRESS
# Use a specific source port number 
-g PORT_NUM or --source-port PORT_NUM
```

## Forcing Fragmentation, MTU & Data Length

You can control the packet size as it allows you to:

* **Fragment packets**, optionally with given MTU. If the firewall, or the IDS/IPS, does not reassemble the packet, it will most likely let it pass. Consequently, the target system will reassemble and process it.
* Send packets with specific **data lengths**.

### Fragment your packets with 8 bytes of data

One easy way to fragment your packets would be to use the `-f` option. This option will **fragment** the IP packet to carry only **8 bytes** of data.

### Fragment your packets with 16 bytes of data

Another handy option is the `-ff`, limiting the IP data to **16 bytes :**

```bash
nmap -sS -Pn -ff -F MACHINE_IP
```

We expect the **24** bytes of the TCP header to be divided between two IP packets, **16 + 8**, because **-ff** has put an upper limit of **16** bytes.

### Fragment Your Packets According to a Set MTU

Another neat way to fragment your packets is by setting the **MTU**. In Nmap, `--mtu VALUE` specifies the number of bytes per IP packet. In other words, the IP header size is not included. **The value set for MTU must always be a multiple of 8.**

_For instance, Ethernet has an MTU of **1500**, meaning that the largest IP packet that can be sent over an Ethernet (link layer) connection is 1500 bytes._

### Generate Packets with Specific Length

In some instances, you might find out that the **size of the packets** is **triggering** the firewall or the IDS/IPS to detect and block you. **If you ever find yourself** in such a situation, you can make your port scanning more evasive by setting a specific length. You can set the length of data carried within the IP packet using `--data-length VALUE`. _Again, remember that the length should be a multiple of 8._

### Summary

```bash
# Fragment IP data into 8 bytes 
-f

# Fragment IP data into 16 bytes 
-ff

# Fragment packets with given MTU
--mtu VALUE

# Specify packet length 	
--data-length NUM
```

## Modifying Header Fields

Nmap allows you to **control various header fields** that might help evade the firewall. You can :

* Set IP time-to-live
* Send packets with specified IP options
* Send packets with a wrong TCP/UDP checksum

### Set TTL

Nmap gives you further control over the different fields in the IP header. One of the fields you can control is the **Time-to-Live (TTL)**. Nmap options include `--ttl` VALUE to set the TTL to a custom value. This option might be useful if you think the default TTL exposes your port scan activities.

### Set IP Options

One of the IP header fields is the IP Options field. Nmap lets you control the **value set in the IP Options** field using `--ip-options HEX_STRING`, where the hex string can specify the bytes you want to use to fill in the IP Options field. Each byte is written as `\xHH`, where HH represents two hexadecimal digits, i.e., **one byte**.

### Use a Wrong Checksum

You can use this to your advantage to **discover more about the systems** in your network. All you need to do is add the option `--badsum` to your Nmap command.

### Summary

```bash
# Set IP time-to-live field 	
--ttl VALUE
# Send packets with specified IP options 	
--ip-options OPTIONS
# Send packets with a wrong TCP/UDP checksum 
--badsum
```

## Port Hopping

**Three** common firewall evasion techniques are:

* Port hopping
* Port tunneling
* Use of non-standard ports

Port hopping is a technique where an application hops **from one port to another** till it can establish and maintain a connection. In other words, the application might try different ports till it can successfully establish a connection.

![Port Hopping - Schema](<../../.gitbook/assets/image (78).png>)

### Port Tunneling

Port tunneling is also known as **port forwarding** and **port mapping**. In simple terms, this technique forwards the packets sent to one destination port to another destination port. For instance, packets sent to port 80 on one system are forwarded to port 8080 on another system.

#### Port Tunneling Using ncat

Consider the case where you have a server behind the firewall that you cannot access from the outside. However, you discovered that the firewall does not block specific port(s). You can use this knowledge to your advantage by tunneling the traffic via a different port.

We have an SMTP server listening on port **25**; however, we cannot connect to the SMTP server because the firewall blocks packets from the Internet sent to destination port 25. We discover that packets sent to destination port **443** are not blocked, so we decide to take advantage of this and send our packets to port 443, and after they pass through the firewall, we **forward them to port 25**. Let’s say that we can run a command of our choice on one of the systems behind the firewall. We can use that system to forward our packets to the SMTP server using the following command.

`ncat -lvnp 443 -c "ncat TARGET_SERVER 25"`

`-lvnp 443` listens on TCP port 443. Because the port number is less than 1024, you need to run ncat as root in this case.

`-c` or `--sh-exec` executes the given command via _/bin/sh_.

`"ncat TARGET_SERVER 25"` will connect to the target server at port 25.

As a result, `ncat` will listen on port 443, but it will forward all packets to port 25 on the target server. Because in this case, the firewall is blocking port 25 and allowing port 443, port tunneling is an efficient way to evade the firewall.

![Port Tunneling - Schema](<../../.gitbook/assets/image (71).png>)
