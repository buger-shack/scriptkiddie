# Dynamic Analysis

* Connect device to the laptop

```bash
# get app name
frida-ps -Uai
objection -g "APP" explore
# view the environment variables for the app
env 

# 1. Sensitive information
cd <env>
cd Documents
ios plist userInfo.plist

env
cd Library
cd Preferences
ios plist can com.highaltitidehacks.DVIAswiftv2.plist

# In keychain
ios keychain dump_raw

# SQLite
cd Library
cd Application\ Support
sqlite connect Model.sqlite

.tables # etc

# Cookies
ios cookies get --json

# Device Logs
idevicesyslog -u idevice_id | grep "application name"


# 2. Broken Cryptography
objection -g "APP" explore
ios monitor crypt

# 3. Local Authentication using Keychain
objection -g "APP" explore
ios ui biometrics_bypass --quiet

# 4. Jailbreak detection bypass
# Find the jailbreak detection classes :
ios hooking search jail

# Use HideJB | Close the running app after starting HideJB
# Settings > HideJB preferences > Select Apps > Toggle on the app name

# or
ios jailbreak disable --quiet


# 5. SSL Pinning Bypass
# SSL Kill Switch 2 
# Settings > SSL KILL SWITCH 2 > Toggle on "Disable certificate validation"
# or
ios sslpinning disable

# 6. Monitor the logs
# on host
idevicesyslog -p 1337
```

