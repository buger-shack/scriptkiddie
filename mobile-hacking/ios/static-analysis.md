# Static Analysis

### MobSF

Run MobSF > Drag and drop IPA file

### Manual

```bash
# obtain path of the app
find /var/ -name "*.plist" | grep "<app>"

# 1. Protections in the binary
# PIE (Position Independent Executable): When enabled, the application loads into a random memory address every-time it launches, making it harder to predict its initial memory address.
otool -hv <app-binary> | grep PIE

# Stack Canaries: To validate the integrity of the stack, a ‘canary’ value is placed on the stack before calling a function and is validated again once the function ends.
otool -I -v <app-binary> | grep stack_chk

# ARC (Automatic Reference Counting): To prevent common memory corruption flaws
otool -I -v <app-binary> | grep objc_release 

# Encrypted Binary: The binary should be encrypted
otool -arch all -Vl <app-binary> | grep -A5 LC_ENCRYPT

# 2. Sensitive Functions
# Weak Hashing Algorithms
otool -I -v <app-binary> | grep -w "_CC_MD5"
otool -I -v <app-binary> | grep -w "_CC_SHA1"

# Insecure Random Functions
otool -I -v <app-binary> | grep -w "_random"
otool -I -v <app-binary> | grep -w "_srand"
otool -I -v <app-binary> | grep -w "_rand"

# Insecure ‘Malloc’ Function
otool -I -v <app-binary> | grep -w "_malloc"

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

# 3. Info.plist checks
# unzip the ipa file and then get the info.plist
# convert into xml
plistutil -i Info.plist -o Infoxml.plist

# or with objection
objection -g "APP" explore
ios plist cat Info.plist

# Check in the info.plist :
#     Bundle identifier ; Bundle version ; Supported device types ; Required permissions ; URL schemes ; NSAppTransportSecurity
```
