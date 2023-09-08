# ClickJacking



<details>

<summary>What is ClickJacking ?</summary>

**Clickjacking**, also known as a “_UI redress attack_”, is when an attacker uses multiple transparent or opaque **layers** to trick a user into **clicking** on a **button** or **link** on another page when they were intending to click on the top level page.

Thus, the attacker is “**hijacking**” clicks meant for their page and routing them to another page, most likely owned by another application, domain, or both.

</details>

![](https://media3.giphy.com/media/Hbc0vXRf7LsADdQKWo/giphy.gif?cid=ecf05e47seuf999xjb46v5bbx6wufxri4dxyi87abs58vsjp\&rid=giphy.gif\&ct=g)

## Check

* <mark style="color:red;">**No X-Frame-Options Header**</mark>
* <mark style="color:red;">**No Content Security Policy (**</mark><mark style="color:red;">with the</mark> _<mark style="color:red;">frame-ancestors</mark>_ <mark style="color:red;">directive</mark><mark style="color:red;">**)**</mark>

## PoC

### BurpSuite

{% embed url="https://portswigger.net/support/using-burp-to-find-clickjacking-vulnerabilities" %}

### Manual

```html
<!-- copy in a form field -->
<iframe src="http://www.google.com" width="250" height="250"></iframe>
```
