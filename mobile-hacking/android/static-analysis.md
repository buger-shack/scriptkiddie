# Static Analysis

{% embed url="https://github.com/bytedance/appshark" %}

## Read Source Code

**Check for :**

* [ ] Weak or improper cryptography use
* [ ] Exported Preference Activities
* [ ] Apps which enable backups
* [ ] Apps which are debuggable
* [ ] App Permissions.
* [ ] Firebase Instance(s)
* [ ] Sensitive data in the code (secrets/api keys)

**Files :**

* **AndroidManifest.xml**: In this file, you can get basic information about the application and its functionalities.
* **res/values/strings.xml**: This file stores the string resources for the application. It’s a good place to look for hardcoded secrets and info leaks.
* Files **.db** or **.sqlite**: Look for these files to see what information is shipped along with the application. This is an easy source of secrets and info leaks.

### Tips

* [ ] Look for **openOrCreateDatabase()** function in source code, it's used by SQLCipher to store key to encrypt DB. `SQLiteDatabase database = SQLiteDatabase. **openOrCreateDatabase** (databaseFile, "test123", null);`
* [ ] Check for **onReceivedSslError**. function in the code, it tells the WebView to ignore SSL errors and proceed with the connection. Can be used by attackers to read or completely change the content being displayed to users. (Page **232**)
* [ ] Look for **setAllowUniversalAccessFromFileURLs** option set to true , can allow attackers to load their files inside WebViews.
* [ ] Always look for **WebView** or **addJavaScriptInterface** keywords in code, can be used to exploit further.
* [ ] Always look for **(((android:targetActivity)))=".someactivity"** to find proxied activities.
* [ ] Search for **filterTouchesWhenObscured** to find if vulnerable to tapjacking or not.
* [ ] Try to access _/Keys/_ instead of _/Keys_, it sometimes bypasses pattern-matching in content URI's.

> To find the name of the launch activity examine the application’s manifest or use **theapp.package.launchintent** module in drozer. You can also launch the main activity from drozer using **theapp.activity.start** module.

## Information Gathering / Enumeration

### AndroidManifest.xml

> The Android Manifest is an XML file which contains **important** metadata about the Android app. This includes the package name, activity names, main activity (the entry point to the app), Android version support, hardware features support, permissions, and other configurations.

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

{% tab title="apkelaks" %}
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

<details>

<summary></summary>



</details>

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
