# ⏩ SAP

{% embed url="https://github.com/shipcod3/mySapAdventures" %}
Methodology
{% endembed %}

### Discovery

<pre class="language-bash"><code class="lang-bash">wfuzz -w /usr/share/SecLists/Discovery/Web-Content/URLs/urls-SAP.txt --hc 404,403,503,406,401 --hl 172 https://domain.com/FUZZ
wfuzz -w /usr/share/SecLists/Discovery/Web-Content/CMS/SAP.fuzz.txt --hc 404,403,503,406,401 --hl 172 https://domain.com/FUZZ
<strong>wfuzz -w /usr/share/SecLists/Discovery/Web-Content/sap.txt --hc 404,403,503,406,401 --hl 172 https://domain.com/FUZZ
</strong><strong>
</strong><strong># good wordlist
</strong><strong>wget https://gist.githubusercontent.com/0x240x23elu/88327494cf7331008a13bc7d5aabfe74/raw/62bed611cfef054ffbb9b8bd0a320a53671d9ee4/SAPwordlists.txt -o sap_great.txt
</strong><strong>wfuzz -w sap_great.txt --hc 404,403,503,406,401 --hl 172 https://domain.comFUZZ
</strong>
# check juicy
http://domain.com/sap/public/info
</code></pre>

#### Default Passwords

|    USER    |      PASSWORD      |      CLIENT      |
| :--------: | :----------------: | :--------------: |
|    SAP\*   |   06071992, PASS   | 001, 066, Custom |
|    DDIC    |      19920706      | 000, 001, Custom |
|   TMSADM   | PASSWORD, $1Pawd2& |        000       |
|   SAPCPIC  |        ADMIN       |      000,001     |
| EARLYWATCH |       SUPPORT      |        066       |

### Exploits

#### SAP\_RECON (CVE-2020-6287, CVE-2020-6286)

> This script allows to check SAP LM Configuration Wizard missing authorization check vulnerability and as a PoC script **exploits directory traversal** in queryProtocol method.&#x20;
>
> **Directory traversal allows to download any zip from SAP server.**

{% embed url="https://github.com/chipik/SAP_RECON" %}

```bash
# install
git clone https://github.com/chipik/SAP_RECON.git 
cd SAP_RECON/

# usage
python RECON.py -h
```
