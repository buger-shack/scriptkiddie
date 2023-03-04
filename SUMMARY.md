# Table of contents

* [➡ /home/red-squad](README.md)
  * [☑ Recently Added](readme/pipline.md)
  * [🍻 Support our projects](readme/support-our-projects.md)

## 🌐 Web Hacking

* [🦶 Initial Foothold](web-hacking/initial-foothold/README.md)
  * [Exposition Audit - Plan](web-hacking/initial-foothold/exposition-audit-plan.md)
  * [Internal Audit - Plan](web-hacking/initial-foothold/internal-audit-plan.md)
  * [External Audit - Plan](web-hacking/initial-foothold/external-audit-plan.md)
* [🚩 CTFs shortcuts](web-hacking/ctfs-shortcuts.md)
* [➕ Enumeration](web-hacking/enumeration/README.md)
  * [🖥 Network Scanners](web-hacking/enumeration/network-scanners.md)
  * [🗃 Directory/Files Scanners](web-hacking/enumeration/directory-files-scanners.md)
  * [🌎 Web Scanners](web-hacking/enumeration/web-scanners.md)
* [🌎 HTTP Stuff](web-hacking/http-stuff/README.md)
  * [🛣 HTTP Methods](web-hacking/http-stuff/http-methods/README.md)
    * [⛔ 403 Bypass](web-hacking/http-stuff/http-methods/403-bypass.md)
  * [📄 Security Headers](web-hacking/http-stuff/security-headers.md)
  * [🍪 Cookies](web-hacking/http-stuff/cookies.md)
  * [⭐ JWT](web-hacking/http-stuff/jwt/README.md)
    * [Attacking JWT](web-hacking/http-stuff/jwt/attacking-jwt.md)
  * [⚒ Parameters](web-hacking/http-stuff/parameters.md)
* [💉 Injections](web-hacking/injections/README.md)
  * [HTML | XSS](web-hacking/injections/html-or-xss.md)
  * [SQLi](web-hacking/injections/sqli/README.md)
    * [SQLmap](web-hacking/injections/sqli/sqlmap.md)
    * [NoSQLi](web-hacking/injections/sqli/nosqli.md)
  * [XXE](web-hacking/injections/xxe.md)
* [🕸 Web Vulnerabilities](web-hacking/web-vulnerabilities/README.md)
  * [👆 CSRF](web-hacking/web-vulnerabilities/csrf.md)
  * [🖱 ClickJacking](web-hacking/web-vulnerabilities/clickjacking.md)
  * [📁 Files / Upload](web-hacking/web-vulnerabilities/files-upload/README.md)
    * [🗃 File Upload Bypass](web-hacking/web-vulnerabilities/files-upload/file-upload-bypass.md)
    * [📦 ZIP Slip](web-hacking/web-vulnerabilities/files-upload/zip-slip.md)
  * [💡 Insecure Direct Object Reference (IDOR)](web-hacking/web-vulnerabilities/insecure-direct-object-reference-idor.md)
  * [📂 LFI](web-hacking/web-vulnerabilities/lfi/README.md)
    * [Files to look for](web-hacking/web-vulnerabilities/lfi/files-to-look-for.md)
  * [🌡 Remote Code Execution](web-hacking/web-vulnerabilities/remote-code-execution.md)
* [🕶 Dorking](web-hacking/dorking/README.md)
  * [Google](web-hacking/dorking/google.md)
  * [Shodan](web-hacking/dorking/shodan.md)
* [🧱 WAF Bypass](web-hacking/waf-bypass.md)
* [🏛 Servers / CMS](web-hacking/servers-cms/README.md)
  * [🐦 Apache](web-hacking/servers-cms/apache.md)
  * [🔷 WordPress](web-hacking/servers-cms/wordpress.md)
  * [⏩ SAP](web-hacking/servers-cms/sap.md)
  * [🕴 Jenkins](web-hacking/servers-cms/jenkins.md)
  * [🏢 Server-Side Vulnerabilities](web-hacking/servers-cms/server-side-vulnerabilities/README.md)
    * [Server-Side Request Forgery](web-hacking/servers-cms/server-side-vulnerabilities/server-side-request-forgery.md)
    * [Server-Side Template Injection](web-hacking/servers-cms/server-side-vulnerabilities/server-side-template-injection.md)
* [⚙ API](web-hacking/api/README.md)
  * [GraphQL](web-hacking/api/graphql.md)

## 🐧 Linux Hacking

* [🚪 Backdoors](linux-hacking/backdoors.md)
* [👁 Compiled Binaries](linux-hacking/compiled-binaries.md)
* [🪜 Privilege Escalation](linux-hacking/privilege-escalation/README.md)
  * [Manual Checks](linux-hacking/privilege-escalation/manual-checks.md)
  * [Automated Checks](linux-hacking/privilege-escalation/automated-checks.md)
* [⭕ Reverse Shells](linux-hacking/reverse-shells/README.md)
  * [Shell Stabilizing](linux-hacking/reverse-shells/shell-stabilizing.md)
  * [PwnCat](linux-hacking/reverse-shells/pwncat.md)
  * [🏓 Ping-Pong](linux-hacking/reverse-shells/ping-pong.md)
* [📦 Buffer Overflow](linux-hacking/buffer-overflow-bof.md)
* [👣 Cover tracks](linux-hacking/cover-tracks.md)

## 🪟 Windows Hacking

* [🔓 AppLocker](windows-hacking/applocker.md)
* [🔏 BitLocker](windows-hacking/bitlocker.md)
* [🪜 Privilege Escalation](windows-hacking/privilege-escalation.md)
* [👥 Active Directory](windows-hacking/active-directory/README.md)
  * [1. Reconnaissance](windows-hacking/active-directory/1.-reconnaissance/README.md)
    * [First Steps](windows-hacking/active-directory/1.-reconnaissance/first-steps.md)
    * [LDAPSearch](windows-hacking/active-directory/1.-reconnaissance/ldapsearch.md)
    * [SMB Enumeration](windows-hacking/active-directory/1.-reconnaissance/smb-enumeration.md)
  * [2. Initial Attack Vectors](windows-hacking/active-directory/2.-initial-attack-vectors/README.md)
    * [Windows Secrets](windows-hacking/active-directory/2.-initial-attack-vectors/windows-secrets.md)
    * [PrivEsc to Domain Admin](windows-hacking/active-directory/2.-initial-attack-vectors/privesc-to-domain-admin.md)
    * [Lookupsid.py](windows-hacking/active-directory/2.-initial-attack-vectors/lookupsid.py.md)
    * [ASREPRoast](windows-hacking/active-directory/2.-initial-attack-vectors/asreproast.md)
    * [Kerbrute](windows-hacking/active-directory/2.-initial-attack-vectors/kerbrute.md)
    * [LLMNR\_NBT NS Poisoning](windows-hacking/active-directory/2.-initial-attack-vectors/llmnr\_nbt-ns-poisoning.md)
    * [PowerView.ps1](windows-hacking/active-directory/2.-initial-attack-vectors/powerview.ps1.md)
    * [IPv6 Attacks](windows-hacking/active-directory/2.-initial-attack-vectors/ipv6-attacks.md)
    * [Relay Poisoning Ressources](windows-hacking/active-directory/2.-initial-attack-vectors/relay-poisoning-ressources.md)
    * [PrivEsc to Domain Admin](windows-hacking/active-directory/2.-initial-attack-vectors/privesc-to-domain-admin-1.md)
  * [3. Post-Compromise Enumeration](windows-hacking/active-directory/3.-post-compromise-enumeration/README.md)
    * [PowerView](windows-hacking/active-directory/3.-post-compromise-enumeration/powerview.md)
    * [BloodHound](windows-hacking/active-directory/3.-post-compromise-enumeration/bloodhound.md)
    * [MimiKatz](windows-hacking/active-directory/3.-post-compromise-enumeration/mimikatz.md)
    * [PingCastle](windows-hacking/active-directory/3.-post-compromise-enumeration/pingcastle.md)
  * [4. Post-Compromise Attacks](windows-hacking/active-directory/4.-post-compromise-attacks/README.md)
    * [WSUS Poison](windows-hacking/active-directory/4.-post-compromise-attacks/wsus-poison.md)
    * [AlwaysInstallElevated](windows-hacking/active-directory/4.-post-compromise-attacks/alwaysinstallelevated.md)
    * [DCSync](windows-hacking/active-directory/4.-post-compromise-attacks/dcsync.md)
    * [Dumping LSASS](windows-hacking/active-directory/4.-post-compromise-attacks/dumping-lsass.md)
    * [Dumping NTDS.dit](windows-hacking/active-directory/4.-post-compromise-attacks/dumping-ntds.dit.md)
    * [Golden Tickets](windows-hacking/active-directory/4.-post-compromise-attacks/golden-tickets.md)
    * [GPP Attacks](windows-hacking/active-directory/4.-post-compromise-attacks/gpp-attacks.md)
    * [Kerberoasting - SPN](windows-hacking/active-directory/4.-post-compromise-attacks/kerberoasting-spn.md)
    * [Pass the Hash](windows-hacking/active-directory/4.-post-compromise-attacks/pass-the-hash.md)
    * [Pass the Password](windows-hacking/active-directory/4.-post-compromise-attacks/pass-the-password.md)
    * [Rubeus](windows-hacking/active-directory/4.-post-compromise-attacks/rubeus.md)
  * [5. PrivEsc & MISC](windows-hacking/active-directory/5.-privesc-and-misc/README.md)
    * [Automated scripts](windows-hacking/active-directory/5.-privesc-and-misc/automated-scripts.md)
    * [Exploits](windows-hacking/active-directory/5.-privesc-and-misc/exploits/README.md)
      * [noPac - CVE-2021-42278](windows-hacking/active-directory/5.-privesc-and-misc/exploits/nopac-cve-2021-42278.md)
      * [ZeroLogon - CVE-2020-1472](windows-hacking/active-directory/5.-privesc-and-misc/exploits/zerologon-cve-2020-1472.md)
      * [PrintNightMare](windows-hacking/active-directory/5.-privesc-and-misc/exploits/printnightmare.md)
      * [Other CVEs](windows-hacking/active-directory/5.-privesc-and-misc/exploits/other-cves.md)
* [🧱 Bypass AV](windows-hacking/bypass-av.md)

## 👷♀ 👷♀ 👷♀ Systems

* [➕ Services Enumeration](systems/services-enumeration.md)
* [🖨 Printers](systems/printers/README.md)
  * [Printer Exploitation Tool (PRET)](systems/printers/printer-exploitation-tool-pret.md)
  * [CUPS](systems/printers/cups.md)

## 🔑 CRACKING | ENCODING

* [⚙ Bruteforce tools](cracking-or-encoding/bruteforce-tools.md)
* [📜 Wordlists](cracking-or-encoding/wordlists.md)
* [〰 Hash Cracking Tools](cracking-or-encoding/hash-cracking-tools.md)
* [🕵 Encoding | Decoding Tools](cracking-or-encoding/encoding-or-decoding-tools.md)
* [🎨 Steganography | Cipher](cracking-or-encoding/steganography-or-cipher.md)

## 🎆 Network Pivoting | Tunneling

* [❓ What is Pivoting ?](network-pivoting-or-tunneling/what-is-pivoting.md)
* [⚙ Pivoting tools / guide](network-pivoting-or-tunneling/pivoting-tools/README.md)
  * [Proxychains / FoxyProxy](network-pivoting-or-tunneling/pivoting-tools/proxychains-foxyproxy.md)
  * [SSH Tunnelling / Port Forwarding](network-pivoting-or-tunneling/pivoting-tools/ssh-tunnelling-port-forwarding.md)
  * [Plinx.exe](network-pivoting-or-tunneling/pivoting-tools/plinx.exe.md)
  * [Socat](network-pivoting-or-tunneling/pivoting-tools/socat.md)
  * [Chisel](network-pivoting-or-tunneling/pivoting-tools/chisel.md)
  * [Sshuttle](network-pivoting-or-tunneling/pivoting-tools/sshuttle.md)
  * [Ligolo-Ng](network-pivoting-or-tunneling/pivoting-tools/ligolo-ng.md)

## 📱 Mobile Hacking

* [🤖 Android](mobile-hacking/android/README.md)
  * [Genymotion](mobile-hacking/android/genymotion.md)
* [🍏 iOS](mobile-hacking/ios.md)
* [📺 IOT](mobile-hacking/iot/README.md)
  * [IOTGoat OWASP | Walkthrough](mobile-hacking/iot/iotgoat-owasp-or-walkthrough.md)
  * [Resources](mobile-hacking/iot/resources.md)

## 📡 Wireless Hacking

* [⚡ Wi-Fi Attacks](wireless-hacking/wi-fi-attacks/README.md)
  * [👹 EvilTwin](wireless-hacking/wi-fi-attacks/eviltwin.md)
  * [⛓ Cracking WPA/WPA2](wireless-hacking/wi-fi-attacks/cracking-wpa-wpa2.md)
  * [👃 Sniffing](wireless-hacking/wi-fi-attacks/sniffing.md)
* [🫐 Bluetooth](wireless-hacking/bluetooth.md)

## 📕 Code Audit

* [✅ Best Practices](code-audit/best-practices.md)
* [❌ Bad Practices](code-audit/bad-practices.md)
* [⚙ Tools](code-audit/tools.md)

## 🐷 Thick Client Hacking

* [📜 Thick Client Pentesting Methodology](thick-client-hacking/thick-client-pentesting-methodology.md)
* [🥑 Resources](thick-client-hacking/resources.md)

## 💥 MISC

* [🧱 Firewalls](misc/firewalls/README.md)
  * [🔥 Evasion](misc/firewalls/evasion.md)
* [🔻 CVEs](misc/cves/README.md)
  * [\[CVE-2021-34527\] - PrintNighmare](misc/cves/cve-2021-34527-printnighmare.md)
  * [\[CVE-2022-0847\] - dirtypipe](misc/cves/cve-2022-0847-dirtypipe.md)
  * [\[CVE-2021-4034\] - Pwnkit](misc/cves/cve-2021-4034-pwnkit.md)
  * [\[CVE-2021-45105\] - Log4J](misc/cves/cve-2021-45105-log4j.md)
  * [\[CVE-2018-15473\] - OPENSSH < 7.7](misc/cves/cve-2018-15473-openssh-less-than-7.7.md)
* [🦊 Firefox Extensions](misc/firefox-extensions.md)
* [🍎 Free Labs](misc/free-labs.md)
* [🎩 OSINT](misc/osint.md)
* [🧞 Exploitation Frameworks](misc/exploitation-frameworks.md)
* [💬 Hacking Forums](misc/hacking-forums.md)
* [👹 Evil Side](misc/evil-side/README.md)
  * [💰 Ransomwares](misc/evil-side/ransomwares/README.md)
    * [🎨 Create A Ransomware](misc/evil-side/ransomwares/create-a-ransomware.md)
    * [📵 Disabling AntiMalware Tools](misc/evil-side/ransomwares/disabling-antimalware-tools.md)
  * [💣 DDoS](misc/evil-side/ddos/README.md)
    * [Tools](misc/evil-side/ddos/tools.md)

## 🕵 OPSEC

* [🧑 Finding Someone](opsec/finding-someone.md)
* [🔑 Privacy](opsec/privacy/README.md)
  * [Online Anonymity](opsec/privacy/online-anonymity.md)
  * [Browser Configuration](opsec/privacy/browser-configuration.md)

## 🔴 RED TEAM

* [🕵 Spy cam](red-team/spy-cam.md)
* [🗝 Lock Picking](red-team/lock-picking.md)
* [🎯 Scrapping](red-team/scrapping.md)
* [🎣 Phishing](red-team/phishing/README.md)
  * [ℹ Infrastructure](red-team/phishing/infrastructure.md)
  * [📚 Resources](red-team/phishing/resources.md)

## 🔵 BLUE TEAM

* [🐾 Forensics](blue-team/forensics.md)
* [🖲 Malware Analysis](blue-team/malware-analysis.md)
* [🔧 Tools](blue-team/tools.md)
* [🍯 HoneyPots](blue-team/honeypots.md)
* [🎆 Networks Security](blue-team/networks-security.md)
* [🔍 Online IoC Scanners](blue-team/online-ioc-scanners.md)

## 🖥 DEVELOPERS

* [🧾 IDE](developers/ide.md)

## 🧑🎓 🧑🎓 🧑🎓 LEARNING

* [Windows](learning/windows/README.md)
  * [Active Directory](learning/windows/active-directory.md)
  * [Kerberos](learning/windows/kerberos.md)
* [LFI | RFI](learning/lfi-or-rfi.md)
* [SQL](learning/sql/README.md)
  * [SQSHell | sqsh | skwish](learning/sql/sqshell-or-sqsh-or-skwish.md)
  * [NoSQL](learning/sql/nosql.md)
  * [DB infos](learning/sql/db-infos.md)
* [SSL/TLS](learning/ssl-tls/README.md)
  * [Configuration on MariaDB](learning/ssl-tls/configuration-on-mariadb.md)

## 🐛 Bug Bounty Related

* [🌡 Searching for CVEs](bug-bounty-related/searching-for-cves.md)
* [⚖ \[FR\] Laws](bug-bounty-related/fr-laws.md)
* [📦 Bug Bounty Stuff](bug-bounty-related/bug-bounty-stuff/README.md)
  * [Dorks](bug-bounty-related/bug-bounty-stuff/dorks.md)
