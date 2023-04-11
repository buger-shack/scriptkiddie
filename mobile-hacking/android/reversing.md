# Reversing

> In this part we will extract the legitimate apk from emulator or the device and get the source code.

## First Steps

```bash
# list android attached devices
adb devices

# list installed packages
adb shell pm list packages -f

# get path of the app we want to reverse
adb shell pm path $package_name

# get the app on our local machine on current folder
adb pull /data/app/com.android.insecurebankv2-Rs76pvad3jczKriwMsp5Wg==/base.apk insecurebank_pulled.apk
```

## Get the source code

**Tools :**

* jadx / jadx-gui
* dex2jar
* apktool
* apkx

```bash
jadx -d $path_output_folder $apk_file

# or d2j
d2j-dex2jar.sh $apk_file

# or apktool decompile
apktool d $apk_file -o $output_folder

# or apkx
apkx dvbanew.apk
cd dvbanew
# open in visual studio
code .

# or use jadx-gui

# Open the JAR file with JD-GUI and you’ll see its Java code.
```

## Decompile/Compile Source Code

```bash
## Install app on android device
adb install app.apk

# decompile
apktool d app.apk -o app-decompile 

# remove app from phone 
adb uninstall app.apk

# recompile
apktool b app-decompile/ -o app.apk

# sign
# create keystore
keytool -genkey -v -keystore demo.keystore -alias demokeys -keyalg RSA -keysize 2048 -validity 10000
# sign the apk
jarsigner -sigalg SHA1withRSA -digestalg SHA1 -keystore demo.keystore -storepass demopass app.apk demokeys
# verify the signature
jarsigner -verify app.apk
# zipalign the APK
zipalign 4 app.apk app_signed.apk

# d2j-apk-sign
d2j-apk-sign app.apk -o app-signed.apk

# uber-apk-signer
java -jar uber-apk-signer.jar --apks app.apk
```
