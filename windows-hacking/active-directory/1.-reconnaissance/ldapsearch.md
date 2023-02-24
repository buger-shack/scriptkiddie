# LDAPSearch

## Ports

```
389 : LDAP (regular)
636 : LDAPs
3268 : msft-gc, Microsoft Global Catalog (LDAP service which contains data from Active Directory forests) 
3269 : msft-gc-ssl, Microsoft Global Catalog over SSL (similar to port 3268, LDAP over SSL) 
```

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
