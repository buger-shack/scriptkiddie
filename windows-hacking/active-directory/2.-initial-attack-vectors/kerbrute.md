# Kerbrute

### `kerbrute`

> _A tool to quickly bruteforce and enumerate valid Active Directory accounts through Kerberos Pre-Authentication_

{% embed url="https://github.com/ropnop/kerbrute" %}

### Attack

**Port 88** : Kerberos authentication system

<pre class="language-bash"><code class="lang-bash"><strong># install via releases : https://github.com/ropnop/kerbrute
</strong><strong>./kerbrute userenum -d $domain_ldap --dc $dc_ip /usr/share/SecLists/Usernames/xato-net-10-million-usernames.txt
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (2).png" alt="" width="563"><figcaption></figcaption></figure>
