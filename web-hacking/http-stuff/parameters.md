# HTTP Parameters

## Find Parameters

### Arjun

> Find hidden HTTP parameters

```bash
# usage / install
pip3 install arjun
arjun --help
arjun -u $target_url
```

### ParamSpider

> Parameter miner for humans

```bash
git clone https://github.com/devanshbatham/ParamSpider
cd ParamSpider
pip3 install -r requirements.txt
python3 paramspider.py --domain $domain
```

## Parameter Pollution

{% hint style="info" %}
When you manipulate any parameter, it’s manipulation depends on how each web technology is parsing their parameters.

You can identify web technologies using “[Wappalyzer](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/)”.

Below is the screenshot of some technologies and their parameter parsing.
{% endhint %}

![](<../../.gitbook/assets/image (131).png>)

Unicode char can cause breaks in some applications. Example with the pile of poo 💩 :

{% embed url="https://emojipedia.org/pile-of-poo/" %}
pile of poo emoji
{% endembed %}

> Understand it :

{% embed url="https://shahjerry33.medium.com/http-parameter-pollution-its-contaminated-85edc0805654" %}
