# 🥸 Dorks

## Working with Dorks

### Online Services:

* [Google Hacking Database](https://www.exploit-db.com/google-hacking-database) — A continuously expanding catalog of dorks with an integrated search function.
* [Dorksearch](https://dorksearch.com/) — A search engine featuring a built-in dork builder.
* [Bug Bounty Helper](https://dorks.faisalahmed.me/) — An online Google dorks builder focused on discovering sensitive pages..
* [https://habr.com/ru/companies/postuf/articles/510766/](https://habr.com/ru/companies/postuf/articles/510766/) — Google Dorking Usage

### Apps

* [pagodo](https://github.com/opsdisk/pagodo) — Automates the search for potentially vulnerable web pages using dorks from the Google Hacking Database.
* [Grawler](https://github.com/A3h1nt/Grawler) — A web-based PHP utility for automating Google Dorks usage, cleaning, and saving search results.
* [DorkScout](https://github.com/R4yGM/dorkscout) — Another tool for automating dork searches, written in Golang.
* [oxDork](https://github.com/rly0nheart/oxdork) — A utility for identifying vulnerabilities and misconfigurations in web servers.
* [ATSCAN SCANNER](https://github.com/AlisamTechnology/ATSCAN) — Designed for dork-based searches and mass scanning of web resources for vulnerabilities.
* [Fast Google Dorks Scan](https://github.com/IvanGlinkin/Fast-Google-Dorks-Scan) — An automated tool for gathering information about a specific website using dorks.
* [SiteDorks](https://github.com/Zarcolio/sitedorks) — A premade collection of search queries for Google, Bing, Ecosia, DuckDuckGo, Yandex, Yahoo, and more, comprising 527 websites.

### Google

​

<figure><img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExMWU3MmVjNjAyMWZlNzJhZWM4MWNiNjkzYTVlZTM2MmZiZTlhY2JlNSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/3orifgceWrEx980fxC/giphy.gif" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Google hacking, also named **Google Dorking**, is a hacker technique that uses Google Search and other Google applications to find **security holes** in the **configuration** and computer code that websites are using. Google dorking could also be used for **OSINT**.&#x20;
{% endhint %}

{% hint style="warning" %}
_**Disclaimer:** It is highly advised that you use the information you find for legal purposes only. The unauthorized access of information can lead to a criminal proceeding against you. So use Google hacking with care (and written permission)._
{% endhint %}

### Tools

#### metagoofil

{% embed url="https://github.com/opsdisk/metagoofil" %}

```
# install
git clone https://github.com/opsdisk/metagoofil
cd metagoofil
pip install -r requirements.txt
​
# usage
python3 metagoofil.py -d domain.com -t doc -l 50 -n 50 -o /tmp/result -f /tmp/result/result.html -u
```

### Base

**Links**

{% embed url="https://sector035.nl/articles/keeping-a-grip-on-google-ids" %}

{% embed url="https://dorks.faisalahmed.me/" %}

{% embed url="https://www.exploit-db.com/google-hacking-database" %}

| **OPERATOR**   | **DESCRIPTION**                                                                 | **EXAMPLE**                                     |
| -------------- | ------------------------------------------------------------------------------- | ----------------------------------------------- |
| **intitle:**   | which finds strings in the title of a page                                      | intitle:”Your Text”                             |
| **allintext:** | which finds all terms in the title of a page                                    | allintext:”Contact”                             |
| **inurl:**     | which finds strings in the URL of a page                                        | inurl:”news.php?id=”                            |
| **site:**      | which restricts a search to a particular site or domain                         | site:yeahhub.com “Keyword”                      |
| **filetype:**  | which finds specific types of files (doc, pdf, mp3 etc) based on file extension | filetype:pdf “Cryptography”                     |
| **link:**      | which searches for all links to a site or URL                                   | link:”example.com”                              |
| **cache:**     | which displays Google’s cached copy of a page                                   | cache:yeahhub.com                               |
| **info:**      | which displays summary information about a page                                 | info:[www.example.com](https://www.example.com) |

### Sensitive Directories

```
intitle:" index of "/Invoices*"
intitle:"index of" ".env"
intitle:"index of" "/configs"
```

### Vulnerable Websites

```
intitle:"index of" "*.php"
intitle:"index of" "*.py"
intitle:"index of" "*.sh"
intitle:"index of" "schema.sql"
inurl: database
​
inurl:php?=id1
inurl:index.php?id=
inurl:trainers.php?id=
inurl:buy.php?category=
inurl:article.php?ID=
inurl:play_old.php?id=
inurl:declaration_more.php?decl_id=
inurl:pageid=
inurl:games.php?id=
inurl:page.php?file=
inurl:newsDetail.php?id=
inurl:gallery.php?id=
inurl:article.php?id=
inurl:show.php?id=
inurl:staff_id=
inurl:newsitem.php?num= andinurl:index.php?id=
inurl:trainers.php?id=
inurl:buy.php?category=
inurl:article.php?ID=
inurl:play_old.php?id=
inurl:declaration_more.php?decl_id=
inurl:pageid=
inurl:games.php?id=
inurl:page.php?file=
inurl:newsDetail.php?id=
inurl:gallery.php?id=
inurl:article.php?id=
inurl:show.php?id=
inurl:staff_id=
inurl:newsitem.php?num=
```

#### Juicy Files

```
inurl:admin filetype:xls
intitle:"index of" "/mysql"
site:.edu intext:"index of" "payroll"
inurl:edu “login”
​
intext:”budget approved”) inurl:confidential
ext:inc "pwd=" "UID="
ext:ini intext:env.ini
ext:ini Version=... password
ext:ini Version=4.0.0.4 password
ext:ini eudora.ini
ext:ini intext:env.ini
ext:log "Software: Microsoft Internet Information Services *.*"
ext:log "Software: Microsoft Internet Information
ext:log "Software: Microsoft Internet Information Services *.*"
ext:log "Software: Microsoft Internet Information Services *.*"
ext:mdb   inurl:*.mdb inurl:fpdb shop.mdb
ext:mdb inurl:*.mdb inurl:fpdb shop.mdb
ext:mdb inurl:*.mdb inurl:fpdb shop.mdb
filetype:SWF SWF
filetype:TXT TXT
filetype:XLS XLS
filetype:asp   DBQ=" * Server.MapPath("*.mdb")
filetype:asp "Custom Error Message" Category Source
filetype:asp + "[ODBC SQL"
filetype:asp DBQ=" * Server.MapPath("*.mdb")
filetype:asp DBQ=" * Server.MapPath("*.mdb") 
filetype:asp “Custom Error Message” Category Source
filetype:bak createobject sa
filetype:bak inurl:"htaccess|passwd|shadow|htusers"
filetype:bak inurl:"htaccess|passwd|shadow|htusers" 
filetype:conf inurl:firewall -intitle:cvs 
filetype:conf inurl:proftpd. PROFTP FTP server configuration file reveals
filetype:dat "password.dat
filetype:dat "password.dat" 
filetype:eml eml +intext:"Subject" +intext:"From" +intext:"To"
filetype:eml eml +intext:"Subject" +intext:"From" +intext:"To" 
filetype:eml eml +intext:”Subject” +intext:”From” +intext:”To”
filetype:inc dbconn 
filetype:inc intext:mysql_connect
filetype:inc mysql_connect OR mysql_pconnect 
filetype:log inurl:"password.log"
filetype:log username putty PUTTY SSH client logs can reveal usernames
filetype:log “PHP Parse error” | “PHP Warning” | “PHP Error”
filetype:mdb inurl:users.mdb
filetype:ora ora
filetype:ora tnsnames
filetype:pass pass intext:userid
filetype:pdf "Assessment Report" nessus
filetype:pem intext:private
filetype:properties inurl:db intext:password
filetype:pst inurl:"outlook.pst"
filetype:pst pst -from -to -date
filetype:reg reg +intext:"defaultusername" +intext:"defaultpassword"
filetype:reg reg +intext:"defaultusername" +intext:"defaultpassword" 
filetype:reg reg +intext:â? WINVNC3â?
filetype:reg reg +intext:”defaultusername” +intext:”defaultpassword”
filetype:reg reg HKEY_ Windows Registry exports can reveal
filetype:reg reg HKEY_CURRENT_USER SSHHOSTKEYS
filetype:sql "insert into" (pass|passwd|password)
filetype:sql ("values * MD5" | "values * password" | "values * encrypt")
filetype:sql ("passwd values" | "password values" | "pass values" ) 
filetype:sql ("values * MD" | "values * password" | "values * encrypt") 
filetype:sql +"IDENTIFIED BY" -cvs
filetype:sql password
filetype:sql password 
filetype:sql “insert into” (pass|passwd|password)
filetype:url +inurl:"ftp://" +inurl:";@"
filetype:url +inurl:"ftp://" +inurl:";@" 
filetype:url +inurl:”ftp://” +inurl:”;@”
filetype:xls inurl:"email.xls"
filetype:xls username password email
index of: intext:Gallery in Configuration mode
index.of passlist
index.of perform.ini mIRC IRC ini file can list IRC usernames and
index.of.dcim 
index.of.password 
intext:" -FrontPage-" ext:pwd inurl:(service | authors | administrators | users)
intext:""BiTBOARD v2.0" BiTSHiFTERS Bulletin Board"
intext:"# -FrontPage-" ext:pwd inurl:(service | authors | administrators | users) "# -FrontPage-" inurl:service.pwd
intext:"#mysql dump" filetype:sql
intext:"#mysql dump" filetype:sql 21232f297a57a5a743894a0e4a801fc3
intext:"A syntax error has occurred" filetype:ihtml
intext:"ASP.NET_SessionId" "data source="
intext:"About Mac OS Personal Web Sharing"
intext:"An illegal character has been found in the statement" -"previous message"
intext:"AutoCreate=TRUE password=*"
intext:"Can't connect to local" intitle:warning
intext:"Certificate Practice Statement" filetype:PDF | DOC
intext:"Certificate Practice Statement" inurl:(PDF | DOC)
intext:"Copyright (c) Tektronix, Inc." "printer status"
intext:"Copyright © Tektronix, Inc." "printer status"
intext:"Emergisoft web applications are a part of our"
intext:"Error Diagnostic Information" intitle:"Error Occurred While"
intext:"Error Message : Error loading required libraries."
intext:"Establishing a secure Integrated Lights Out session with" OR intitle:"Data Frame - Browser not HTTP 1.1 compatible" OR intitle:"HP Integrated Lights-
intext:"Fatal error: Call to undefined function" -reply -the -next
intext:"Fill out the form below completely to change your password and user name. If new username is left blank, your old one will be assumed." -edu
intext:"Generated   by phpSystem"
intext:"Generated by phpSystem"
intext:"Host Vulnerability Summary Report"
intext:"HostingAccelerator" intitle:"login" +"Username" -"news" -demo
intext:"IMail Server Web Messaging" intitle:login
intext:"Incorrect syntax near"
intext:"Index of" /"chat/logs"
intext:"Index of /network" "last modified"
intext:"Index of /" +.htaccess
intext:"Index of /" +passwd
intext:"Index of /" +password.txt
intext:"Index of /admin"
intext:"Index of /backup"
intext:"Index of /mail"
intext:"Index of /password"
intext:"Microsoft (R) Windows * (TM) Version * DrWtsn32 Copyright (C)" ext:log
intext:"Microsoft CRM : Unsupported Browser Version"
intext:"Microsoft ® Windows * ™ Version * DrWtsn32 Copyright ©" ext:log
intext:"Network Host Assessment Report" "Internet Scanner"
intext:"Network Vulnerability   Assessment Report"
intext:"Network Vulnerability Assessment Report"
intext:"Network Vulnerability Assessment Report" 本文来自 pc007.com
intext:"SQL Server Driver][SQL Server]Line 1: Incorrect syntax near"
intext:"Thank you for your order"   +receipt
intext:"Thank you for your order" +receipt
intext:"Thank you for your purchase" +download
intext:"The following report contains confidential information" vulnerability -search
intext:"phpMyAdmin MySQL-Dump" "INSERT INTO" -"the"
intext:"phpMyAdmin MySQL-Dump" filetype:txt
intext:"phpMyAdmin" "running on" inurl:"main.php"
intextpassword | passcode)   intextusername | userid | user) filetype:csv
intextpassword | passcode) intextusername | userid | user) filetype:csv
intitle:"index of" +myd size
intitle:"index of" etc/shadow
intitle:"index of" htpasswd
intitle:"index of" intext:connect.inc
intitle:"index of" intext:globals.inc
intitle:"index of" master.passwd
intitle:"index of" master.passwd 007电脑资讯
intitle:"index of" members OR accounts
intitle:"index of" mysql.conf OR mysql_config
intitle:"index of" passwd
intitle:"index of" people.lst
intitle:"index of" pwd.db
intitle:"index of" spwd
intitle:"index of" user_carts OR user_cart
intitle:"index.of *" admin news.asp configview.asp
intitle:("TrackerCam Live Video")|("TrackerCam Application Login")|("Trackercam Remote") -trackercam.com
intitle:(“TrackerCam Live Video”)|(“TrackerCam Application Login”)|(“Trackercam Remote”) -trackercam.com
inurl:admin inurl:userlist Generic userlist files
```

### Database

{% embed url="https://www.boxpiper.com/posts/google-dork-list" %}

{% embed url="https://www.ma-no.org/en/security/google-dorks-find-interesting-data-search-like-hacker" %}

### Shodan

#### Resources

{% embed url="https://github.com/jakejarvis/awesome-shodan-queries" %}

#### Basic

`port:` Search by specific port

`net:` Search based on an IP/CIDR

`hostname:` Locate devices by hostname

`os:` Search by Operating System

`city:` Locate devices by city

`country:` Locate devices by country

`geo:` Locate devices by coordinates

`org:` Search by organization

`before/after:` Timeframe delimiter

`hash:` Search based on banner hash

`has_screenshot:true` Filter search based on a screenshot being present

`title:` Search based on text within the title

`asn:` Search ASN e.g. 'AS12345'

`ssl.jarm:` Search by JARM fingerprint

### Examples

**net:**

Find devices based on an IP address or /x CIDR. `net:210.214.0.0/16`

**Organization**

```
org:microsoft 
org:"United States Department"
```

**Autonomous System Number (ASN)**

`asn:ASxxxx`

**os:**

Find devices based on operating system. `os:"windows 7"`

**port:**

Find devices based on open ports. `proftpd port:21`

**before/after:**

Find devices before or after between a given time. `apache after:22/02/2009 before:14/3/2010`

**SSL/TLS Certificates**

Self signed certificates `ssl.cert.issuer.cn:example.com ssl.cert.subject.cn:example.com`

Expired certificates `ssl.cert.expired:true`

`ssl.cert.subject.cn:example.com`

**Device Type**

```
device:firewall 
device:router 
device:wap 
device:webcam 
device:media 
device:"broadband router" 
device:pbx 
device:printer 
device:switch 
device:storage 
device:specialized 
device:phone 
device:"voip" 
device:"voip phone" 
device:"voip adaptor" 
device:"load balancer" 
device:"print server" 
device:terminal 
device:remote 
device:telecom 
device:power 
device:proxy 
device:pda 
device:bridge
```

**Operating System**

```
os:"windows 7" 
os:"windows server 2012"
os:"linux 3.x"
```

**Product**

```
product:apache 
product:nginx 
product:android 
product:chromecast
```

**Customer Premises Equipment (CPE)**

```
cpe:apple 
cpe:microsoft 
cpe:nginx 
cpe:cisco
```

**Server**

```
server: nginx 
server: apache 
server: microsoft 
server: cisco-ios
```

**ssh fingerprints**

`dc:14:de:8e:d7:c1:15:43:23:82:25:81:d2:59:e8:c0`

#### Dorks

**Pulse Secure**

`http.html:/dana-na`

**PEM Certificates**

`http.title:"Index of /" http.html:".pem"`

### Databases

**MySQL**

`"product:MySQL"`

**MongoDB**

`"product:MongoDB"` `mongodb port:27017`

**Fully open MongoDBs**

`"MongoDB Server Information { "metrics":"` `"Set-Cookie: mongo-express=" "200 OK"`

**Kibana dashboards without authentication**

`kibana content-legth:217`

**elastic**

`port:9200 json` `port:"9200" all:elastic`

**Memcached**

`"product:Memcached"`

**CouchDB**

`"product:CouchDB"` `port:"5984"+Server: "CouchDB/2.1.0"`

**PostgreSQL**

`"port:5432 PostgreSQL"`

**Riak**

`"port:8087 Riak"`

**Redis**

`"product:Redis"`

**Cassandra**

`"product:Cassandra"`

#### Industrial Control Systems

**Samsung Electronic Billboards**

`"Server: Prismview Player"`

**Gas Station Pump Controllers**

`"in-tank inventory" port:10001`

**Fuel Pumps connected to internet:**

No auth required to access CLI terminal.\ `"privileged command" GET`

**Automatic License Plate Readers**

`P372 "ANPR enabled"`

**Traffic Light Controllers / Red Light Cameras**

`mikrotik streetlight`

**Voting Machines in the United States**

"voter system serial" country:US

**Open ATM:**

May allow for ATM Access availability `NCR Port:"161"`

**Telcos Running Cisco Lawful Intercept Wiretaps**

`"Cisco IOS" "ADVIPSERVICESK9_LI-M"`

**Prison Pay Phones**

`"[2J[H Encartele Confidential"`

**Tesla PowerPack Charging Status**

`http.title:"Tesla PowerPack System" http.component:"d3" -ga3ca4f2`

**Electric Vehicle Chargers**

`"Server: gSOAP/2.8" "Content-Length: 583"`

**Maritime Satellites**

Shodan made a pretty sweet Ship Tracker that maps ship locations in real time, too!

`"Cobham SATCOM" OR ("Sailor" "VSAT")`

**Submarine Mission Control Dashboards**

`title:"Slocum Fleet Mission Control"`

**CAREL PlantVisor Refrigeration Units**

`"Server: CarelDataServer" "200 Document follows"`

**Nordex Wind Turbine Farms**

`http.title:"Nordex Control" "Windows 2000 5.0 x86" "Jetty/3.1 (JSP 1.1; Servlet 2.2; java 1.6.0_14)"`

**C4 Max Commercial Vehicle GPS Trackers**

`"[1m[35mWelcome on console"`

**DICOM Medical X-Ray Machines**

Secured by default, thankfully, but these 1,700+ machines still have no business being on the internet.

`"DICOM Server Response" port:104`

**GaugeTech Electricity Meters**

`"Server: EIG Embedded Web Server" "200 Document follows"`

**Siemens Industrial Automation**

`"Siemens, SIMATIC" port:161`

**Siemens HVAC Controllers**

`"Server: Microsoft-WinCE" "Content-Length: 12581"`

**Door / Lock Access Controllers**

`"HID VertX" port:4070`

**Railroad Management**

`"log off" "select the appropriate"`

**Tesla Powerpack charging Status:**

Helps to find the charging status of tesla powerpack. `http.title:"Tesla PowerPack System" http.component:"d3" -ga3ca4f2`

**XZERES Wind Turbine**

`title:"xzeres wind"`

**PIPS Automated License Plate Reader**

`"html:"PIPS Technology ALPR Processors""`

**Modbus**

`"port:502"`

**Niagara Fox**

`"port:1911,4911 product:Niagara"`

**GE-SRTP**

`"port:18245,18246 product:"general electric""`

**MELSEC-Q**

`"port:5006,5007 product:mitsubishi"`

**CODESYS**

`"port:2455 operating system"`

**S7**

`"port:102"`

**BACnet**

`"port:47808"`

**HART-IP**

`"port:5094 hart-ip"`

**Omron FINS**

`"port:9600 response code"`

**IEC 60870-5-104**

`"port:2404 asdu address"`

**DNP3**

`"port:20000 source address"`

**EtherNet/IP**

`"port:44818"`

**PCWorx**

`"port:1962 PLC"`

**Crimson v3.0**

`"port:789 product:"Red Lion Controls"`

**ProConOS**

`"port:20547 PLC"`

### Remote Desktop

**Unprotected VNC**

`"authentication disabled" port:5900,5901` `"authentication disabled" "RFB 003.008"`

**Windows RDP**

99.99% are secured by a secondary Windows login screen.

`"\x03\x00\x00\x0b\x06\xd0\x00\x00\x124\x00"`

#### Network Infrastructure

**CobaltStrike Servers**

`product:"cobalt strike team server"` `ssl.cert.serial:146473198` - default certificate serial number `ssl.jarm:07d14d16d21d21d07c42d41d00041d24a458a375eef0c576d23a7bab9a9fb1`

**Hacked routers:**

Routers which got compromised\ `hacked-router-help-sos`

**Redis open instances**

`product:"Redis key-value store"`

**Citrix:**

Find Citrix Gateway.\ `title:"citrix gateway"`

**Weave Scope Dashboards**

Command-line access inside Kubernetes pods and Docker containers, and real-time visualization/monitoring of the entire infrastructure.

`title:"Weave Scope" http.favicon.hash:567176827`

**MongoDB**

Older versions were insecure by default. Very scary.

`"MongoDB Server Information" port:27017 -authentication`

**Mongo Express Web GUI**

Like the infamous phpMyAdmin but for MongoDB.

`"Set-Cookie: mongo-express=" "200 OK"`

**Jenkins CI**

`"X-Jenkins" "Set-Cookie: JSESSIONID" http.title:"Dashboard"`

**Jenkins:**

Jenkins Unrestricted Dashboard `x-jenkins 200`

**Docker APIs**

`"Docker Containers:" port:2375`

**Docker Private Registries**

`"Docker-Distribution-Api-Version: registry" "200 OK" -gitlab`

**Pi-hole Open DNS Servers**

`"dnsmasq-pi-hole" "Recursion: enabled"`

**Already Logged-In as root via Telnet**

`"root@" port:23 -login -password -name -Session`

**Telnet Access:**

NO password required for telnet access.\ `port:23 console gateway`

**Polycom video-conference system no-auth shell**

`"polycom command shell"`

**NPort serial-to-eth / MoCA devices without password**

`nport -keyin port:23`

**Android Root Bridges**

A tangential result of Google's sloppy fractured update approach. 🙄 More information here.

`"Android Debug Bridge" "Device" port:5555`

**Lantronix Serial-to-Ethernet Adapter Leaking Telnet Passwords**

`Lantronix password port:30718 -secured`

**Citrix Virtual Apps**

`"Citrix Applications:" port:1604`

**Cisco Smart Install**

Vulnerable (kind of "by design," but especially when exposed).

`"smart install client active"`

**PBX IP Phone Gateways**

`PBX "gateway console" -password port:23`

**Polycom Video Conferencing**

`http.title:"- Polycom" "Server: lighttpd"` `"Polycom Command Shell" -failed port:23`

**Telnet Configuration:**

`"Polycom Command Shell" -failed port:23`

Example: Polycom Video Conferencing

**Bomgar Help Desk Portal**

`"Server: Bomgar" "200 OK"`

**Intel Active Management CVE-2017-5689**

`"Intel(R) Active Management Technology" port:623,664,16992,16993,16994,16995` `”Active Management Technology”`

**HP iLO 4 CVE-2017-12542**

`HP-ILO-4 !"HP-ILO-4/2.53" !"HP-ILO-4/2.54" !"HP-ILO-4/2.55" !"HP-ILO-4/2.60" !"HP-ILO-4/2.61" !"HP-ILO-4/2.62" !"HP-iLO-4/2.70" port:1900`

**Lantronix ethernet adapter’s admin interface without password**

`"Press Enter for Setup Mode port:9999"`

**Wifi Passwords:**

Helps to find the cleartext wifi passwords in Shodan. `html:"def_wirelesspassword"`

**Misconfigured Wordpress Sites:**

The wp-config.php if accessed can give out the database credentials. `http.html:"* The wp-config.php creation script uses this file"`

#### Outlook Web Access:

**Exchange 2007**

`"x-owa-version" "IE=EmulateIE7" "Server: Microsoft-IIS/7.0"`

**Exchange 2010**

`"x-owa-version" "IE=EmulateIE7" http.favicon.hash:442749392`

**Exchange 2013 / 2016**

`"X-AspNet-Version" http.title:"Outlook" -"x-owa-version"`

**Lync / Skype for Business**

`"X-MS-Server-Fqdn"`

#### Network Attached Storage (NAS)

**SMB (Samba) File Shares**

Produces \~500,000 results...narrow down by adding "Documents" or "Videos", etc.

`"Authentication: disabled" port:445`

**Specifically domain controllers:**

`"Authentication: disabled" NETLOGON SYSVOL -unix port:445`

**Concerning default network shares of QuickBooks files:**

`"Authentication: disabled" "Shared this folder to access QuickBooks files OverNetwork" -unix port:445`

**FTP Servers with Anonymous Login**

`"220" "230 Login successful." port:21`

**Iomega / LenovoEMC NAS Drives**

`"Set-Cookie: iomega=" -"manage/login.html" -http.title:"Log In"`

**Buffalo TeraStation NAS Drives**

`Redirecting sencha port:9000`

**Logitech Media Servers**

`"Server: Logitech Media Server" "200 OK"`

Example: Logitech Media Servers

**Plex Media Servers**

`"X-Plex-Protocol" "200 OK" port:32400`

**Tautulli / PlexPy Dashboards**

`"CherryPy/5.1.0" "/home"`

**Home router attached USB**

`"IPC$ all storage devices"`

#### Webcams

**Generic camera search**

`title:camera`

**Webcams with screenshots**

`webcam has_screenshot:true`

**D-Link webcams**

`"d-Link Internet Camera, 200 OK"`

**Hipcam**

`"Hipcam RealServer/V1.0"`

**Yawcams**

`"Server: yawcam" "Mime-Type: text/html"`

**webcamXP/webcam7**

`("webcam 7" OR "webcamXP") http.component:"mootools" -401`

**Android IP Webcam Server**

`"Server: IP Webcam Server" "200 OK"`

**Security DVRs**

`html:"DVR_H264 ActiveX"`

**Surveillance Cams:**

With username:admin and password: :P\ `NETSurveillance uc-httpd` `Server: uc-httpd 1.0.0`

#### Printers & Copiers:

**HP Printers**

`"Serial Number:" "Built:" "Server: HP HTTP"`

**Xerox Copiers/Printers**

`ssl:"Xerox Generic Root"`

**Epson Printers**

`"SERVER: EPSON_Linux UPnP" "200 OK"`

`"Server: EPSON-HTTP" "200 OK"`

**Canon Printers**

`"Server: KS_HTTP" "200 OK"`

`"Server: CANON HTTP Server"`

#### Home Devices

**Yamaha Stereos**

`"Server: AV_Receiver" "HTTP/1.1 406"`

**Apple AirPlay Receivers**

Apple TVs, HomePods, etc.

`"\x08_airplay" port:5353`

**Chromecasts / Smart TVs**

`"Chromecast:" port:8008`

**Crestron Smart Home Controllers**

`"Model: PYNG-HUB"`

#### Random Stuff

**Calibre libraries**

`"Server: calibre" http.status:200 http.title:calibre`

**OctoPrint 3D Printer Controllers**

`title:"OctoPrint" -title:"Login" http.favicon.hash:1307375944`

**Ethereum Miners**

`"ETH - Total speed"`

**Apache Directory Listings**

Substitute .pem with any extension or a filename like phpinfo.php.

`http.title:"Index of /" http.html:".pem"`

**Misconfigured WordPress**

Exposed wp-config.php files containing database credentials.

`http.html:"* The wp-config.php creation script uses this file"`

**Too Many Minecraft Servers**

`"Minecraft Server" "protocol 340" port:25565`

**Literally Everything in North Korea**

`net:175.45.176.0/22,210.52.109.0/24,77.94.35.0/24`

## Twitter / GitHub

{% embed url="https://github.com/techgaun/github-dorks" %}

{% embed url="https://github.com/igorbrigadir/twitter-advanced-search" %}
