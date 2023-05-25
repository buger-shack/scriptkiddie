# Disable SSL Pinning

## Disable SSL Pinning

**Resources :**

{% embed url="https://infosecwriteups.com/hail-frida-the-universal-ssl-pinning-bypass-for-android-e9e1d733d29" %}

{% embed url="https://kishorbalan.medium.com/its-all-about-android-ssl-pinning-bypass-and-intercepting-proxy-unaware-applications-91689c0763d8" %}

{% embed url="https://blog.certcube.com/android-ssl-pinng-bypass-with-frida/" %}

### Frida

```bash
# install
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

# via codeshare
frida -U --codeshare pcipolloni/universal-android-ssl-pinning-bypass-with-frida -f $package
```

### APK-MITM

```bash
# install
npm install -g apk-mitm

# usage
apk-mitm $apk
# reinstall the app
adb uninstall $package-app
adb install $apk_patched
```

### Magisk - Move Certificates Module

1.  **Download Magisk.apk** : [https://github.com/topjohnwu/Magisk/releases](https://github.com/topjohnwu/Magisk/releases)


2.  **Launch it in your device** : Allow SuperUser access&#x20;


3.  **Follow :** [https://www.xda-developers.com/how-to-install-magisk/](https://www.xda-developers.com/how-to-install-magisk/)


4. **Enable MagiskHide :** Magisk App > Modules > Enable 'Move Certificates'

### Objection

```bash
pip3 install objection

# start frida server on android device
adb shell 
cd /data/local/tmp
./frida-server

# on host
objection -g “com.package.android” explore
android sslpinning disable
```

### Modifying the network\_security\_config.xml file

> The Network Security Configuration lets apps customize their network security settings through _a declarative configuration file_. The entire configuration is contained within this XML file, and no code changes are required. The Network Security Configuration works in **Android 7.0 or higher.**

1. Install Burp CA certificate on the device.
2. Decompile the android application with **apktool** : `apktool d app.apk -o app-decompile`
3. Locate the **network\_security\_config.xml** file under **/res/xml**
4. Remove the `<pin-set>...</pin-set>` tag section and add :

```xml
<trust-anchors>
	<certificates src="user" />
	<certificates src="system" />
</trust-anchors>
```

4.  If the **network\_security\_config.xml** file is not present in the application, the **AndroidManifest.xml** file must also be modified by adding the **networkSecurityConfig** tag as follows :

    ```xml
    <application android:name="AppName" android:networkSecurityConfig="@xml/network_security_config">
    ```
5. Save the file and repackage the application: `apktool b app-decompile -o app-ssl.apk`.
6. Sign the application (see Reversing > Decompilation)
