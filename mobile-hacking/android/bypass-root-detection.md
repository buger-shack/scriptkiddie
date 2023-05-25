# Bypass Root Detection

> To understand root detection, we must first understand what rootage is. Rooting is the process of **obtaining the highest privileges** possible on the operating system. In the case of Android, rooting allows you to change or replace applications, files and system settings, and run specialized applications ("apps") that require administrator-level permissions. Root detection is the process of detecting whether a device is rooted. This is usually done when the application is launched. Sometimes root checks are implemented in applications such that the application will not respond or exit when running on a rooted device.

**Resources :**&#x20;

{% embed url="https://redfoxsec.com/blog/android-root-detection-bypass-using-frida/" %}

{% embed url="https://kishorbalan.medium.com/my-fav-7-methods-for-bypassing-android-root-detection-f8afb0ddfaf3" %}

### Frida

```bash
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

### Objection

#### Common method

```bash
# instal
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

#### Manual method

1. Convert the apk file into class files using **dex2jar**
2. Analyse the class files and identify which library is being used for the root detection (for example, de.cyberkatze.iroot)
3. Connect the app with objection : `objection -g “com.package.android” explore`
4. Execute in objection : `android hooking list class_methods <root detection class>` 2. Change return value for the method in charge of Root Detection : `android hooking set return_value <root_detect_class.method> false`
5. For _de.cyberkatze.iroot_, in objection :
   1. `android hooking list class_methods de.cyberkatze.iroot.IRoot`
   2. Change return value for the method : `android hooking set return_value de.cyberkatze.iroot.IRoot.isDeviceRooted false`

### Tampering Smali Code

1. Decompile the apk file using **JADX-GUI** _or any other alternative._
2. Identify the code which is in charge of the root detection process
   1. If the library _rootbear_ is used and there is an 'if' condition to verify if the device is rooted or not :
      1. Decompile the apk with **apktool** : \`\`apktool d app.apk -o app-decompile\`\`\`
      2. Find the SMALI code for the 'if' statement (can look like this) : `if-eqz v0, :cond_0`
      3. Change it by : `if-nez v0, :cond_0`
      4. Save the file and rebuild the application : `apktool b app-decompile -o app-root-patch.apk`
      5. Sign the application (see '`## Decompile/Compile Source Code`)
      6. Re-install the application on the android device

### AndroPass

{% embed url="https://github.com/koengu/AndRoPass" %}

> Allows to bypass root and emulator detection. Can also bypass the **RootBeer** detection mechanism.

```bash
# installation
git clone https://github.com/koengu/AndRoPass.git
pip install -r requirements.txt

# usage
python3 AndRoPass.py -a app.apk
```

### Medusa Framework

{% embed url="https://github.com/Ch0pin/medusa" %}

```bash
# install
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

### Magisk - MagiskHide

1.  **Download Magisk.apk** : [https://github.com/topjohnwu/Magisk/releases](https://github.com/topjohnwu/Magisk/releases)


2.  **Launch it in your device** : Allow SuperUser access&#x20;


3.  **Follow :** [https://www.xda-developers.com/how-to-install-magisk/](https://www.xda-developers.com/how-to-install-magisk/)


4.  **Enable MagiskHide :** Magisk App > Settings > Magisk > Magisk Hide (enable)&#x20;


5. **Choose the application in which we will hide the root**
