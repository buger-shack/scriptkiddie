# SSL/TLS

### HandShake Process

![](<../../.gitbook/assets/image (79).png>)

1. The SSL or TLS client sends a “**Client Hello**” message that **lists cryptographic information** such as the SSL or TLS **version** and, in the client's order of preference, the CipherSuites supported by the client. The message also contains a random byte string that is used in subsequent computations. The protocol allows for the “client hello” to include the data compression methods supported by the client.
2. The SSL or TLS server responds with a “**Server Hello**” message that contains the **CipherSuite chosen by the server** from the list provided by the client, the session ID, and another random byte string. The server also sends its digital certificate. If the server requires a digital certificate for client authentication, the server sends a “client certificate request” that includes a list of the types of certificates supported and the Distinguished Names of acceptable Certification Authorities (CAs).
3. The SSL or TLS client verifies the **server's digital certificate**.&#x20;
4. The SSL or TLS client sends the random byte string that enables both the client and the server to compute the secret key to be used for encrypting subsequent message data. The random byte string itself is encrypted with the server's public key.
5. If the SSL or TLS server sent a “**Client Certificate Request**”, the client sends a random byte string encrypted with the client's private key, together with the client's digital certificate, or a “no digital certificate alert”. This alert is only a warning, but with some implementations the handshake fails if client authentication is mandatory.
6. The SSL or TLS server verifies the **client's certificate**.&#x20;
7. The SSL or TLS **client** sends the server a “F**inished**” message, which is encrypted with the secret key, indicating that the client part of the handshake is complete.
8. The SSL or TLS **server** sends the client a “**Finished**” message, which is encrypted with the secret key, indicating that the server part of the handshake is complete.
9. For the duration of the SSL or TLS session, the server and client can now exchange messages that are symmetrically encrypted with the shared secret key.
