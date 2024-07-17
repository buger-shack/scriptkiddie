# 👥 Active Directory

<figure><img src="../../.gitbook/assets/image (160).png" alt="" width="563"><figcaption></figcaption></figure>

> Active Directory is a widely used directory service by Microsoft that stores information about users, computers, and other resources on a network.&#x20;
>
> As with any technology, Active Directory has its own set of vulnerabilities that can be exploited by attackers to gain unauthorized access to network resources.&#x20;
>
> **Some common Active Directory vulnerabilities are:**

1. **Weak passwords**: Weak passwords or passwords that are easily guessable are one of the most common Active Directory vulnerabilities. Attackers can use automated tools to try multiple passwords until they find the correct one and gain access to the system.
2. **Pass the hash attacks**: Pass the hash (PtH) is a type of attack that involves stealing the hashed password of a user and using it to authenticate to other systems on the network. This type of attack is particularly dangerous because the attacker does not need to know the user's plaintext password.
3. **Kerberos attacks**: Kerberos is a network authentication protocol used by Active Directory. Kerberos attacks involve exploiting vulnerabilities in the Kerberos protocol to gain unauthorized access to network resources.
4. **Domain controller vulnerabilities**: Domain controllers are the backbone of an Active Directory environment. If an attacker gains access to a domain controller, they can take control of the entire network.
5. **Group Policy vulnerabilities**: Group Policy is a powerful tool used to manage security settings in Active Directory. Misconfigured Group Policy settings can leave a network vulnerable to attack.
6. **Unsecured LDAP traffic**: LDAP (Lightweight Directory Access Protocol) is used to communicate with Active Directory. If LDAP traffic is not encrypted, an attacker can intercept it and steal sensitive information.
7. **Privilege escalation**: If an attacker gains access to a low-privileged account, they can attempt to escalate their privileges and gain administrative access to the system.

## Windows Active Directory Pentest Methodology

{% content-ref url="1.-reconnaissance/" %}
[1.-reconnaissance](1.-reconnaissance/)
{% endcontent-ref %}

{% content-ref url="2.-initial-attack-vectors/" %}
[2.-initial-attack-vectors](2.-initial-attack-vectors/)
{% endcontent-ref %}

{% content-ref url="3.-post-compromise-enumeration/" %}
[3.-post-compromise-enumeration](3.-post-compromise-enumeration/)
{% endcontent-ref %}

{% content-ref url="4.-post-compromise-attacks/" %}
[4.-post-compromise-attacks](4.-post-compromise-attacks/)
{% endcontent-ref %}

{% content-ref url="5.-privesc-and-misc/" %}
[5.-privesc-and-misc](5.-privesc-and-misc/)
{% endcontent-ref %}
