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

* chall/response using NT hash
* NTLMSSP Communication with DC over NetLogon (RPC)
