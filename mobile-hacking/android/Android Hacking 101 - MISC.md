# MISC
## ADB Commands
```bash
### ADB
adb install $apk_file # install app
adb shell pm list packages # list apps
adb shell netstat 
adb shell ps
adb shell # Connect to device
adb install # sideload apks
adb push # PC to device
adb pull # Device to PC
adb logcat # commande line took that dumps a log of system messages
adb shell pm list packages # will give you a list of all installed package names
adb uninstall $package_name # uninstall a package

# start service
adb shell am startservice app.beetlebug/app.beetlebug.handlers.VulnerableService

```

## Enable screenshot in app
```bash
# Common in Banking application. Technically not a vulnerability.
wget https://gist.githubusercontent.com/su-vikas/36410f67c9e0127961ae344010c4c0ef/raw/a05b9aee83d1edcd78d8aa4d69f32c660404e0af/screenshot.js
frida -U -f $package -l screenshot.js
```

## NFC Traffic
https://github.com/nfcgate/nfcgate