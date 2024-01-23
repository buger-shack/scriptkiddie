# Static Analysis

{% embed url="https://github.com/bytedance/appshark" %}

## Read Source Code

**Check for :**

1. Cryptography: Look for any use of encryption algorithms and verify that they are implemented correctly. Check for any hardcoded keys, weak encryption methods, or use of insecure cryptographic algorithms.
2. Code Obfuscation: Check for obfuscation techniques used to hide the code and make it difficult to understand. Obfuscation techniques make it harder for reverse engineering, but it can also hide any malicious code.
3. API Usage: Verify that the application does not use any insecure APIs or APIs with known vulnerabilities. Look for any APIs that allow unauthorized access or data leakage.
4. Reflection: Check for reflection, a feature that allows dynamic code execution. Verify that reflection is not used in a way that can allow an attacker to inject malicious code.
5. Dynamic Code Loading: Check for dynamic code loading, a feature that allows an application to load code at runtime. Verify that the application does not load code from untrusted sources or execute any code that is not verified.
6. Access Control: Verify that the application has implemented proper access control to sensitive functionality and data. Look for any hardcoded credentials or access tokens.
7. Hardcoded sensitive information: Check for any insecure storage of sensitive data such as passwords, user credentials, or personal information. Search for harcoded database(SQL queries), password, keys, sensitive information,URLs.
8. External Libraries: Verify that the application does not use any insecure third-party libraries or libraries with known vulnerabilities.
9. Integrity Checks: Look for any integrity checks that the application performs to ensure that the code has not been tampered with.
10. Native Code: If the application uses native code, verify that the native code is compiled securely and does not contain any vulnerabilities.
11. Web view related checks:
    1. **`setJavaScriptEnabled()`**: This method enables or disables the use of JavaScript in a web view. If this method is set to true, the web view can execute JavaScript code, which can be used to manipulate the web page or to communicate with the native Android code. However, if the application does not properly validate the input data sent to the web view, it could allow an attacker to inject malicious JavaScript code.
    2. **`setAllowFileAccess()`**: This method allows or denies access to local files in the device file system from the web view. If this method is set to true, the web view can access local files, which can be useful for displaying local HTML files or accessing data stored in the device file system. However, if the application does not properly validate the input data sent to the web view, it could allow an attacker to access or modify local files.
    3. **`setAllowUniversalAccessFromFileURLs`** : if this option is set to true , it can allow attackers to load their files inside WebViews.
    4. **`addJavascriptInterface()`**: This method allows JavaScript code in the web view to access the native Android code by exposing a Java object to JavaScript. This feature can be used to provide additional functionality or to interact with the native Android code. However, if the application does not properly validate the input data sent to the web view, it could allow an attacker to execute arbitrary Java code.
    5. **`runtime.exec()`**: This method is used to execute shell commands on the device. If an attacker can inject malicious input data into a web view and exploit an application vulnerability, it could allow the attacker to execute arbitrary shell commands on the device.
    6. **`onReceivedSslError` :** it tells the WebView to ignore SSL errors and proceed with the connection. Can be used by attackers to read or completely change the content being displayed to users.
12. SQLite related checks :&#x20;
    1. **openOrCreateDatabase()** : function, it's used by SQLCipher to store key to encrypt DB. `SQLiteDatabase database = SQLiteDatabase. **openOrCreateDatabase** (databaseFile, "test123", null);`
13. **Proxified activities** :&#x20;
    1. &#x20;**`(((android:targetActivity)))=".someactivity"`**
14. **TapJacking** ;&#x20;
    1. **`filterTouchesWhenObscured`** to find if vulnerable to tapjacking or not.
15. Root Detection Implementation details
16. SSL Pinning Implementation details
17. Exported Preference Activitie
18. Apps which enable backups
19. Apps which are debuggable
20. App Permissions.
21. Firebase Instance(s) / AWS&#x20;

## **Files :**

### **AndroidManifest.xml**

> The Android Manifest is an XML file which contains **important** metadata about the Android app. This includes the package name, activity names, main activity (the entry point to the app), Android version support, hardware features support, permissions, and other configurations.

**Check for :**&#x20;

* **Permissions**: Check if the application requests any sensitive permissions like camera, microphone, location, SMS, or call logs. If the app is requesting unnecessary permissions, it could be a red flag for privacy violations or potential security risks.
* **Components**: Android components like activities, services, receivers, and providers can be exploited by attackers to gain unauthorized access or to launch attacks. Check if any of the components are exposed to other applications or if they are **exported** with overly permissive access.\
  **`android:exported`:** The default value of the attribute is true. (should be set to false)
* **Intents**: Intents are messages used by different Android components to communicate with each other. They can be used to launch activities, services, or broadcast messages. Check if the app is using any implicit intents that could be intercepted or manipulated by attackers.
* **`Allow debugable: true`** — Without a rooted phone it is possible to extract the data or run an arbitrary code using application permission (Should be false). The default value is “false”.
* **`Allow backup: true`** — The default value of this attribute is true. This setting defines whether application data can be backed up and restored by a user who has enabled usb debugging.(Should be false)
* **Application information:** Check if the application has any hard-coded credentials, sensitive information, or debugging features that could be exploited by attackers.
* **Malware signatures**: Check if the application has any malware signatures that could indicate that the app is malicious or potentially harmful.
* **Target SDK version**: Check if the app is targeting an older version of the Android SDK. If the app is not targeting the latest version, it could be vulnerable to known security vulnerabilities.

**NOTE: All the permissions that the application requests should be reviewed to ensure that they don’t introduce a security risk.**

### **res/values/strings.xml**

> This file stores the string resources for the application. It’s a good place to look for hardcoded secrets and info leaks.

### Files **.db** or **.sqlite**

> Look for these files to see what information is shipped along with the application. This is an easy source of secrets and info leaks.

## Information Gathering / Enumeration

### Tools

{% tabs %}
{% tab title="apkinspector" %}
> Find permissions and dependencies used by an Android application.&#x20;

{% embed url="https://github.com/CameraKit/apk-inspector" %}

```bash
# install
git clone https://github.com/CameraKit/apk-inspector.git
cd apk-inspector
npm install -g .

# usage
apki -l ./Downloads/myapk.apk -c
```
{% endtab %}

{% tab title="apkid" %}
> Identifies many compilers, packers, obfuscators, etc. It's **PEiD** for Android.&#x20;

{% embed url="https://github.com/rednaga/APKiD" %}

```bash
# install
pip install apkid

# usage
apkid app.apk
```
{% endtab %}

{% tab title="apkenum" %}
> Passive Enumeration Utility For Android Applications.

{% embed url="https://github.com/shivsahni/APKEnum" %}

```bash
# install
git clone https://github.com/shivsahni/APKEnum.git
cd APKEnum

# usage
python2 APKEnum.py -p $apk_file
```
{% endtab %}

{% tab title="apkleaks" %}
> Scanning APK file for URIs, endpoints & secrets.&#x20;

{% embed url="https://github.com/dwisiswant0/apkleaks" %}

```bash
# install
git clone https://github.com/dwisiswant0/apkleaks
cd apkleaks/
pip3 install -r requirements.txt

# usage
python3 apkleaks.py -f $apk_file -o results.txt
```
{% endtab %}
{% endtabs %}

## Enumerate AWS buckets

### Manual

1. Search in **strings.xml** for AWS key : `<string name="aws_Identify_pool_ID">$key</string>`
2. Extract data with this AWS key :

```bash
# install
apt-get install awscli
# usage
ws cognito-identity get-id --identity-pool-id $key --region $region
# region exemple : us-east-1
aws cognito-identity get-credentials-for-identity --identity-id $identity-id-from-previous-command --region $region
```

3. Enumerate permissions :

```bash
git clone https://github.com/andresriancho/enumerate-iam
cd enumerate-iam/
pip install -r requirements.txt
python enumerate-iam.py --access-key <ACCESS-KEY-ID> --secret-key <SECRET-KEY-ID> --session-token <SESSION-TOKEN-VALUE>
```

4. Get content :

```bash
export AWS_ACCESS_KEY_ID=$access_key
export AWS_SECRET_ACCESS_KEY=$secret_key
export AWS_SESSION_TOKEN=$session_token

# list aws buckets
aws s3 ls

# list content
aws s3 ls s3://$bucket_name --recursive
```

5. [Privilege Escalation](https://cloud.hacktricks.xyz/pentesting-cloud/aws-security/aws-privilege-escalation)

### Cloud Enum

{% embed url="https://github.com/initstring/cloud_enum" %}

```bash
git clone https://github.com/initstring/cloud_enum.git
cd cloud_enum/
pip install requirements.txt
# usage
python3 cloud_enum.py -k $name_app
```

## Firebase instances

### Manual

```bash
# find instance in source code
cat /res/values/strings.xml | grep "firebase"
# looks like : https://xyz.firebaseio.com/

# or
# FireBaseScanner
git clone https://github.com/shivsahni/FireBaseScanner
cd FireBaseScanner/
# usage
python2 FirebaseMisconfig.py -p app.apk
```

1. Go to browser : https://xyz.firebaseio.com/.json
2. If "**Permission Denied**" : you cannot access it => **well configured**
3. Else, "**null**" or **json data** => db is **public** and you have at least **read access**
   1. Check for **writing** privileges :
      1. [Insecure Firebase Exploit](https://github.com/MuhammadKhizerJaved/Insecure-Firebase-Exploit)

```bash
git clone https://github.com/MuhammadKhizerJaved/Insecure-Firebase-Exploit.git
cd Insecure-Firebase-Exploit/
pip install -r requirements.txt
# usage
python2 Firebase_Exploit.py
```

## Unobfuscated code - Frameworks

### MobSF

{% embed url="https://github.com/MobSF/Mobile-Security-Framework-MobSF" %}

```bash
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git
cd Mobile-Security-Framework-MobSF
./setup.sh

# running it
./run.sh 127.0.0.1:8000

# docker
docker pull opensecurity/mobile-security-framework-mobsf
docker run -it -p 8000:8000 opensecurity/mobile-security-framework-mobsf

# go to browser : 0.0.0.0:8000
```

### APKHunt - OWASP MASVS Static Analyzer

{% embed url="https://github.com/Cyber-Buddy/APKHunt" %}

> APKHunt is a comprehensive static code analysis tool for Android applications, based on OWASP's MASVS. It can be used to identify and address potential security vulnerabilities in application code.

```bash
git clone https://github.com/Cyber-Buddy/APKHunt.git
cd apkhunt
go run apkhunt.go -p app.apk -l
```

### MARA

```bash
git clone --recursive https://github.com/xtiankisutsa/MARA_Framework
cd MARA_Framework
./mara.sh -s app.apk
```

### QARK

```bash
git clone https://github.com/linkedin/qark
cd qark
pip install -r requirements.txt
pip install . --user  # --user is only needed if not using a virtualenv
qark --help

# apk
qark --apk app.apk

# java files
qark --java $path_java_folder
qark --java $file_java
```

### Super Analyzer

{% embed url="https://github.com/SUPERAndroidAnalyzer/super" %}

```bash
# installation
wget https://github.com/SUPERAndroidAnalyzer/super/releases/download/0.5.1/super-analyzer_0.5.1_debian_amd64.deb
dpkg -i super-analyzer_0.5.1_debian_amd64.deb

# usage
super-analyzer app.apk
```

## Obfuscated code - Frameworks

* [Simplify (Generic Android Deobfuscator)](https://github.com/CalebFenton/simplify)
* [Java Deobfuscator](https://github.com/java-deobfuscator/deobfuscator)
* [Online Statistical Deobfuscation for Android - APK-Deguard](http://apk-deguard.com/)
* **How to :**

{% embed url="https://medium.com/rahasak/reverse-engineering-obfuscated-android-apk-67da84b259e4" %}

## Manual

### Hardcoding Issues

{% embed url="https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_password" %}

Sensitive information, such as API keys, passwords, and other credentials, are directly coded into the app's source code instead of being stored securely in a separate configuration file or database.

```bash
# Reverse Engineer the application :
apktool d app.apk
# or
jadx-gui app.apk
# look for sensitive infos such as passwords, api keys, etc...
# check /lib files
```

> Often hardcoded strings can be found in **resources/strings.xml** and also in activity source code.

### Insecure Data Storage

Sensitive information, such as user credentials, personal information, and other sensitive data, is stored in an insecure manner. This information can be stored locally on the device or remotely on a server or in the cloud.

```bash
cd  /data/data/com.$appname
# check shared_preferences file -> credentials may be stored !
# check databases file
# check tmp files
# check in external storages too :
cd /storage
```
