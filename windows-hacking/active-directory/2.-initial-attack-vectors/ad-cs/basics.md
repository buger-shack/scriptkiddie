# Basics

## ADCS | PrivEsc to Domain Admin

{% embed url="https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/adcs-+-petitpotam-ntlm-relay-obtaining-krbtgt-hash-with-domain-controller-machine-certificate" %}

{% embed url="https://www.truesec.com/hub/blog/from-stranger-to-da-using-petitpotam-to-ntlm-relay-to-active-directory" %}

### tools

{% embed url="https://github.com/grimlockx/ADCSKiller" %}

## Reconnaissance & Enumeration

```bash
certipy find -u khal.drogo@essos.local -p 'horse' -dc-ip 192.168.56.12
```

This will search the certificate server, and dump all the information needed in three format :

* **bloodhound** : a zip ready to import in bloodhound (if you use certipy 4.0 you will have to install the bloodhound gui modified by oliver lyak, if you do not want to use the modified version, you must use the -old-bloodhound option)
* **json** : information json formated
* **txt** : a textual format

**Find vulnerable templates :**

```bash
certipy find -u khal.drogo@essos.local -p 'horse' -vulnerable -dc-ip 192.168.56.12 -stdout
```
