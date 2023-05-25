# Wordlists

![](https://media0.giphy.com/media/B7o99rIuystY4/giphy.gif?cid=ecf05e47cf3wyj24p0mb9x0ecz5kqfzxwwgade22vbck2cph\&rid=giphy.gif\&ct=g)

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
