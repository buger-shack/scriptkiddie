# Firefox

## Firefox Decrypt

> Extract passwords from Mozilla

{% embed url="https://github.com/unode/firefox_decrypt" %}

### Install

```bash
git clone https://github.com/unode/firefox_decrypt
cd firefox_decrypt/
```

### Usage

```bash
python firefox_decrypt.py [PATH = Path of the firefox extracted profile]
```

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption><p>example</p></figcaption></figure>

```bash
# Advanced usage

python firefox_decrypt.py --list
python firefox_decrypt.py /folder/containing/profiles.ini/
python firefox_decrypt.py | grep -C2 keyword
```

## firepwd

> decrypt Mozilla protected passwords&#x20;

{% embed url="https://github.com/lclevy/firepwd" %}

```bash
# install 
git clone https://github.com/lclevy/firepwd.git
cd firepwd
pip3 install -r requirements.txt

# must have key4.db or key3.db & logins.json
# usage
python3 firepwd.py
```
