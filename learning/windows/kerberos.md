# Kerberos

## Brief

![How Kerberos works](<../../.gitbook/assets/image (142).png>)

Kerberos is just SSO, it's like SAML or OpenID. **Port 88** : Kerberos authentication system Authentication to a trusted source (KDC) KDC delegates access

* _KDC_ = Key Distribution Center
* _AS_ = Authentication Service
* _TGT_= Ticket Granting Ticket
* _TGS_ = Ticket Graning Service

In network, protocol used is **KRB5** TGS are for resources, not hosts

### **Authentication Process**

* Authenticate to AS with a password → Get a TGT
* Request access to resource from TGS → Show TGT
* Valid TGT → Get TGS
* Show TGS to resource → resource accepts TGS → Log in

Each resource can check for valid TGS → Privileged Attribute Certificate (PAC) → Addition to Kerberos

### **NTLM Authentication**

1. (Interactive authentication only) A user accesses a client computer and provides a domain name, user name, and password. The client computes a cryptographic hash of the password and discards the actual password.
2. The client sends the user name to the server (in plaintext).
3. The server generates a 16-byte random number, called a challenge or nonce, and sends it to the client.
4. The client encrypts this challenge with the hash of the user's password and returns the result to the server. This is called the response.
5. The server sends the following three items to the domain controller:
   - User name
   - Challenge sent to the client
   - Response received from the client
6. The domain controller uses the user name to retrieve the hash of the user's password from the Security Account Manager database. It uses this password hash to encrypt the challenge.
7. The domain controller compares the encrypted challenge it computed (in step 6) to the response computed by the client (in step 4). If they are identical, authentication is successful.

## **Attacks**

### Kerberos Unconstrained Delegation attack

#### Requirements

- A domain computer with the delegation option ***“Trust This computer for delegation to any service”\*** enabled.
- Local ***admin privileges\*** on the delegated computer to dump the TGT tickets. If you compromised the server as a regular user, you would need to escalate to abuse this delegation feature.

#### Infos

> **Unconstrained delegation** allows a **user or computer** with the option “***Trust This user/computer for delegation to any service (Kerberos Only)\***” enabled to **impersonate ANY** user authenticates to it and request access to **ANY service.**

Example of a user authenticating to a web server and wanting to request data from a database server that is hosted on a different server. Without delegation, **the web server doesn’t have permission to request the data directly.**

![No Unconstrained](https://miro.medium.com/v2/resize:fit:700/1*tOIKvm1Mgn-GYumlz3lanQ.png)

However, if the **delegation** is enabled, the web server can **impersonate** the authenticated user and **forward their credentials** to fetch the requested information from the database **as if it was the user authenticating directly to the service.**

![Unconstrained](https://miro.medium.com/v2/resize:fit:720/format:webp/1*-Rp4pWfit5SmNIoZFfUxYw.png)

#### Steps explained

- **Step 1**: Client requests TGT from KDC. Usual.
- **Step 2**: Client requests TGS (service ticket) to `HTTP/WEBSERVER`. Upon receiving this request, KDC would notice that `WEBSERVER$` has `TRUSTED_FOR_DELEGATION` UAC flag set. Thus, KDC would reply back with the TGS with `ok-as-delegate` flag set.
- **Step 3**: Client would receive the `HTTP/WEBSERVER` TGS from KDC. It would notice that the `ok-as-delegate` flag is set, which indicates that the service it is trying to authenticate with has Unconstrained Delegation set and client needs to send a copy of its TGT along with TGS. Thus, client would send another TGS request here, this time requesting a copy of its TGT. KDC would receive this request, and reply with a service ticket containing a `Forwardable` TGT.
- **Step 4**: Client would then send the TGS(`HTTP/WEBSERVER`) + TGT(`Forwardable`) to the `WEBSERVER$` web service.
- **Step 5**: Server would receive the request and while serving the response it would notice it needs to interact with a remote share on `FILESERVER$` server. `WEBSERVER$` would then send a service request to KDC for SPN `CIFS/FILESERVER` along with TGT(`Forwardable`) it received from client. KDC would comply with the request and issue the TGS.
- **Step 6**: `WEBSERVER$` receive the `CIFS/FILESERVER` TGS from KDC, accesses the file share and proceeds with the response.

#### PrinterBug

>We are combining Printer Bug and Unconstrained Delegation to escalate our impact.
>
>Printer Bug is a bug with how Print Spooler service works. By exploiting Printer Bug, we can force any system that runs the Print Spooler service to interact with any other system. For example, if we have a shell in a client machine, and the DC is running Print Spooler, we can ask DC to connect to client or any other machine. We can combine this behavior with Unconstrained Delegation. We can force DC to connect with a system which has Unconstrained Delegation set. That way, when DC would connect to that system, it would authenticate first. Due to Unconstrained Delegation, it would send a copy of its TGT to that system. And that’s what we want! TGT of DC! It can allow us to perform loads of attack, I’ll use the example of DCSync below.

**DEMO** : https://www.youtube.com/watch?v=2hGWdqkmpL0

### Kerberos Constrained Delegation attack

#### Requirements

- A user or computer account with the delegation option enabled — ***“Trust This user/computer for delegation to specified services only”\***.
- Local ***admin privileges\*** on the delegated compromised host. If you compromised the server as a regular user, you would need to escalate to admin to abuse this delegation feature.

#### Infos

> **Constrained delegation** allows the **account** with the ***“Trust this user/computer for delegation to specified services only”\*** enabled to impersonate **ANY user** to access **specific services** listed in the **allowable delegation list**.
>
> Like Unconstrained Delegation, Constrained Delegation also requires a user with `SeEnableDelegation` to set it up on any account.
>
> **Kerberos Protocol does this type of delegation with two extensions:**
>
> - **Service for User to Self (S4U2Self)** — Kerberos protocol transition extension.
> - **Service for User to Proxy (S4U2Proxy)** — Kerberos Constrained Delegation extension.

#### Steps

To understand the authentication flow, let’s take an example of a user authenticating to a constrained delegated account like a web service account that only allows delegation to **SQL** services. The authentication flow would consist of five(5) main steps.

1. The user authenticates to the domain controller (DC) using their username and password. The KDC verifies the user’s credentials and issues a Ticket Granting Ticket (TGT) to the user.
2. Using the obtained TGT, the user requests a service ticket for the web service; the KDC verifies the TGT’s authenticity, and if everything is fine, it grants the service ticket to the web service.

📍 *This step uses* ***Service for User To Self (S4U2self) extension that\*** *allows a user to obtain a service ticket for themselves.*

![step1](https://miro.medium.com/v2/resize:fit:720/format:webp/1*WtIL0c2AuPE7AmhNK3E_DQ.png)

3. The web service, now acting on behalf of the user, initiates a new request to the SQL service, presenting the TGS ticket received to the SQL service.

*📍This step uses* ***the Service for User to Proxy (S4U2proxy)\*** *extension to obtain a service ticket for another service on behalf of a user*

![step2](https://miro.medium.com/v2/resize:fit:720/format:webp/1*AKkhVM_aw-9ZIqJuMD6ZEg.png)

4. The SQL service forwards the TGS ticket to the KDC for verification, then grants the service ticket for the SQL service to the web service.
5. The web service presents the service ticket to the SQL service, which verifies the ticket’s authenticity with the KDC and then grants access to the requested resources.

![step3](https://miro.medium.com/v2/resize:fit:720/format:webp/1*s6RwWyNiNrKuEqiotoOclA.png)

### Kerberos Resource-based Constrained Delegation

#### Requirements

- Populate the **msDS-AllowedToActOnBehalfOfOtherIdentity** attribute with a computer account that they have control over.
- Know a **SPN** set on the object that they want to gain access

#### Infos

- In unconstrained and constrained Kerberos delegation, a computer/user is told what resources it can delegate authentications to;
- In resource based Kerberos delegation, **computers** (resources) **specify who they trust** and who can delegate authentications to them.
- In this case, the constrained object will have an attribute called ***msDS-AllowedToActOnBehalfOfOtherIdentity*** with the name of the user that can impersonate any other user against it.
  - Another important difference from this Constrained Delegation to the other delegations is that any user with **write permissions over a machine account** (*GenericAll/GenericWrite/WriteDacl/WriteProperty/etc*) can set the ***msDS-AllowedToActOnBehalfOfOtherIdentity*** (In the other forms of Delegation you needed domain admin privs).

#### Steps

1. We have compromised a non-privileged account on a Windows 10 host that has access to write the msDS-AllowedToActOnBehalfOfOtherIdentity attribute on a domain controller due to poorly configured Active Directory permissions.
2. We will create a new computer account using PowerMad (allowed due to the default MachineAccountQuota value).
3. We set the msDS-AllowedToActOnBehalfOfOtherIdentity attribute to contain a security descriptor with the computer account we created.
4. We leverage Rubeus to abuse resource-based constrained delegation.
