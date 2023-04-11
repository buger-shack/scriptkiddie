# Online Anonymity

## Complete Guide

{% embed url="https://anonfiles.com/Bf32x3Vayc/guide_md" %}
Guide to online anonymity
{% endembed %}

## Being Anonymous on the Internet

### Prerequisites

* **VMware** Workstation
* A **Windows 7** ISO
* A **VPN** (I recommend **AirVPN**)
* A **DNS fix**

### Tutorial

{% hint style="info" %}
Change your MAC address, many softwares exist.

* Download **VMware Worskation** (preferably the Pro version), the latest version. **Install it on your computer.**
* Create a new VM, with the **Windows 7 iso**.
* Open your VM and install all the necessary software: _Firefox_, _Ccleaner_, _Tor browser_, _Your VPN_, etc...
* Open Firefox, type "_about:config_" and change the following setting: _media.peerconnection.enabled:_ change to "**false**"
* List itemDownload this DNS fix :&#x20;

[http://www.partage-facile.com/XE3JB81LAS/fixdns.zip.html](http://www.partage-facile.com/XE3JB81LAS/fixdns.zip.html)&#x20;
{% endhint %}

:point\_right: You will now make a snapshot of your VM, you will call it "_Initial state_".

#### Procedure BEFORE each connection

{% hint style="success" %}
* Re-initialize your VM to the initial state.
* Execute the file "_1.vbs_" (Fix DNS)
* Execute your VPN.
* Execute the "_2.vbs_" file
* Go to this [link ](https://www.dnsleaktest.com/)and click on "**Extended test**".
  * <mark style="background-color:green;">If only the IP of your VPN appears, all is good. If not, try again.</mark>
* Go to this [link ](http://check2ip.com/)to check if your **IP is blacklisted**.
  * <mark style="background-color:red;">If it is, you may have problems. Change the IP in your VPN.</mark>
  * <mark style="background-color:green;">If it is not, everything is fine.</mark>
{% endhint %}

{% hint style="info" %}
:tada:**YOU CAN NOW SURF WITHOUT RISK ! YOU ARE ANONYMOUS !**&#x20;

**Of course, total anonymity does not exist. Always be careful.**
{% endhint %}

#### Procedure AFTER each connection

{% hint style="success" %}
* Disconnect your VPN.
* Run the "_3.vbs_" file
* Re-initialize your VM to the initial state.
{% endhint %}

## Testing your anonymity

**Here are some tools to test your anonymity on the internet (WebRTC vulnerability and others):**&#x20;

{% embed url="https://privacy.net/analyzer" %}
&#x20;Various: fingerprint, autofill leaks, info about your browser etc.
{% endembed %}

{% embed url="https://ipv6leak.com" %}
IPv6 leak
{% endembed %}

{% embed url="https://dnsleaktest.com" %}
DNS leak
{% endembed %}

{% embed url="https://www.expressvpn.com/fr/webrtc-leak-test" %}
IP leak with the WebRTC flaw
{% endembed %}

{% embed url="https://ipx.ac/run" %}
IP, geolocation, IP leak with WebRTC
{% endembed %}

## Add-Ons

{% embed url="https://www.ghostery.com" %}

{% embed url="https://www.eff.org/https-everywhere" %}

{% embed url="https://addons.mozilla.org/en-US/firefox/addon/noscript" %}

{% embed url="https://addons.mozilla.org/en-US/firefox/addon/privacy-settings" %}

{% embed url="http://anonymous-proxy-servers.net/en/jondofox.html" %}

{% embed url="https://www.privacytools.io" %}

## Extreme Privacy - What It Takes to Disappear

{% embed url="https://inteltechniques.com/book7.html" %}
Extreme Privacy - Michael Bazzell&#x20;
{% endembed %}

{% embed url="https://www.amazon.fr/dp/B094LDWKGZ?dchild=1&keywords=extreme+privacy&language=en_US&linkCode=gs2&linkId=449c8b8fe32e9d67997625713aa70242&ref_=as_li_ss_tl&sr=8-3&tag=inteltechni0a-21" %}
Buy the Book on Amazon - USA
{% endembed %}

{% embed url="https://inteltechniques.com/data/workbook.pdf" %}
Extreme Privacy - File
{% endembed %}
