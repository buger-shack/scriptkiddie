# Network / Traffic Analysis

## Proxifier

{% embed url="https://medium.com/kinandcartacreated/how-to-easily-proxy-your-android-device-e2ba907de4d9" %}

### HTTP communications (Browser)

{% embed url="https://portswigger.net/burp/documentation/desktop/mobile/config-android-device" %}

1. **Open Burp Suite > Proxy > Options > "Import / Export Certificate Authority "**.
2. Select Certificate in **DER** format
3. Transfer the certificate to your device: `adb push cert.der /data/local/tmp/cert-der.crt`.
4. You can also drag and drop it if you are using an emulator.

### Android System

* See : [**Disable SSL Pinning > Modify the network\_security\_config.xml file**](disable-ssl-pinning.md#modifying-the-network\_security\_config.xml-file)
* [**Android Proxy Toggle**](https://github.com/theappbusiness/android-proxy-toggle) **- GREAT**

#### With ADB

```bash
adb shell settings put global http_proxy YOUR_IP:YOUR_PORT
```

## TCPDump

> Packet analyzer, get the details of the visible traffic of the device.

```bash
adb root
adb remount
adb push /wherever/you/put/tcpdump /system/xbin
adb shell tcpdump -i any -p -s 0 -w /sdcard/capture.pcap
adb pull /sdcard/capture.pcap /wherever/you/want/to/save/it
wireshark /path/to/your/capture
```

## NFC Traffic

{% embed url="https://github.com/nfcgate/nfcgate" %}

> NFCGate is an Android application designed to capture, analyze or modify NFC traffic
