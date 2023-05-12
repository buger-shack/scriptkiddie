# IOTGoat OWASP | Walkthrough

![](<../../.gitbook/assets/image (1).png>)

> Copyright to book.redsquad.xyz

## Prerequisites

```bash
# get the iotgoat .img for static analysis and .vmdk for dynamic analysis (run it locally)
https://github.com/OWASP/IoTGoat/releases

# packets
apt update
apt install binwalk
apt install squashfs-tools 

# TOOLS
# firmwalker
mkdir tools
cd tools
git clone https://github.com/scriptingxss/firmwalker.git

# testssl
git clone https://github.com/drwetter/testssl.sh.git

# linpeas
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```

## Static Analysis <a href="#static-analysis" id="static-analysis"></a>

**Get the IOTGoat .img file.**

### Binwalk <a href="#binwalk" id="binwalk"></a>

```bash
# Scan to identify code, files and other information
binwalk IOTGoat.img

# Recursively extract the firmware and decompress the file
binwalk -reM IOTGoat.img
```

_Squashfs filesystem, little endian, version 4.0, compression:xz_

```bash
# extract the .img file and save it as a .bin file 
dd if=IoTGoat-raspberry-pi2.img bs=1 skip=29360128 of=iotgoat.bin
```

**.bin analysis :**

![](<../../.gitbook/assets/image (135).png>)

It is a squashfs file system so we can use **unsquashfs**, which will allow us to unpack the squashfs file system:

![](<../../.gitbook/assets/image (92).png>)

### 🐞 Vulnerabilities <a href="#lady_beetle-vulnerabilities" id="lady_beetle-vulnerabilities"></a>

#### ⚔️ No 1. Weak, Guessable or Harcoded Passwords <a href="#crossed_swords-no-1-weak-guessable-or-harcoded-passwords" id="crossed_swords-no-1-weak-guessable-or-harcoded-passwords"></a>

> Use of easily found, publicly available, or unmodifiable credentials, including backdoors in firmware or client software that allow unauthorized access to deployed systems.

**Firmwalker**

{% hint style="info" %}
This tool allows us to search the extracted firmware file system for **juicy elements** (_passwords, keys, info leak, etc_).
{% endhint %}

```bash
./firmwalker.sh ../firmware/_IoTGoat.img.extracted/squashfs-root/ ./IoTGoat.txt
```

Excerpt :

![Info leaks](<../../.gitbook/assets/image (21).png>)

```bash
# in the squashfs folder of the unpacked firmware
cat etc/shadow

# output :
root:$1$Jl7H1VOG$Wgw2F/C.nLNTC.4pwDa4H1:18145:0:99999:7:::
daemon:*:0:0:99999:7:::
ftp:*:0:0:99999:7:::
network:*:0:0:99999:7:::
nobody:*:0:0:99999:7:::
dnsmasq:x:0:0:99999:7:::
dnsmasq:x:0:0:99999:7:::
iotgoatuser:$1$79bz0K8z$Ii6Q/if83F1QodGmkb4Ah.:18145:0:99999:7:::
```

We have 2 password hashes: root and a user **iotgoatuser**.

#### **Bruteforce SSH**

With the user found, we try a **bruteforce** attack on the ssh service (port 22).\
We use the **mirai-botnet** wordlist of **SecLists**

```bash
# We want to filter only on the passwords because the list is of form user:password
awk '{print $2}' /usr/share/SecLists/Passwords/Malware/mirai-botnet.txt > /usr/share/SecLists/Passwords/Malware/mirai_pass.txt

# Bruteforce with Hydra
hydra -f -t 4 -l iotgoatuser -P /usr/share/SecLists/Passwords/Common-Credentials/mirai_pass.txt ssh://137.74.253.251
```

> **Credentials :**\
> iotgoatuser:7ujMko0vizxv

#### ⚔️ No 6: Insufficient Privacy Protection <a href="#crossed_swords-no-6-insufficient-privacy-protection" id="crossed_swords-no-6-insufficient-privacy-protection"></a>

> User's personal information stored on the device or in the ecosystem that is used in an insecure, inappropriate or unauthorized manner.

Firmwalker found a database containing personal information, unsecured since it was stored locally:

![](<../../.gitbook/assets/image (33).png>)

This allowed us to extract unsecured sensitive information :\\

![](<../../.gitbook/assets/image (73).png>)

## ⬛ Dynamic Analysis (1) <a href="#dynamic-analysis-black-box-black_large_square" id="dynamic-analysis-black-box-black_large_square"></a>

### 🐞 Vulnerabilities & Exploits <a href="#lady_beetle-vulnerabilities-exploits" id="lady_beetle-vulnerabilities-exploits"></a>

#### ⚔️ No 2. Insecure Network Services <a href="#crossed_swords-no-2-insecure-network-services" id="crossed_swords-no-2-insecure-network-services"></a>

> Unnecessary or unsecured network services running on the device itself, especially those exposed to the Internet, can compromise the confidentiality, integrity/authenticity, or availability of information or allow an attacker to gain unauthorized remote control.

1. **Exposed services**

```bash
❯ nmap -A -Pn 192.168.197.132

PORT    STATE SERVICE        VERSION
22/tcp  open  ssh            
25/tcp  open  smtp			 
53/tcp  open  domain		
80/tcp  open  http			
110/tcp open  pop3			 
119/tcp open  nntp
143/tcp open  imap
443/tcp open  https
465/tcp open  smtps
563/tcp open  snews
587/tcp open  submission
993/tcp open  imaps
995/tcp open  pop3s
5000/tcp  open   upnp		 
5515/tcp  open   unknown
65534/tcp open	 unknown
```

Due to a lack of restriction in network filtering, some services are exposed on the Internet. This can potentially allow an attacker to identify vulnerabilities in services that are often more vulnerable.

**2. MiniUPnP 2.1**\
The MiniUPnP version is vulnerable to these exploits:

* **Use after free** vulnerability (_CVE-2019-12106_)
* **Information disclosure** vulnerability (_CVE-2019-12107_)
* Multiple **DoS** vulnerabilities due to NULL pointer dereferences (_CVE-2019-12108, CVE-2019-12109, CVE-2019-12110, CVE-2019-12111_)

**3. dnsmasq 2.73**

![](<../../.gitbook/assets/image (110).png>)

The version of dnsmasq is outdated and vulnerable to 20 exploits:

![1](<../../.gitbook/assets/image (137) (1) (1) (2).png>)

![2](<../../.gitbook/assets/image (87).png>)

Some **PoC** are available here :

{% embed url="https://github.com/google/security-research-pocs/tree/master/vulnerabilities/dnsmasq" %}

{% hint style="warning" %}
**Unfortunately, we were unable to test these exploits because the virtual network interfaces had problems accessing IoTGoat and running the UPnP and Dnsmasq exploits.**
{% endhint %}

**4. DropBear 2017.75-7.1**\\

![](<../../.gitbook/assets/image (29).png>)

This version is **vulnerable to 4 exploits** :

![](<../../.gitbook/assets/image (61).png>)

![](<../../.gitbook/assets/image (128).png>)

![](<../../.gitbook/assets/image (55).png>)

![](<../../.gitbook/assets/image (68).png>)

Moreover, a vulnerability is present in the configuration file: the **possibility to connect as root via ssh** is enabled:

![](<../../.gitbook/assets/image (88).png>)

#### ⚔️ No 7: Insecure Data Transfer <a href="#crossed_swords-no-7-insecure-data-transfer" id="crossed_swords-no-7-insecure-data-transfer"></a>

> Lack of encryption or access control of sensitive data.

We use the **testssl.sh** tool, which allows us to check the service of a server on any port for support of TLS/SSL encryptions, protocols as well as recent cryptographic flaws and more.

The cipher suites used (_CBC_) by the web service are **obsolete** :

![](<../../.gitbook/assets/image (22).png>)

Also:

* The certificate does not match the URI provided,
* Certificate is **self-signed** (null trust chain)
* No CRL or OCSP URI provided

![](<../../.gitbook/assets/image (77).png>)

Moreover, the **HSTS** header is not implemented on the Web service and no security header is implemented:

![](<../../.gitbook/assets/image (130).png>)

The web service is potentially vulnerable to **Lucky13** :

![](<../../.gitbook/assets/image (115).png>)

**2.** Finally, running an OWASP ZAP scan on the web service :

![](<../../.gitbook/assets/image (139).png>)

* The absence of the **X-Frame-Options** header
  * Could lead to a **ClickJacking** attack.
* Absence of **Anti-CSRF** token
  * Could lead to a **CSRF** attack via one of the forms.

Finally, **port 25 is open** on the machine, allowing the use of **telnet**, a non-secure communication protocol.

**🚪 Root BackDoor - PoC**

After having decrypted the password of the **iotgoatuser** user, we connect to the machine with ssh.\
Many manual tests have been done to try to **escalate privileges** and become root (_cf._ [_https://hackerbible.gitbook.io/en/pentest-linux/privilege-escalation/manual-checks_](https://hackerbible.gitbook.io/en/pentest-linux/privilege-escalation/manual-checks)).\
For example, we have run **Linpeas** on the machine in order to try to find points of privilege escalation attempts:

![](<../../.gitbook/assets/image (144).png>)

Linpeas allowed us to list the active ports on the machine, this allowed us to see 2 interesting ports open on the machine: **5515** and **65534**.

The port **65534** is open on the machine, we try to connect to it with netcat :

![](<../../.gitbook/assets/image (80).png>)

We were not able to crack the root password, so this backdoor is not useful because we can only connect as _iotgoatuser_.

Also, the port **5515** is open. We try to connect to it with netcat :

![](<../../.gitbook/assets/image (69).png>)

This reverse shell gives us access as a **root** so we can **change its password** in order to access the web interface:

![](<../../.gitbook/assets/image (54).png>)

> \*Since this is an OpenWRT router, the SSH password and the root web interface password are the same.

## ⬜ Dynamic Analysis (2) <a href="#dynamic-analysis-white-box-white_large_square" id="dynamic-analysis-white-box-white_large_square"></a>

### 🔎 OSINT <a href="#mag_right-osint" id="mag_right-osint"></a>

New root password on the web interface: **password**

**Website Tree** (after authentication) :

![](<../../.gitbook/assets/image (83).png>)

### 🐞 Vulnerabilities & Exploits <a href="#lady_beetle-vulnerabilities-exploits-2" id="lady_beetle-vulnerabilities-exploits-2"></a>

#### ⚔️ No 5. Use of Insecure or Outdated Components <a href="#crossed_swords-no-5-use-of-insecure-or-outdated-components" id="crossed_swords-no-5-use-of-insecure-or-outdated-components"></a>

> Use of outdated or insecure software components/libraries that could allow the device to be compromised. This includes insecure customization of operating system platforms and the use of third-party software or hardware components from a compromised supply chain.

**1. Busybox 1.28.4**

![](<../../.gitbook/assets/image (105).png>)

This version of Busybox is potentially vulnerable to 14 exploits (_cf._ [_https://cyber.vumetric.com/vulns/busybox/busybox/1-28-4/_](https://cyber.vumetric.com/vulns/busybox/busybox/1-28-4/))

**2. Linux Kernel 4.14.95**

![](<../../.gitbook/assets/image (51).png>)

This version of Kernel **is** from **2017**. Thus, it is potentially vulnerable to 25 exploit (_cf._ [_https://www.security-database.com/cpe.php?detail=cpe%3A2.3%3Ao%3Alinux%3Alinux\_kernel%3A4.14.95%3A\*%3A\*%3A\*%3A\*%3A\*%3A\*%3A\*_](https://www.security-database.com/cpe.php?detail=cpe%3A2.3%3Ao%3Alinux%3Alinux\_kernel%3A4.14.95%3A%2A%3A%2A%3A%2A%3A%2A%3A%2A%3A%2A%3A%2A))

**3. pppd version 2.4.7**

![](<../../.gitbook/assets/image (145).png>)

This version of pppd is vulnerable to a denial of service and arbitrary code execution attack(_cf._ [_https://www.cyberveille-sante.gouv.fr/cyberveille/1646_](https://www.cyberveille-sante.gouv.fr/cyberveille/1646)).

#### ⚔️ No 4. Lack of Secure Update Mecanism <a href="#crossed_swords-no-4-lack-of-secure-update-mecanism" id="crossed_swords-no-4-lack-of-secure-update-mecanism"></a>

> Lack of security updates. This includes lack of firmware validation on the device, lack of secure encryption, lack of anti-rollback mechanisms, and lack of notifications of security changes due to updates.

* **OpenWRT Version :**\\

![](<../../.gitbook/assets/image (11) (1) (1) (3).png>)

This version is **vulnerable** to **21 exploits** :\\

![](<../../.gitbook/assets/image (67).png>)

![](<../../.gitbook/assets/image (45).png>)

**CVE-2020-7982 - PoC**

{% embed url="https://forallsecure.com/blog/uncovering-openwrt-remote-code-execution-cve-2020-7982" %}

#### ⚔️ No 3. Insecure Ecosystem Interfaces <a href="#crossed_swords-no-3-insecure-ecosystem-interfaces" id="crossed_swords-no-3-insecure-ecosystem-interfaces"></a>

> Unsecured web interfaces, APIs, mobile devices in the ecosystem, can allow the device or its related components to be compromised. The most common issues are lack of authentication/authorization, lack of or weak encryption, and lack of input and output filtering.

**CVE-2019-18992 - PoC**

> OpenWrt 18.06.2 is vulnerable to a **stored XSS attack** via these fields at the URI _/cgi-bin/luci/admin/network/firewall/rules_: "**Open ports on router**", "**New forward rule**" and "**New Source NAT**".

An XSS payload has been inserted in _/cgi-bin/luci/admin/network/firewall/rules_, in the **New Forward Rule** field.

![](<../../.gitbook/assets/image (125).png>)

Then we click on _Edit_ to trigger the XSS :\\

![](<../../.gitbook/assets/image (126).png>)

This XSS is also present in the **New Forward Rule** and **New Source Nat** fields, as well as in **Traffic Rules Name**.

**CVE-2019-18993 - PoC**

> OpenWrt 18.06.4 is vulnerable to **XSS attack stored** via the "New port forward" field at the URI /cgi-bin/luci/admin/network/firewall/forwards

This XSS payload has been inserted in _/cgi-bin/luci/admin/network/firewall/forwards_, in the **New Port Forward** field:

```html
<script>alert("HACKED!");</script>
```

![](<../../.gitbook/assets/image (7).png>)

By clicking on _Edit_, we trigger the XSS :\\

![](<../../.gitbook/assets/image (57).png>)

**CVE-2019-25015 - PoC**

> LuCI in OpenWrt versions **18.06.0 to 18.06.4** contains a **XSS vulnerability stored** via a modified SSID.

An XSS payload was inserted in _/cgi-bin/luci/admin/network/wireless/wl0.network1_ in the **ESSID** field.

![](<../../.gitbook/assets/image (132).png>)

Then we click on **Save & Apply,** to trigger the XSS :

![](<../../.gitbook/assets/image (113).png>)

#### Lack of Anti Bruteforce Mechanism

We were able to test a brute force attack on the web application folders with the BurpSuite tool. This one does not implement any anti-bruteforce mechanism.\\

![](<../../.gitbook/assets/image (52).png>)

**Command Execution**

There is a page _cgi-bin/luci/admin/iotgoat_ on the web service :\\

![](<../../.gitbook/assets/image (15).png>)

At the root of **/iotgoat**, we find this _hidden_ page:\\

![](<../../.gitbook/assets/image (109).png>)

This is a **Command Execution** vulnerability, allowing us to access the ash shell as **root** :\\

![](<../../.gitbook/assets/image (32).png>)

From there, an attacker might be able to **take full control** of the machine.

**DOS - CVE-2019-19945 - PoC**

> uhttpd in OpenWrt versions up to 18.06.5 and 19.x up to 19.07.0-rc2 has an integer signature error. This leads to an out-of-bounds access to a heap buffer and a subsequent crash. It can be triggered by an HTTP POST request to a CGI script, specifying both "Transfer-Encoding: chunked" and a large negative Content-Length value.

{% embed url="https://github.com/mclab-hbrs/openwrt-dos-poc" %}

#### ⚔️ No 8. Lack of Device Management <a href="#crossed_swords-no-8-lack-of-device-management" id="crossed_swords-no-8-lack-of-device-management"></a>

> Lack of security support on production-deployed devices, including asset management, update management, secure decommissioning, system monitoring and response capabilities.

Logs are not enabled :

![](<../../.gitbook/assets/image (8).png>)

In addition, OpenWRT packages are **not updated by default.**

#### ⚔️ No 9: Insecure Default Settings <a href="#crossed_swords-no-9-insecure-default-settings" id="crossed_swords-no-9-insecure-default-settings"></a>

> Devices or systems are shipped with unsecured default settings or lack the ability to make the system more secure by preventing operators from changing configurations.

* Using the default **root** user to log in to the web interface
* UPnp enabled by default and without _secure mode_

![](<../../.gitbook/assets/image (70).png>)
