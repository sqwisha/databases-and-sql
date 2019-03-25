## Databases: Introduction Assignment

> 1. What data types do each of these values represent?
>    1. "A Clockwork Orange"
>    2. 42
>    3. 09/02/1945
>    4. 98.7
>    5. $15.99

1. String / TEXT
2. Integer / SMALLINT
3. Date
4. Float
5. Float / money(?)

> 2. Explain when a database would be used. Explain when a text file would be used.

A database would be used when we need data to persist outside an active program, and when we need the data to be manipulatable and retrievable potentially by multiple programs at the same time.

A text file would be used if data was needed/updated only by you locally.

> 3. Describe one difference between SQL and other programming languages.

SQL is a declarative programming language rather than a procedural one. Meaning that you're telling the program what to do rather than how to do it.

> 4. In your own words, explain how the pieces of a database system fit together at a high level.

A database is structured in tables. The tables consists of columns, all naming their own category of data. Each set of data has its own row containing the appropriate information for each column inside that column's cell.

> 5. Explain the meaning of **table**, **row**, **column**, and **value**.

A **table** is holds an organized set of data made up of columns, rows, and values. The data is separated into categories with **columns**. Each **row** represents a set of data, which has **values** in each cell corresponding to the columns.

> 6. List three data types that can be used in a table.

1. Strings
2. Integers
3. Dates

> 7. Given this [`payments`](https://www.db-fiddle.com/f/5gVGFmB8Aq66SejCFEbfdd/0) table, provide an English description of the following queries and include their results:

```sql
  SELECT date, amount
  FROM payments;

  SELECT amount
  FROM payments
  WHERE amount > 500;

  SELECT *
  FROM payments
  WHERE payee = 'Mega Foods';
```

1. "I need all payment dates and amounts"

| date       | amount  |
| :--------- | :------ |
| 2016-05-01 | 1500.00 |
| 2016-05-10 | 37.00   |
| 2016-05-15 | 124.93  |
| 2016-05-10 | 54.72   |

2. "I need all the payment amounts above 500"

|amount|
|:---|
|1500.00|
|37.00|
|124.93|
|54.72|

3. "I need to see all the information about payments to Mega Foods"

| date       | payee      | amount | memo      |
| :--------- | :--------- | :----- | :-------- |
| 2016-05-15 | Mega Foods | 124.93 | Groceries |



> 8. Given this [`users`](https://www.db-fiddle.com/f/iQAEYktwysXqcLQHv2dwbc/0) table, write SQL queries using the following criteria and include the output:
    * The email and sign-up date for the user named DeAndre Data.
    * The user ID for the user with email 'aleesia.algorithm@uw.edu'.
    * All the columns for the user ID equal to 4.


1:
````sql
SELECT email, signup
FROM users
WHERE name = 'DeAndre Data';
````

Output:

| email             | signup     |
| :---------------- | :--------- |
| datad@comcast.net | 2008-01-20 |


2:
````sql
SELECT userid
FROM users
WHERE email = 'aleesia.algorithm@uw.edu';
````

Output:

| userid |
| :----- |
|    1   |

3:
````sql
SELECT *
FROM users
WHERE userid = 4;
````

Output:

| userid | name           | email             | signup     |
| :----- | :------------- | :---------------- | :--------- |
| 4      | Brandy Boolean | bboolean@nasa.gov | 1999-10-15 |