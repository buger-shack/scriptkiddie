# Firewalls

## Brief

{% hint style="info" %}
A firewall is **software** or **hardware** that **monitors** the **network traffic** and compares it against a set of rules before passing or blocking it. One simple analogy is a guard or gatekeeper at the entrance of an event. This gatekeeper can check the ID of individuals against a set of rules before letting them enter (or leave).
{% endhint %}

## Types

Based on firewall abilities, we can list the following firewall types:

* <mark style="color:blue;">**Packet-Filtering Firewall**</mark> : Packet-filtering is the most basic type of firewall. This type of firewall inspects the protocol, source and destination IP addresses, and source and destination ports in the case of TCP and UDP datagrams. It is a stateless inspection firewall.
* <mark style="color:blue;">**Circuit-Level Gateway**</mark> : In addition to the features offered by the packet-filtering firewalls, circuit-level gateways can provide additional capabilities, such as checking TCP three-way-handshake against the firewall rules.
* <mark style="color:blue;">**Stateful Inspection Firewall**</mark> : Compared to the previous types, this type of firewall gives an additional layer of protection as it keeps track of the established TCP sessions. As a result, it can detect and block any TCP packet outside an established TCP session.
* <mark style="color:blue;">**Proxy Firewall**</mark> : A proxy firewall is also referred to as Application Firewall (AF) and Web Application Firewall (WAF). It is designed to masquerade as the original client and requests on its behalf. This process allows the proxy firewall to inspect the contents of the packet payload instead of being limited to the packet headers. Generally speaking, this is used for web applications and does not work for all protocols.
* <mark style="color:blue;">**Next-Generation Firewall (NGFW)**</mark> : NGFW offers the highest firewall protection. It can practically monitor all network layers, from OSI Layer 2 to OSI Layer 7. It has application awareness and control. Examples include the Juniper SRX series and Cisco Firepower.
* <mark style="color:blue;">**Cloud Firewall or Firewall as a Service (FWaaS)**</mark> : FWaaS replaces a hardware firewall in a cloud environment. Its features might be comparable to NGFW, depending on the service provider; however, it benefits from the scalability of cloud architecture. One example is Cloudflare Magic Firewall, which is a network-level firewall. Another example is Juniper vSRX; it has the same features as an NGFW but is deployed in the cloud. It is also worth mentioning AWS WAF for web application protection and AWS Shield for DDoS protection.

![NGFW - Schema](<../../.gitbook/assets/image (20).png>)
