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
