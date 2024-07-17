Sure, here are the updated connection details along with the default credentials for each database.

# DB infos

## Connection

### MySQL

```bash
mysql -u [username] -p’[password]’ -h [host] -P [port]
```
**Default Credentials:** `root:<blank>`

### Microsoft SQL Server

{% embed url="https://www.mssqltips.com/sqlservertip/5305/installing-and-using-mssqlcli-on-linux-for-sql-server" %}
MSSQL - Installation
{% endembed %}

```bash
mssql-cli -S 'SQL instance' -U 'user id' -P 'password'
```
**Default Credentials:** `sa:<blank>`

### PostgreSQL

```bash
psql -U [username] -d [database] -h [host] -p [port]
```
**Default Credentials:** `postgres:postgres`

### Oracle

```bash
sqlplus [username]/[password]@[host]:[port]/[service_name]
```
**Default Credentials:** `<blank>:<blank>`

### phpMyAdmin

Access via web interface using URL:
```
http://[host]/phpmyadmin
```
**Default Credentials:** `root:<blank>`

### Redis

```bash
redis-cli -h [host] -p [port] -a [password]
```
**Default Credentials:** `<blank>:<blank>` (Note: Redis typically does not use username/password authentication by default, but it can be configured to use a password.)

### SQLite

```bash
sqlite3 [database_file]
```
**Default Credentials:** No default username/password. Authentication is usually managed at the file level.

### MongoDB

```bash
mongo --host [host] --port [port] -u [username] -p [password] --authenticationDatabase [database]
```
**Default Credentials:** `admin:<blank>`

### Cassandra

```bash
cqlsh [host] [port] -u [username] -p [password]
```
**Default Credentials:** `cassandra:cassandra`

### CouchDB

Access via web interface using URL:
```
http://[username]:[password]@[host]:[port]/_utils
```
**Default Credentials:** `admin:admin`

### Neo4j

```bash
cypher-shell -u [username] -p [password] -a [bolt://host:port]
```
**Default Credentials:** `neo4j:neo4j`
