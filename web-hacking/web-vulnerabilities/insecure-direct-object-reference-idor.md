# IDOR

<details>

<summary>What is IDOR ?</summary>

It is a type of an **Access Control Vulnerability**.

* _An Access Control Vulnerability is when an attacker can gain access to information or actions not intended for them_.

An **IDOR** vulnerability can occur when a web server receives user-supplied input to retrieve objects (files, data, documents), and **too much trust** has been placed on that input data, and the web application does not validate whether the user should, in fact, have access to the requested object.

</details>

## Find and Exploit

![Changing the id value can show us personal infos about other users (IDOR)](<../../.gitbook/assets/image (30).png>)

### **Post Variables**

Examining the contents of forms on a website can sometimes reveal fields that could be vulnerable to IDOR exploitation.

For instance, the following HTML code for a form that updates a user's password :

![](<../../.gitbook/assets/image (24).png>)

### **Cookies**

![Cookie value changed to 5](<../../.gitbook/assets/image (118).png>)

{% embed url="https://portswigger.net/web-security/access-control/idor" %}
More
{% endembed %}
