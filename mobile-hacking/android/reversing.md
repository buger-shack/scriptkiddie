# Reversing
>In this part we will extract the legitimate apk from emulator or the device and get the source code.
## First Steps
```bash
# install tools
apt install adb apktool openjdk-11-jdk-headless zipalign apksigner

# list android attached devices
adb devices

# find app name
adb shell pm list packages -f | grep $app_name

# list installed packages
adb shell pm list packages -f

# get path of the app we want to reverse
adb shell pm path $package_name

# get the app on our local machine on current folder
adb pull $PATH_app .
```
## Get the source code
**Tools :**
- jadx / jadx-gui
- dex2jar
- apktool
- apkx

```bash
jadx -d $path_output_folder app.apk

# or d2j
d2j-dex2jar.sh app.apk

# or apktool decompile
apktool d app.apk -o $output_folder

# or apkx
apkx app.apk
cd app
# open in visual studio
code .

# or use jadx-gui
jadx-gui app.apk
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
## apksigner supports signature v1 to v4
apksigner sign --ks demo.keystore --ks-pass pass:$password app.apk

# verify the signature
jarsigner -verify app.apk
# zipalign the APK
zipalign 4 app.apk app_signed.apk

# also for signing :
## d2j-apk-sign
d2j-apk-sign app.apk -o app-signed.apk

## uber-apk-signer
java -jar uber-apk-signer.jar --apks app.apk
```
