# 🤖 Android

![](../.gitbook/assets/android.gif)

## Tools

{% embed url="https://github.com/kyawthiha7/Mobile-App-Pentest" %}
Everything about Pentesting Android
{% endembed %}

### MindMap

{% embed url="https://www.xmind.net/m/GkgaYH" %}
Checklist
{% endembed %}

### Mobile Security Framework (MobSF)

{% hint style="info" %}
**Mobile Security Framework** (MobSF) is an automated, all-in-one mobile application (Android/iOS/Windows) pentesting, malware analysis and security assessment framework capable of performing static and dynamic analysis. MobSF support mobile app binaries (APK, XAPK, IPA & APPX) along with zipped source code and provides REST APIs for seamless integration with your CI/CD or DevSecOps pipeline.The Dynamic Analyzer helps you to perform runtime security assessment and interactive instrumented testing.
{% endhint %}

{% embed url="https://mobsf.github.io/Mobile-Security-Framework-MobSF" %}

### QARK

{% hint style="info" %}
**Quick Android Review Kit** This tool is designed to look for **several security related** Android application vulnerabilities, either in source code or packaged APKs. The tool is also capable of creating "**Proof-of-Concept**" deployable APKs and/or ADB commands, capable of exploiting many of the vulnerabilities it finds. There is no need to root the test device, as this tool focuses on vulnerabilities that can be exploited under otherwise secure conditions.
{% endhint %}

{% embed url="https://github.com/linkedin/qark" %}

#### Install

```bash
# install
git clone https://github.com/linkedin/qark
cd qark
pip install -r requirements.txt
pip install . --user  # --user is only needed if not using a virtualenv
qark --help
```

### MarianaTrench

{% hint style="info" %}
**SAST** for Android.
{% endhint %}

{% embed url="https://github.com/facebook/mariana-trench" %}

#### Install

```bash
apt-get install python3 python3-pip python3-venv
python3 -m venv ~/.venvs/mariana-trench
source ~/.venvs/mariana-trench/bin/activate
(mariana-trench)$ pip install mariana-trench
(mariana-trench)$ git clone https://github.com/facebook/mariana-trench
(mariana-trench)$ cd mariana-trench/documentation/sample-app
# run analysis
(mariana-trench)$ mariana-trench \
  --system-jar-configuration-path=$ANDROID_SDK/platforms/android-30/android.jar \
  --apk-path=sample-app-debug.apk \
  --source-root-directory=app/src/main/java
# ...
```

### Frida

{% hint style="info" %}
Hooking method, bypassing root detection, bypassing cert pinning, etc.
{% endhint %}

{% embed url="https://frida.re" %}

#### Root Detection Bypass

{% embed url="https://kth.ninja/2018/12/27/Android-Root-Detection-Bypass" %}

### APKTool

{% hint style="info" %}
Reverse Engineering APK.
{% endhint %}

{% embed url="https://ibotpeaches.github.io/Apktool" %}

### JDGui

{% hint style="info" %}
Code review (Java).
{% endhint %}

{% embed url="https://java-decompiler.github.io" %}
