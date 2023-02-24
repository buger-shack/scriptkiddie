# NoSQL

{% hint style="info" %}
A NoSQL database refers to a **non-relational database** that is short for **non SQL** **and Not only SQL**. It is a data-storing and data-retrieving system.
{% endhint %}

### Understanding NoSQL | MongoDB

Similar to relational databases (such as MySQL and MSSQL), MongoDB consists of databases, tables, fields but with different names where

* Collections are similar to tables or views in MySQL and MSSQL.
* Documents are similar to rows or records in MySQL and MSSQL.
* Fields are similar to columns in MySQL and MSSQL.

**Brief look at operators between MongoDB and MySQL:**

```
$and equivalent to AND in MySQL
$or equivalent to OR in MySQL
$eq equivalent to = in MySQL
```

{% embed url="https://docs.mongodb.com/manual/reference/operator/query" %}

#### Interacting with a MongoDB server

* `show databases` : list all the databases
* `use <database>` : to connect to a database
* `db.createCollection()` : create new collections
* `db.getCollectionNames();` : show all available collections within the database
* `db.users.insert({id:"1", username: "admin", email: "admin@thm.labs", password: "idk2021!"})` : insert data into **users** collection
* `db.users.find()` : show available documents within the collection
* `db.users.update({id:"2"}, {$set: {username: "tryhackme"}}); || db.<collection>.update` : to update a document
* `db.users.remove()` : remove a document
* `db.users.drop()` : drop a collection
