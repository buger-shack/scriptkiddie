# 〰 Hash Cracking Tools

## `Hashcat`

{% embed url="https://hashcat.net/wiki/doku.php?id=example_hashes" %}
Hashes examples list
{% endembed %}

### Installation

```bash
# usually installed by default on kali/parrot
apt install hashcat
```

### Usage

```bash
# List of hashes
hashcat --example-hashes | grep -i md5 # to see various types of hash examples
```

#### Examples

```bash
# Decrypt md5 hashes
hashcat -m 0 -a 0 -O hashes.txt /usr/share/wordlists/rockyou.txt

# Decrypt Kerberos5 hashes
hashcat -m 18200 -a 0 -O hashes.txt /usr/share/wordlists/rockyou.txt

# Decrypt Django (PBKDF2-SHA256) hash
hashcat -m 10000 -a 0 -O hash.txt --wordlist /usr/share/wordlists/rockyou.txt
```

{% hint style="info" %}
**Rule based attacks** :boom:****

It is more like a **programming language** designed for password candidate generation. It has functions that can modify and mutate any given word list with literally anything you can imagine allowing you to have a higher rate of successly cracking a hash.

For more information : [https://hashcat.net/wiki/doku.php?id=rule\_based\_attack](https://hashcat.net/wiki/doku.php?id=rule\_based\_attack)
{% endhint %}

You can locate a handful of hashcat rules available by default on Kali Linux in : **/usr/share/hashcat/rules**

The following is a list of rules that can be used if the password is not present in a word list chosen. **Note** : This list is sorted by its complexity, meaning the bottom rule will rule the rest of the rules :wink:&#x20;

* `best64.rule`
* `rockyou-30000.rule`
* `dive.rule`
* `OneRuleToRuleThemAll`

```bash
# Decrypt NTLM hashes with hashcat rule best64.rule
hashcat -m 1000 -a 0 -O hashes.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
```

## `John the Ripper`

### Installation

```bash
git clone https://github.com/openwall/john.git
cd john/src
./configure && make
# use
cd ../run
./john
```

### Usage

```bash
# Cracking MD5 hash
john hash.txt --format=RAW-MD5

# Cracking SHA1 hash
john hash.txt /usr/share/wordlists/rockyou.txt --format=RAW-SHA1

# Cracking Linux passwords
sudo john /etc/shadow
```
