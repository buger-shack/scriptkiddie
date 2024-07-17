# 🎆 Wi-Fi Attacks

{% embed url="https://mmmds.pl/wifi-cheatsheet" %}

{% embed url="https://book.hacktricks.xyz/pentesting/pentesting-wifi" %}

## Attacks List

* **DoS**
  * Deauthentication/disassociation -- Disconnect everyone (or a specific ESSID/Client)
  * Random fake APs -- Hide nets, possible crash scanners
  * Overload AP -- Try to kill the AP (usually not very useful)
  * WIDS -- Play with the IDS
  * TKIP, EAPOL -- Some specific attacks to DoS some APs Cracking
* **Crack WEP** (several tools and methods)
* **WPA-PSK**
  * WPS pin "Brute-Force"
  * WPA PMKID bruteforce
  * \[DoS +] WPA handshake capture + Cracking
* **WPA-MGT**
  * Username capture
  * Bruteforce Credentials
* **Evil Twin** (with or without DoS)
  * Open Evil Twin \[+ DoS] -- Useful to capture captive portal creds and/or perform LAN attacks
  * WPA-PSK Evil Twin -- Useful to network attacks if you know the password
  * WPA-MGT -- Useful to capture company credentials
* **KARMA, MANA, Loud MANA, Known beacon**
  * Open -- Useful to capture captive portal creds and/or perform LAN attacks
  * WPA -- Useful to capture WPA handshakes

## Scan Wi-Fis

```bash
# scans for wifis
sudo iwlist wlan0 scanning
```

## Tools

### Airgeddon

{% embed url="https://github.com/v1s1t0r1sh3r3/airgeddon" %}

{% embed url="https://www.hackingarticles.in/wireless-penetration-testing-airgeddon/" %}
Tutorial
{% endembed %}
