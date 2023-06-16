# Ping-Pong

## TcpDump

{% hint style="info" %}
Check **connectivity** between attacker and victim.
{% endhint %}

```bash
#Victim
ping 10.8.132.133
```

```bash
#Attacker
sudo tcpdump -i tun0 icmp
```

```bash
#Networks accessible via the VPN
#Checking Routing Table | Linux

netstat -rn
sudo netstat -rn

#Checking current connections
netstat -an
netstat -ano | findstr TCP | findstr ":0"
```
