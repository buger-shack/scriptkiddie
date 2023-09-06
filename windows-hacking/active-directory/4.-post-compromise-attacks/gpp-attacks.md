# GPP Attacks

<details>

<summary>What are GPP Attacks ?</summary>

Group Policy Preferences (GPP) are a powerful tool that once **allowed administrators to create domain policies with embedded credentials**. These policies allowed administrators to set local accounts, embed credentials for the purposes of mapping drives, or perform other tasks that may otherwise require an embedded password in a script.

_While a useful tool, the storage mechanism for the credentials was flawed and allowed attackers to trivially decrypt the plaintext credentials_.&#x20;

While addressed in **MS14-025**, this patch only prevents new policies from being created, and any legacy GPPs containing credentials must be found and removed. Often, these kinds of policies can involve service accounts that frequently have elevated privileges. And one of these methods to find those credz is mining SYSVOL for credential data.

SYSVOL is the domain-wide share in Active Directory to which all authenticated users have read access. SYSVOL contains logon scripts, group policy data, and other domain-wide data which needs to be available anywhere there is a Domain Controller (since SYSVOL is automatically synchronized and shared among all Domain Controllers).

**All domain Group Policies are stored here:** `\\<DOMAIN>\SYSVOL\<DOMAIN>\Policies\`

</details>

## Attack

> Use a module called **smb\_enum\_gpp** in **metasploit** to find the **group.xml** file

```bash
msfconsole
use auxiliary/scanner/smb/smb_enum_gpp
# crack
gpp-decrypt hash
```
