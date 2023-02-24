# ✅ Best Practices

{% embed url="https://vladtoie.gitbook.io/secure-coding" %}
Secure Coding Handbook
{% endembed %}

## Misc

* [ ] Journaling code events (logs)
* [ ] Unit tests
* [ ] ORM (like SQLAlchemy)
* [ ] Documented functions (good names)
* [ ] Versioning (like SVN)
* [ ] Comments

## Checklist

### Input Validation

* [ ] Do not trust input, consider centralized input validation.
* [ ] Do not rely on client-side validation.
* [ ] Be careful with canonicalization issues.
* [ ] Constrain, reject, and sanitize input. Validate for type, length, format, and range.

### Authentication

* [ ] Partition site by anonymous, identified, and authenticated area.
* [ ] Use strong passwords.
* [ ] Support password expiration periods and account disablement.
* [ ] Do not store credentials (use one-way hashes with salt).
* [ ] Encrypt communication channels to protect authentication tokens.
* [ ] Pass forms authentication cookies only over HTTPS connections.
* [ ] Use of ORM (SQLAlchemy for instance)

### Authorization

* [ ] Use least-privileged accounts.
* [ ] Consider authorization granularity.
* [ ] Enforce separation of privileges.
* [ ] Restrict user access to system-level resources.
* [ ] Use OAuth 2.0 protocol for Authentication and Authorization.
* [ ] Carryout API Validation.
* [ ] Whitelist allowable methods.
* [ ] Protect privileged actions and sensitive resource collections.
* [ ] Protect against Cross-site resource forgery (CSRF).

### Session Management

* [ ] Create a Session identifier on the server.
* [ ] Terminate the session with the Logoff.
* [ ] Generate a new session on re-authentication.
* [ ] Set the ‘secure’ attribute for cookies transmitted over TLS.

### Cryptography

* [ ] Use cryptography while ‘Data in transit, Data in storage, Data in motion, Message Integrity’.
* [ ] Do not develop your own. Use tried and tested platform features.
* [ ] Keep unencrypted data close to the algorithm.
* [ ] Use the right algorithm and key size.
* [ ] Avoid key management (use DPAPI).
* [ ] Cycle your keys periodically.
* [ ] Store keys in a restricted location.

### Logging and Auditing

* [ ] Identify malicious behavior.
* [ ] Know what good traffic looks like.
* [ ] Audit and log activity through all of the application tiers.
* [ ] Secure access to log files.
* [ ] Back up and regularly analyze the log files.

### Output Encoding

* [ ] Carryout ‘Input Validation (XML, JSON….).
* [ ] Use Parameterized query.
* [ ] Carry out ‘Schema validation’.
* [ ] Carry out Encoding (XML, JSON..).
* [ ] Send Security Headers.
