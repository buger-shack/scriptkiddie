# LDAP Enumeration

## LDAP - Lightweight Directory Access Protocol

> **LDAP** (Lightweight Directory Access Protocol) is a software protocol for enabling anyone to **locate data** about **organizations**, **individuals** and other resources such as files and devices in a network - whether on the public Internet or on a corporate Intranet.&#x20;
>
> LDAP is a "**lightweight**" (smaller amount of code) version of Directory Access Protocol (DAP), which is part of X.500, a standard for directory services in a network.

### Ports

```
389 : LDAP (regular)
636 : LDAPs
3268 : msft-gc, Microsoft Global Catalog (LDAP service which contains data from Active Directory forests) 
3269 : msft-gc-ssl, Microsoft Global Catalog over SSL (similar to port 3268, LDAP over SSL) 
```

### ldapsearch

> **ldapsearch** opens a connection to an LDAP server, binds, and performs a search using specified parameters.

```bash
ldapsearch -h $ip
# “-x” option for simple authentication and specify the search base with “-b”
ldapsearch -x -b &lt;search\_base&gt; -H &lt;ldap\_host&gt;

# If server accepts anonymous authentication, query without binding to the admin account

# 1. to get the DC roots
ldapsearch -x -h <host> -s base namingcontexts</host>

# 2. to get all LDAP information
ldapsearch -x -b "dc=htb,dc=local" -H ldap://$ip
ldapsearch -x -b "dc=htb,dc=local" -h $ip

# 3. fine tune the search
-- Objects --
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user

ldapsearch -x -h $ip -b "DC=htb,DC=local" '(objectClass=Person)'

# 4. look for user accounts
ldapsearch -x -h $ip -b "DC=htb,DC=local" '(objectClass=Person)' sAMAccountName
```

### ldeep

> In-depth ldap enumeration utility&#x20;

{% embed url="https://github.com/franc-pentest/ldeep" %}

```bash
# install
https://github.com/franc-pentest/ldeep.git

# usage
ldeep ldap -a -d winlab.local -s ldap://$IP users -v
```

### ldapdomaindump

> ldapdomaindump is a tool which aims to solve this problem, by collecting and parsing information available via LDAP and outputting it in a human readable HTML format, as well as machine readable json and csv/tsv/greppable files. \
> **Alternative of ldapsearch**&#x20;

{% embed url="https://github.com/dirkjanm/ldapdomaindu" %}

```bash
ldapdomaindump --user "support.htb\\ldap" -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' support.htb
```
