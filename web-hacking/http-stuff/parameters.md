# ⚒ Parameters

## Arjun

{% embed url="https://github.com/s0md3v/Arjun" %}
Arjun repo
{% endembed %}

Sometimes hidden parameters are set on pages You can use tools like Arjun to find them

```bash
# usage / install
pip3 install arjun
arjun --help
arjun -u https://$target
```

## Parameters Pollution

{% hint style="info" %}
When you manipulate any parameter, it’s manipulation depends on how each web technology is parsing their parameters.&#x20;

You can identify web technologies using “[Wappalyzer](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer/)”.&#x20;

Below is the screenshot of some technologies and their parameter parsing.&#x20;
{% endhint %}

![](<../../.gitbook/assets/image (131).png>)

Unicode char can cause breaks in some applications. Example with the pile of poo 💩 :&#x20;

{% embed url="https://emojipedia.org/pile-of-poo/" %}
pile of poo emoji
{% endembed %}
