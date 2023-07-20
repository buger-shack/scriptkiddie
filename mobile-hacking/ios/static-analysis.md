# Static Analysis

## Static Analysis

### MobSF

Run MobSF > Drag and drop IPA file

### Dump App Path Files on Host

```bash
git clone https://github.com/AloneMonkey/frida-ios-dump
cd frida-ios-dump/
pip3 install -r requirements.txt

python3 dump.py -H $ios-device-ip -p 22 $app-name
```

### Binary Analysis

#### otool

```bash
# obtain path of the app
find /var/ -name "*.plist" | grep "<app>"

<<'
Script that does all the checks for binary
https://github.com/saladandonionrings/iOS-Binary-Security-Analyzer
'>>
./check-binary.sh <binary>

<<'
Protections in the Binary
'>>
# PIE (Position Independent Executable): When enabled, the application loads into a random memory address every-time it launches, making it harder to predict its initial memory address.
otool -hv <app-binary> | grep PIE
# Output : should return PIE

# Stack Canaries: To validate the integrity of the stack, a ‘canary’ value is placed on the stack before calling a function and is validated again once the function ends.
otool -I -v <app-binary> | grep stack_chk
# Good practice output : 
# stack_chk_fail
# stack_chk_guard

# ARC (Automatic Reference Counting): To prevent common memory corruption flaws
otool -I -v <app-binary> | grep _objc_
# Good practice output :
# _objc_retain
# _objc_release
# _objc_storeStrong
# _objc_releaseReturnValue
# _objc_autoreleaseReturnValue
# _objc_retainAutoreleaseReturnValue

# Encrypted Binary: The binary should be encrypted
otool -arch all -Vl <app-binary> | grep -A5 LC_ENCRYPT
# Output : if cryptid is 0 -> not encrypted

<<'
Weak Crypto
'>>
# Weak Hashing Algorithms
otool -I -v <app-binary> | grep -w "_CC_MD5"
otool -I -v <app-binary> | grep -w "_CC_SHA1"
# Good practice : no output

<<'
Insecure Functions
'>>
# Insecure Random Functions
otool -I -v <app-binary> | grep -w "_random"
otool -I -v <app-binary> | grep -w "_srand"
otool -I -v <app-binary> | grep -w "_rand"
# Good practice : no output

# Insecure ‘Malloc’ Function - Lead to memory related vulnerabilities
otool -I -v <app-binary> | grep -w "_malloc"
# Good practice : no output

# Insecure and Vulnerable Functions
otool -I -v <app-binary> | grep -w "_gets"
otool -I -v <app-binary> | grep -w "_memcpy"
otool -I -v <app-binary> | grep -w "_strncpy"
otool -I -v <app-binary> | grep -w "_strlen"
otool -I -v <app-binary> | grep -w "_vsnprintf"
otool -I -v <app-binary> | grep -w "_sscanf"
otool -I -v <app-binary> | grep -w "_strtok"
otool -I -v <app-binary> | grep -w "_alloca"
otool -I -v <app-binary> | grep -w "_sprintf"
otool -I -v <app-binary> | grep -w "_printf"
otool -I -v <app-binary> | grep -w "_vsprintf"
# Good practice : no output
```

#### objection

```bash
# binary analysis
objection -g app explore
ios info binary

<<'
PLIST Checks
'>>
# unzip the ipa file and then get the info.plist
# convert into xml
plistutil -i Info.plist -o Infoxml.plist

# or objection
objection -g "APP" explore
ios plist cat Info.plist

# Check in the info.plist :
#     Bundle identifier ; Bundle version ; Supported device types ; Required permissions ; URL schemes ; NSAppTransportSecurity
# MinimumOSVersion must be 14.0 (non vulnerable)
# NSAppTransportSecurity
#	 Must be set and NSAllowArbitraryLoads must be set to NO (by default), setting it to YES will opt-out of ATS and its security
# Hardcoded API Keys
```

### Files Analysis

* Search for **sensitive** **information** in application files
* App files in :&#x20;
  * &#x20;`/var/mobile/Containers/Data/Application/XXXXXXXX/`
  * `/private/var/containers/Bundle/Application/XXXXXXXX/`

```bash
# search for IP
grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' -r *

# search for Emails
grep -Eo '[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$' -r *

# search for DNI
grep -Eo '[0-9]{8,8}[A-Za-z]$' -r *

# search for IBAN
grep -Eo '[a-zA-Z]{2}[0-9]{2}[a-zA-Z0-9]{4}[0-9]{7}([a-zA-Z0-9]?){0,16}' -r *

# search for Base64
grep -Eo '(?:[A-Za-z0-9+\/]{4})*(?:[A-Za-z0-9+\/]{2}==|[A-Za-z0-9+\/]{3}=)?$' -r *
```

#### Cookies

```
/var/mobile/Containers/Data/Application/XXXXXXXX/Library/Cookies/ 
```

{% embed url="https://github.com/as0ler/BinaryCookieReader" %}
Use this tool
{% endembed %}

```bash
wget https://raw.githubusercontent.com/as0ler/BinaryCookieReader/master/BinaryCookieReader.py
python3 BinaryCookieReader.py $cookie-file
```

### Databases

#### Core Data

```
/var/mobile/Containers/Data/Application/XXXXXXXX/Library/Application Support/.sqlite
```

#### Realm

```
/var/mobile/Containers/Data/Application/XXXXXXXX/Documents/.realm
```

#### Couchbase

```
/var/mobile/Containers/Data/Application/XXXXXXXX/Library/Application Support/CouchbaseLite/
```

#### YapDatabase

```
/var/mobile/Containers/Data/Application/XXXXXXXX/Library/Application Support/YapDatabase
```
