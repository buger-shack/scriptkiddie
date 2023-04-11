# Static Analysis
https://github.com/bytedance/appshark
## Read Source Code
**Check for :**
- Weak or improper cryptography use
- Exported Preference Activities
- Apps which enable backups
- Apps which are debuggable
- App Permissions.
- Firebase Instance(s)
- Sensitive data in the code

**Files :**
- **AndroidManifest.xml**: In this file, you can get basic information about the application and its functionalities.
- **res/values/strings.xml**: This file stores the string resources for the application. It’s a good place to look for hardcoded secrets and info leaks.
- Files **.db** or **.sqlite**: Look for these files to see what information is shipped along with the application. This is an easy source of secrets and info leaks.

## Information Gathering / Enumeration
- Search for secrets / URI
### AndroidManifest.xml
The Android Manifest is an XML file which contains important metadata about the Android app. This includes the package name, activity names, main activity (the entry point to the app), Android version support, hardware features support, permissions, and other configurations.
### apkleaks - BEST
https://github.com/dwisiswant0/apkleaks
```bash
git clone https://github.com/dwisiswant0/apkleaks
cd apkleaks/
pip3 install -r requirements.txt
# usage
python3 apkleaks.py -f $apk_file -o results.txt
```
### apkenum - MINIMAL
```bash
git clone https://github.com/shivsahni/APKEnum.git
cd APKEnum
python2 APKEnum.py -p $apk_file
```

## Enumerate AWS Storage Bucket
https://github.com/initstring/cloud_enum
```bash
git clone https://github.com/initstring/cloud_enum.git
cd cloud_enum/
pip install requirements.txt
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
python2 FireBaseScanner.py -p $apk_file
```
1. Go to browser : https://xyz.firebaseio.com/.json
2. If "**Permission Denied**" : you cannot access it => **well configured**
3. Else, "**null**" or **json data** => db is **public** and you have at least **read access**
	1. Check for **writing** privileges :
		1. https://github.com/MuhammadKhizerJaved/Insecure-Firebase-Exploit
```bash
git clone https://github.com/MuhammadKhizerJaved/Insecure-Firebase-Exploit.git
cd Insecure-Firebase-Exploit/
pip install -r requirements.txt
python2 Firebase_Exploit.py
```

## Unobfuscated code - Frameworks
### MARA
```bash
git clone --recursive https://github.com/xtiankisutsa/MARA_Framework
cd MARA_Framework
./mara.sh --help
```

### MobSF
```bash
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git
cd Mobile-Security-Framework-MobSF
./setup.sh

# running it
./run.sh 127.0.0.1:8000
```

### QARK
```bash
git clone https://github.com/linkedin/qark
cd qark
pip install -r requirements.txt
pip install . --user  # --user is only needed if not using a virtualenv
qark --help

# apk
qark --apk path/to/my.apk

# java files
qark --java $path_java_folder
qark --java $file_java
```
## Obfuscated code - Frameworks
http://apk-deguard.com/

https://github.com/java-deobfuscator/deobfuscator
- **How to :**
https://medium.com/rahasak/reverse-engineering-obfuscated-android-apk-67da84b259e4

## Manual
### Hardcoding Issues
https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_password
Sensitive information, such as API keys, passwords, and other credentials, are directly coded into the app's source code instead of being stored securely in a separate configuration file or database.

```bash
# Reverse Engineer the application :
apktool d $apk_file
# or
jadx-gui $apk_file
# look for sensitive infos such as passwords, api keys, etc...
# check /lib files
```
>Often hardcoded strings can be found in **resources/strings.xml** and also in activity source code.

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
