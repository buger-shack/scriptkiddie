# Google

<figure><img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExMWU3MmVjNjAyMWZlNzJhZWM4MWNiNjkzYTVlZTM2MmZiZTlhY2JlNSZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/3orifgceWrEx980fxC/giphy.gif" alt="" width="375"><figcaption></figcaption></figure>

{% hint style="info" %}
Google hacking, also named **Google Dorking**, is a hacker technique that uses Google Search and other Google applications to find **security holes** in the **configuration** and computer code that websites are using. Google dorking could also be used for **OSINT**.
{% endhint %}

{% hint style="warning" %}
_**Disclaimer:** It is highly advised that you use the information you find for legal purposes only. The unauthorized access of information can lead to a criminal proceeding against you. So use Google hacking with care (and written permission)._
{% endhint %}



## Tools

### metagoofil



{% embed url="https://github.com/opsdisk/metagoofil" %}

```bash
# install
git clone https://github.com/opsdisk/metagoofil
cd metagoofil
pip install -r requirements.txt

# usage
python3 metagoofil.py -d domain.com -t doc -l 50 -n 50 -o /tmp/result -f /tmp/result/result.html -u
```

## Base

**Links**

{% embed url="https://sector035.nl/articles/keeping-a-grip-on-google-ids" %}

{% embed url="https://dorks.faisalahmed.me/" %}

{% embed url="https://www.exploit-db.com/google-hacking-database" %}

| **Operator**   | **Description**                                                                 | **Example**                 |
| -------------- | ------------------------------------------------------------------------------- | --------------------------- |
| **intitle:**   | which finds strings in the title of a page                                      | intitle:”Your Text”         |
| **allintext:** | which finds all terms in the title of a page                                    | allintext:”Contact”         |
| **inurl:**     | which finds strings in the URL of a page                                        | inurl:”news.php?id=”        |
| **site:**      | which restricts a search to a particular site or domain                         | site:yeahhub.com “Keyword”  |
| **filetype:**  | which finds specific types of files (doc, pdf, mp3 etc) based on file extension | filetype:pdf “Cryptography” |
| **link:**      | which searches for all links to a site or URL                                   | link:”example.com”          |
| **cache:**     | which displays Google’s cached copy of a page                                   | cache:yeahhub.com           |
| **info:**      | which displays summary information about a page                                 | info:www.example.com        |

## Sensitive Directories

```
intitle:" index of "/Invoices*"
intitle:"index of" ".env"
intitle:"index of" "/configs"
```

## Vulnerable Websites

```
intitle:"index of" "*.php"
intitle:"index of" "*.py"
intitle:"index of" "*.sh"
intitle:"index of" "schema.sql"
inurl: database

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

### Juicy Files

```
inurl:admin filetype:xls
intitle:"index of" "/mysql"
site:.edu intext:"index of" "payroll"
inurl:edu “login”

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
ext:mdb   inurl:*.mdb inurl:fpdb shop.mdb
ext:mdb inurl:*.mdb inurl:fpdb shop.mdb
ext:mdb inurl:*.mdb inurl:fpdb shop.mdb
filetype:SWF SWF
filetype:TXT TXT
filetype:XLS XLS
filetype:asp   DBQ=" * Server.MapPath("*.mdb")
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
intext:"Generated   by phpSystem"
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
intext:"Network Vulnerability   Assessment Report"
intext:"Network Vulnerability Assessment Report"
intext:"Network Vulnerability Assessment Report" 本文来自 pc007.com
intext:"SQL Server Driver][SQL Server]Line 1: Incorrect syntax near"
intext:"Thank you for your order"   +receipt
intext:"Thank you for your order" +receipt
intext:"Thank you for your purchase" +download
intext:"The following report contains confidential information" vulnerability -search
intext:"phpMyAdmin MySQL-Dump" "INSERT INTO" -"the"
intext:"phpMyAdmin MySQL-Dump" filetype:txt
intext:"phpMyAdmin" "running on" inurl:"main.php"
intextpassword | passcode)   intextusername | userid | user) filetype:csv
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

## Database

{% embed url="https://www.boxpiper.com/posts/google-dork-list" %}
Check this
{% endembed %}

{% embed url="https://www.ma-no.org/en/security/google-dorks-find-interesting-data-search-like-hacker" %}

{% embed url="https://www.exploit-db.com/google-hacking-database" %}
