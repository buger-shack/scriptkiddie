# Attacking JWT

## Aim

JWT attacks involve a user sending modified JWTs to the server in order to achieve a malicious goal. Typically, this goal is to **bypass authentication** and **access controls** by impersonating another user who has already been authenticated.

The **impact** of JWT attacks is usually **severe**. If an attacker is able to create their own valid tokens with arbitrary values, they may be able to escalate their own privileges or impersonate other users, taking full control of their accounts.

### Flawed Signature Verification

For example, consider a JWT containing the following claims:

```json
{
    "username": "carlos",
    "isAdmin": false
}
```

**Changing the parameter "isAdmin" to true : Privilege Escalation.**

JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library jsonwebtoken has **verify()** and **decode()**. **Occasionally, developers confuse these two methods and only pass incoming tokens to the decode() method. This effectively means that the application doesn't verify the signature at all.**

### Accepting Tokens with No Signature

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

**Change the "alg" value to "none". Remove the signature part but leave the trailing dot ".".**

> Even if the token is unsigned, the payload part must still be terminated with a trailing dot.

### Brute-forcing Secret Keys

Some signing algorithms, such as **HS256** (HMAC + SHA-256), use an arbitrary, standalone string as the secret key. Just like a password, it's crucial that this secret can't be easily guessed or brute-forced by an attacker. Otherwise, they may be able to create JWTs with any header and payload values they like, then use the key to re-sign the token with a valid signature.

When implementing JWT applications, **developers sometimes make mistakes** like forgetting to change default or placeholder secrets. They may even copy and paste code snippets they find online, then forget to change a hardcoded secret that's provided as an example. In this case, it can be trivial for an attacker to brute-force a server's secret using a **wordlist of well-known secrets**.

> JWT Wordlist : https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list$

**If the server uses an extremely weak secret, it may even be possible to brute-force this character-by-character rather than using a wordlist.**

#### How to

```bash
hashcat -a 0 -m 16500 $JWT $wordlist
# --show to output the result if you already run it
```

**Then :** Generate another key using JWT Editor Keys on BurpSuite, change the "k" parameter to the base64-encoded secret. Start accessing admin panels.

### Tools

#### JWT Toolkit V2

{% embed url="https://github.com/ticarpi/jwt_tool" %}

```bash
git clone https://github.com/ticarpi/jwt_tool
cd jwt_tool/
python3 -m pip install termcolor cprint pycryptodomex requests
python3 jwt_tool.py $JWT
```
