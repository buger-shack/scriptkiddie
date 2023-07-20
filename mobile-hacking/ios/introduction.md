# Introduction

## Basics

### Apps

* System Applications can be found : /Applications
* User-installed applications : /private/var/containers/
* **Apps have 2 main locations** :
  * **Bundle** :
    * _/var/containers/Bundle/Application/_
    * Contains the application's executable code (binary) along with any resources used by the application, such as the application's icon, interface files, and localized content.
  * **Data** :
    * _/var/mobile/Containers/Data/Application/_
    * Applications can write files, create databases, save user preferences, etc on RUNTIME. This directory is sandboxed, meaning each application can only access its own directory and can't view or modify files in another application's directory.

```bash
# install app on device
ideviceinstaller -i app.ipa

# to obtain the ipa file name, if we installed via appstore or etc
find /var/ -name "*.app"
```

### Structure of IPA file

* IPA files are ZIP files : change the extension to .zip and decompress them
  * .app is a zip archive containing the rest of the resources
  * info.plist : application-specific configurations
  * `_CodeSignature/` : plist file with a signature for all the files in the bundle
  * Assets.car : zip archive containing assets (icons...)
  * Frameworks/ : contains native librairies for the app as .dylib or .framework files
  * PlugIns/ : may contain app extensions as .appex files (not present in all cases).
  * Core Data : store permanent data for offline use, cache temporary data, and add undo functionality to the app on a single device.
  * PkgInfo : This file is an alternate way to specify the type and creator codes for the application or bundle.
  * en.lproj, fr.proj, Base.lproj: language packs containing resources for specific languages, and a default resource in case a language is not supported.

### Security features

The main security features of iOS are:

* **Code signing**
  * Ensures that all applications come from a approved source (using Apple-issued certificates)
* **Generic exploit mitigations**
  * Address Space Layout Randomization (ASLR)
    * Usually compiled using -fPIE –pie
  * Non Executable Memory (ARM’s Execute Never feature)
  * Stack Smashing Protections (SSP)
    * Usually compiled with –fstack-protector-all flag
* **Sandboxing**
  * run applications as non-privileged user
  * 3rd-party apps are restricted in accessing files stored by other apps
* **Memory Management**
  * Automatic Reference Counting (ARC) protects applications from memory coruption issues by letting the compiler do the memory management stuff

### [Jailbreak](./#jailbreak)

* [Can I Jailbreak](https://canijailbreak.ams1gn.id/)

#### Taurine

* Install **Cydia** via **Zebra** via **Sileo**
* Otool :&#x20;
  * Cydia repository: **http://apt.thebigboss.org/repofiles/cydia/**&#x20;
  * Installation: search for “**Big Boss Recommended Tools**” on Cydia&#x20;
  * Installation2: search for “**Darwin CC tools**” on Cydia

## Install Tools on iOS

Cydia > Manage > Sources > Edit > Add **https://cydia.akemi.ai/ https://build.frida.re**

Cydia > Search > HideJB / Frida / AppSync Unified

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

{% embed url="https://portswigger.net/burp/documentation/desktop/mobile/config-ios-device" %}

> **On device :**&#x20;
>
> Settings > Wi-Fi > Configure Proxy > Manual > IP:8082 Safari > http://burp > Download and Allow&#x20;
>
> Settings > General > Profile > PortSwigger CA > Install > Done&#x20;
>
> Settings > General > About > Certificate Trust Settings > Enable Toggle PortSwigger CA
