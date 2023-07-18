# Introduction

## Basics

* **/Applications** : System Applications can be found
* **/private/var/containers/** : User-installed applications
* Apps have 2 main locations :
  * Bundle : **/var/containers/Bundle/Application/**
  * Data : **/var/mobile/Containers/Data/Application/**

```bash
# install app on device
ideviceinstaller -i app.ipa

# to obtain the ipa file name, if we installed via appstore or etc
find /var/ -name "*.app"
```

### Structure of IPA file

* IPA files are ZIP files : change the extension to .zip and decompress them
  * .**app** is a zip archive containing the rest of the resources
  * i**nfo.plist** : application-specific configurations
  * `_CodeSignature/` : plist file with a signature for all the files in the bundle
  * **Assets.car** : zip archive containing assets (icons...)
  * **Frameworks/** : contains native librairies for the app as .dylib or .framework files
  * **PlugIns/** : may contain app extensions as .appex files (not present in all cases).
  * **Core Data** : store permanent data for offline use, cache temporary data, and add undo functionality to the app on a single device.
  * **PkgInfo** : This file is an alternate way to specify the type and creator codes for the application or bundle.
  * **en.lproj**, **fr.proj**, **Base.lproj** : language packs containing resources for specific languages, and a default resource in case a language is not supported.

## Install Tools on iOS

_**Cydia > Manage > Sources > Edit > Add**_ _https://cydia.akemi.ai/ https://build.frida.re_

_**Cydia > Search > HideJB / Frida / AppSync Unified**_

**Also :**

```bash
# on ios device
ssh root@ios-device

# ssl pinning
cd /tmp
wget https://github.com/nabla-c0d3/ssl-kill-switch2/releases/download/0.14/com.nablac0d3.sslkillswitch2_0.14.deb .
dpkg -i com.nablac0d3.sslkillswitch2_0.14.deb

killall -HUP SpringBoard
apt install otool
```

### Burp

**On device :**&#x20;

_**Settings > Wi-Fi > Configure Proxy > Manual > IP:8082 Safari > http://burp > Download and Allow**_&#x20;

_**Settings > General > Profile > PortSwigger CA > Install > Done Settings > General > About > Certificate Trust Settings > Enable PortSwigger CA**_
