# ⭐ JWT

## Brief

JSON web tokens (JWTs) are a standardized format for sending cryptographically signed JSON data between systems. They can theoretically contain any kind of data, but are most commonly used to send information ("claims") about users as part of authentication, session handling, and access control mechanisms.

Unlike with classic session tokens, all of the data that a server needs is stored client-side within the JWT itself. This makes JWTs a popular choice for highly distributed websites where users need to interact seamlessly with multiple back-end servers.

A JWT consists of **3 parts**: a header, a payload, and a signature. These are each separated by a dot, as shown in the following example:&#x20;

<figure><img src="../../../.gitbook/assets/image (123).png" alt=""><figcaption><p>JWT example</p></figcaption></figure>

The header and payload parts of a JWT are just **base64url-encoded** JSON objects. The header contains metadata about the token itself, while the payload contains the actual "claims" about the user. For example, you can decode the payload from the token above to reveal the following claims:

```json
{
    "iss": "portswigger",
    "exp": 1648037164,
    "name": "Carlos Montoya",
    "sub": "carlos",
    "role": "blog_author",
    "email": "carlos@carlos-montoya.net",
    "iat": 1516239022
}
```

In most cases, this data can be easily read or modified by anyone with access to the token. Therefore, the security of any JWT-based mechanism is heavily reliant on the **cryptographic signature**.

### Signature part

The server that issues the token typically generates the signature by **hashing the header** and **payload**. In some cases, they also encrypt the resulting hash. Either way, this process involves a **secret signing key**. This mechanism provides a way for servers to verify that none of the data within the token has been tampered with since it was issued:

* As the signature is directly derived from the rest of the token, changing a single byte of the header or payload results in a mismatched signature.
* Without knowing the server's secret signing key, it shouldn't be possible to generate the correct signature for a given header or payload.

### Particularities

> In other words, a JWT is usually either a JWS or JWE token. When people use the term "JWT", they almost always mean a JWS token. JWEs are very similar, except that the actual contents of the token are encrypted rather than just encoded.
