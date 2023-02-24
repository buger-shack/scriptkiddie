# LLMNR\_NBT NS Poisoning

## Brief

Exploiting weaknesses in name resolution protocols is a common technique for performing man-in-the-middle (MITM) attacks. Two particularly vulnerable name resolution protocols are Link-Local Multicast Name Resolution (**LLMNR**) and NetBIOS Name Service (**NBNS**). Attackers leverage both of these protocols to respond to requests that fail to be answered through higher priority resolution methods, such as DNS. The default enabled status of LLMNR and NBNS within Active Directory (AD) environments allows this type of spoofing to be an extremely effective way to both gain initial access to a domain, and also elevate domain privilege during post exploitation efforts.

First, without implementing some router based wizardry, LLMNR and NBNS requests are contained within a single multicast or broadcast domain respectively. This can greatly limit the scope of a spoofing attack with regards to both the affected systems and potential privilege of the impacted sessions.

Second, by default, Windows systems use the following priority list while attempting to resolve name resolution requests through network based protocols:

1. DNS
2. LLMNR
3. NBNS

**Which means if the resolution using DNS doesn't fail, the client will probably not try to resolve via LLMNR or NBT-NS.**

## Attack

### `responder`

{% embed url="https://github.com/lgandx/Responder" %}

```bash
python responder.py -I <interface> -rdwv
```

**Now you should see some hashes (NTLMv2) captured**. The captured hashes are output into the logs file of Responder (_/usr/share/responder/logs_). At this point, you have two options, either relay the hash to try and have an open session or you can take the hash and try to crack it offline by running hashcat on it using the following command (depending on where you're running it, its best to run it on your host system) :

```bash
hashcat -m 5600 hashes.txt dictionary.txt
```

Check for more : Hashcat
