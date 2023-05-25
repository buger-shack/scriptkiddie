# Pivoting

{% hint style="info" %}
<mark style="color:blue;">Pivoting is the</mark> _<mark style="color:blue;">art</mark>_ <mark style="color:blue;">of</mark> <mark style="color:blue;">**using access obtained**</mark> <mark style="color:blue;">over one machine to</mark> <mark style="color:blue;">**exploit another machine deeper in the network**</mark><mark style="color:blue;">. It is one of the most essential aspects of network penetration testing.</mark>
{% endhint %}

* Put simply, by using one of the _techniques_, it becomes possible to gain initial access to a remote network, and use it to _access other machines in the network_ that would **not otherwise be accessible** :

![](<../.gitbook/assets/image (82).png>)

* In this diagram, there are **four** machines on the target network: one public facing server, with three machines which are not exposed to the internet. **By accessing the public server, we can then pivot to attack the remaining three targets.**

### :link: Source

{% embed url="https://tryhackme.com/room/wreath" %}

## High-level Overview

**Two main methods :**

* <mark style="color:blue;">**Tunnelling/Proxying**</mark> : Creating a **proxy type connection** through a _compromised machine_ in order to **route all desired traffic** into the _targeted network_. This could potentially also be tunnelled inside another protocol (e.g. SSH tunnelling), which can be useful for evading a basic Intrusion Detection System (IDS) or firewall.
* <mark style="color:blue;">**Port Forwarding**</mark> : Creating a connection between a **local port** and a **single port** on a _target_, via a _compromised host._

<mark style="color:blue;">A</mark> <mark style="color:blue;">**proxy**</mark> <mark style="color:blue;">is good to</mark> <mark style="color:blue;">**redirect**</mark> <mark style="color:blue;">lots of different kinds of traffic into the target network -- for example, with an nmap scan, or to access multiple ports on multiple different machines.</mark>

<mark style="color:blue;">**Port Forwarding**</mark> <mark style="color:blue;">tends to be faster and more reliable, but only allows to access a single port (or a small range) on a target device.</mark>

Which style of pivoting is more suitable will depend entirely on the **layout of the network**, so start with further enumeration before deciding how to proceed. It would be sensible at this point to also start to **draw up a layout of the network**.

**As a general rule**, if you have multiple possible entry-points, try to use a Linux/Unix target where possible, as these tend to be easier to pivot from. An outward facing Linux webserver is absolutely ideal.

**Enumerating a network using native and statically compiled tools :**

* [Proxychain](pivoting-tools/proxychains-foxyproxy.md) : Proxychains and FoxyProxy are used to access a proxy created with one of the other tools
* [SSH Tunnelling](pivoting-tools/ssh-tunnelling-port-forwarding.md) (primarily Unix) : SSH can be used to create both port forwards, and proxies
* [Plinx.exe](pivoting-tools/plinx.exe.md) (Windows) : plink.exe is an SSH client for Windows, allowing you to create reverse SSH connections on Windows
* [Socat ](pivoting-tools/socat.md)(Windows and Unix) : Socat is a good option for redirecting connections, and can be used to create port forwards in a variety of different ways
* [Chisel ](pivoting-tools/chisel.md)(Windows and Unix) : Chisel can do the exact same thing as with SSH portforwarding/tunneling, but doesn't require SSH access on the box
* [Sshuttle ](pivoting-tools/sshuttle.md)(currently Unix only) : sshuttle is a nicer way to create a proxy when we have SSH access on a target
* [Ligolo-Ng](pivoting-tools/ligolo-ng.md) : TODO

***

## Enumeration

As always, '_**enumeration is the key to success**. **Information is power**_**'**

* The more we know about our target, the more options we have available to us. As such, our first step when attempting to pivot through a network is to get an idea of what's around us.

> <mark style="color:blue;">**Five possible ways to enumerate a network through a compromised host :**</mark>
>
> * Using **material found** on the machine : i.e., _hosts file_ or _ARP cache_.
> * Using **pre-installed tools**
> * Using statically **compiled tools**
> * Using **scripting** techniques
> * Using **local tools** through a **proxy**
