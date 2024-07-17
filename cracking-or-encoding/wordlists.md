# 📝 Wordlists

![IMG](https://files.oaiusercontent.com/file-VRbKwQo7vUACMMbdFlAiH6Mv?se=2023-12-01T15%3A35%3A24Z\&sp=r\&sv=2021-08-06\&sr=b\&rscc=max-age%3D31536000%2C%20immutable\&rscd=attachment%3B%20filename%3D45351aba-f0ce-49a0-b0ab-b2d69cefc751.webp\&sig=NJNvlZ%2Baibyczc4TyeM%2BufElVEMwbo/W0firNaKYj/c%3D)

### All

{% embed url="https://github.com/gmelodie/awesome-wordlists" %}

### Usernames

{% embed url="https://github.com/jeanphorn/wordlist/blob/master/usernames.txt" %}

```bash
# Get usernames wordlist
wget https://raw.githubusercontent.com/jeanphorn/wordlist/master/usernames.txt -o /usr/share/wordlists/usernames.txt
```

### Passwords

{% embed url="https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.csv" %}
Default credentials
{% endembed %}

{% embed url="https://github.com/berandal666/Passwords" %}

[SecLists](https://github.com/danielmiessler/SecLists)

```bash
# Install SecLists
git clone https://github.com/danielmiessler/SecLists
sudo apt install seclists -y
```

### Personalized

{% embed url="https://github.com/Mebus/cupp" %}

Generate passwords based on your knowledge of the victim (names, dates...)

```bash
python3 cupp.py -h
```
