---
title: SQL Injection, simplified
date: 2023-04-18
---

https://acmcsuf.com/blog/845
Hello again my ACM folks, last time we broke down XSS on a higher abstract level (https://acmcsuf.com/blog/778). Today we’re gonna go over another pretty common web vulnerability called SQL injection (https://en.wikipedia.org/wiki/SQL_injection). This is another pretty common vulnerability to keep in mind when developing applications that make use of databases.

##  What is SQL injection?
SQL injection occurs when malicious SQL is injected into your application to manipulate or access any information that isn't really intended to be shown or accessed. This occurs when attackers take advantage of SQL syntax to inject some into your site to modify queries that go on in the backend.


##  What is the impact of a successful attack?
When someone has access to querying any information in your database they can extract sensitive info you didn't intend for them to see, like user info, passwords, payment information, etc. They can also create admin logins in the database. Having admin logins pretty much lets attackers do whatever they want as if you handed over the application to them directly.

##  What types of SQL injection methods exist?
When watching out for SQL injection some of its main forms are **In-band (classic) SQLi**, **Blind SQLI** and **Out of band SQLi**.  

**<u> SQLi</u>**

+ **In Band**  

    -Error-based

    -Union-based
+ **Blind**

 -Boolean based
 
 -Time based  
+ **Out of band**



## In-band (classic) SQLi
In-band SQL refers to the attacker using the same channel to attack and gather results.
Under In-band SQLi there exists, **Error-based SQLi**, and **Union-based SQLi**.  

### Error based SQLi
is when an attacker causes the database to show error messages to lead them to a successful attack. By using these error messages they can prune and gather information on how the database is structured.

This can be as simple as trying to sqli a hyperlink to get errors
```
https://twitter.com/user.html?username=anguzz’
```
The database could then throw errors such as giving up the syntax that causes the error, and what type of database is used.

### Union-based SQLi
takes advantage of the UNION operation in SQL, and allows attackers to combine multiple select statements to gather information about the database.

Here's an example of a simple union statement that adds on multiple select statements to one query.
```
SELECT a, b from table1 UNION SELECT c, d from TABLE2
```
Something to keep in mind is that UNION statements only work when the columns have the same data types.

## Inferential (blind) SQLi
occurs when there is no HTTP response or database errors shown. Instead attackers look at how the application responds. Two ways of doing this are **Boolean based** queries and **Time based queries**.

#### Boolean-based SQLi
One way of doing this is by making use of different SQL queries that query true or false statements to tell if a database is susceptible to blind SQLi by comparing differences in response between the true and false statements.

If an attacker can modify a query to return no items then they know the page is vulnerable.

For example
```
http://www.twitter/user.php?id=123456 and 1=2
```
The SQL query would return false and the user would not be displayed.

```
http://www.twitter/user.php?id=123456 and 1=1
```
After testing something like this the SQL query would return true and the user would be displayed. By comparing the two results we know the page is vulnerable to SQLi.

#### Time based SQLi
Time based SQLi tries to make the database wait after a query is submitted. If a query takes longer to load the results we know the database is successful to SQLi.

Here's an example of a query that would make the database take longer to load.
```
SELECT * FROM users WHERE id=1; IF SYSTEM_USER=’anguzz’ WAIT FOR DELAY ’00:00:15’
```

## Out of band SQLi
Out of band SQLi occurs when the attacker does not receive a response from the application but instead is able to cause the application to send data to a remote endpoint.

For example an attacker sends a payload that causes a database to send a DNS request to a server in the attacker's control. The attacker can use the information sent to the server to carry out more attacks.

Out of band relies on the database server to make DNS or HTTP requests to send the attacker data. Different databases have different commands to do this for example, SQL server has ```xp_dirtree``` and Oracle has the ```UTL_HTTP package```



## Preventing SQLi
Let's go over some of the techniques or good practices you can follow to prevent SQLi.

Using **prepared statements** is a good practice. This is when you take user input as parameters and pass those parameters into constructed queries. You do not want to take complete user inputs as strings.

Another method is using **stored procedures** which are similar to prepared statments in that they are when sql statements are parameterized, instead though the SQL code procedure is defined and stored in the database and called by the application as needed.  

 You can also **disable SQL error messages** in your applications output to prevent giving up information on your database and making it harder for attackers.

A last good practice is to just give your database the **least privilege** it needs to run. Usually you won't have DDL statements(create/modify/alter tables) and only be running DML statements(query/edit/add/remove data). DDL statements tend to usually change the structure of the table, and this usually only happens at the creation of the database, hence you should only allow DML statements for the most part.

SQLi can get pretty complicated and this just scratches the surface to give you a basic understanding of the different types of SQLi out there. 


## Practice SQL
Practice more SQLi at https://los.rubiya.kr/ and https://portswigger.net/web-security/all-labs under SQLi.
##### Injection on NoSQL databases
Its to be noted that NoSQL databases like MongoDB, Cassandra, or Redis can also be injected.
NoSQL DBs tend to follow JavaScript Object Notation (JSON) format. More information on this can be found at https://www.imperva.com/learn/application-security/nosql-injection/