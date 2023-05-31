# Configuration on MariaDB

### Wireshark capture while connecting to MariaDB

![Username and DB name in clear + hashed password](<../../.gitbook/assets/image (106).png>)

![Hashed password](<../../.gitbook/assets/image (108).png>)

### Step 1 : Protect MariaDB/MySQL

```shell
$ mysql_secure_installation
```

* Set a **strong password** for the root user.
* Remove access to the user **Anonymous**.
* We allow only the connection to the root user from **localhost**.
* We delete the database '**test**' to which all users could access.
* Finally, we **reload** the table of **privileges**.

### Step 2 : Configuration SSL/TLS

#### Create CA certificate CA

```bash
# Create a directory for the key 
cd /etc/mysql
mkdir ssl
cd ssl

# Create a new CA key
openssl genrsa 2048 > ca-key.pem

# Generate the certificate using the key
openssl req -new -x509 -nodes -days 365000 -key ca-key.pem -out ca-cert.pem
```

![](<../../.gitbook/assets/image (90).png>)

Indeed, we have 2 files:

1. _/etc/mysql/ssl/ca-cert.pem_ - Certificate file for the certification authority (CA).
2. _/etc/mysql/ssl/ca-key.pem_ - Key file for the certification authority (CA).

#### Create the server's SSL/TLS certificate

```bash
# Create the server key
openssl req -newkey rsa:2048 -days 365000 -nodes -keyout server-key.pem -out server-req.pem
```

```bash
# Server RSA key
openssl rsa -in server-key.pem -out server-key.pem

# Sign the certificate
openssl x509 -req -in server-req.pem -days 365000 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out server-cert.pem
```

We have 2 additional files:

1. _/etc/mysql/ssl/server-cert.pem_ - MariaDB server certificate file.
2. _/etc/mysql/ssl/server-key.pem_ - File of key of the MariaDB server.

#### Create the SSL/TLS certificate for the MySQL client

```bash
openssl req -newkey rsa:2048 -days 365000 -nodes -keyout client-key.pem -out client-req.pem
```

```bash
# Client's RSA key
openssl rsa -in client-key.pem -out client-key.pem

# Sign the client's certificate
openssl x509 -req -in client-req.pem -days 365000 -CA ca-cert.pem -CAkey ca-key.pem -set_serial 01 -out client-cert.pem
```

#### Verification of certificates

```bash
openssl verify -CAfile ca-cert.pem server-cert.pem client-cert.pem
# it should return 'OK'
```

#### Apply SSL/TLS to MariaDB

Modify the MariaDB configuration file and add :

![](<../../.gitbook/assets/image (31).png>)

```bash
# Securing the keys
chown -Rv mysql:root /etc/mysql/ssl/

# Restart MariaDB
systemctl restart mysql

# Look at the logs (errors)
grep ssl /var/log/syslog
grep ssl /var/log/syslog | grep key
grep mysqld /var/log/syslog | grep -i ssl

```

Configure the MariaDB client to use SSL (add in the client configuration file /etc/mysql/mariadb.conf.d/50-mysql-clients.cnf) :

![](<../../.gitbook/assets/image (42).png>)

Copy /etc/mysql/ssl/ca-cert.pem, /etc/mysql/ssl/client-cert.pem, and /etc/mysql/ssl/client-key.pem to all clients. Here, we only have one user so it's not useful.

**Verification**

```bash
mysql -u foo -h 127.0.0.1 -p bank
```

SSL configuration worked well on MariaDB :

```bash
MariaDB [(bank)]> SHOW VARIABLES LIKE '%ssl%';
MariaDB [(bank)]> status;
```

![](<../../.gitbook/assets/image (19).png>)

![](<../../.gitbook/assets/image (47).png>)

### 4. Two-Way TLS

Logging in as **root**, we remove the old privileges of the user **foo** :

![](<../../.gitbook/assets/image (5) (1).png>)

New ones are created, allowing the client to verify the server certificate and the server to verify the client certificate:

```sql
GRANT SELECT
ON bank.comptes_stats
TO 'foo'@'localhost' REQUIRE X509;

GRANT SELECT
ON bank.prets_stats
TO 'foo'@'localhost' REQUIRE X509;
```

Check by logging in as a **foo** user:

![](<../../.gitbook/assets/image (46).png>)

### 5. Perform a flow capture with Wireshark of a mutual-auth TLS encrypted connection to the database, identify the key exchanges

![](<../../.gitbook/assets/image (44).png>)

1 : The first TLS packet is "**Client Hello**". The client lists the SSL/TLS versions and cipher suites it is able to use (here 31 suites) :

![](<../../.gitbook/assets/image (6).png>)

2 : Then a "**Server Hello**" packet arrives. The server consults the list of SSL/TLS versions and cipher suites and chooses the most recent one it can use. Then the server sends a message to the client containing the SSL/TLS version and the cipher suite it has chosen :

![](<../../.gitbook/assets/image (140).png>)

**3 : Exchange of keys** Once the server and the client have agreed on the SSL/TLS version and the number sequence, the server sends two elements. The first is its SSL/TLS certificate to the client. The client (web browser) validates the server's certificate. Web browsers store a list of Root CAs within them. These root CAs are third parties that web browsers trust. The server's certificate is issued by a Root CA or an Immediate CA. The immediate CA is a CA that the root CA trusts.

On the Wireshark capture, we do not see any **Server Key Exchange** or **Client Key Exchange** packets. However the encryption is in place and we receive the **Change Cipher Spec** packet.

**Change Cipher Spec**

The Change Cipher Spec message is sent by the client and server to notify the receiving party that the following records will be protected by the cipher specification and keys that were just negotiated.

![](<../../.gitbook/assets/image (76).png>)

## Appendices

[**MySQL Traffic on Wireshark**](https://stackoverflow.com/questions/48477121/wireshark-password-capture-of-mysql-traffic)

[**Capture passwords using Wireshark**](https://www.infosecmatter.com/capture-passwords-using-wireshark/)

[**SSL/TLS Setup**](https://www.cyberciti.biz/faq/how-to-setup-mariadb-ssl-and-secure-connections-from-clients/)

[**Two-Way TLS**](https://mariadb.com/docs/clients/mariadb-connectors/connector-j/tls/)

[**SSL/TLS Handshake**](https://www.linuxbabe.com/security/ssltls-handshake-process-explained-with-wireshark-screenshot)
