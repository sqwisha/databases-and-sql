## Operators in Select Statements
> 1. Write out a generic `SELECT` statement.

````sql
SELECT column
FROM table
WHERE condition;
````

> 2. Create a fun way to remember the order of operations in a `SELECT` statement, such as a [mnemonic](https://en.wikipedia.org/wiki/Mnemonic).

I didn't come up with this but I like this mnemonic :

- Sweaty
- Feet
- Will
- Give
- Horrible
- Odours

for

- SELECT
- FROM
- WHERE
- GROUP BY
- HAVING
- ORDER BY

> 3. Given this [`dogs`](https://www.db-fiddle.com/f/kvD6xZQ14vRbpPe5muA92L/0) table, write queries to select the following pieces of data:
> Intake teams typically guess the breed of shelter dogs, so the `breed` column may have multiple words (for example, "Labrador Collie mix").

  > - Display the name, gender, and age of all dogs that are part Labrador.

````sql
SELECT name, gender, age
FROM dogs
WHERE breed LIKE '%labrador%';
````

  > - Display the ids of all dogs that are under 1 year old.

````sql
SELECT id
FROM dogs
WHERE age < 1;
````

  > - Display the name and age of all dogs that are female and over 35lbs.

````sql
SELECT name, age
FROM dogs
WHERE gender = 'F' AND weight > 35;
````

  > - Display all of the information about all dogs that are not Shepherd mixes.

````sql
SELECT *
FROM dogs
WHERE breed NOT LIKE '%shepherd%';
````

  > - Display the id, age, weight, and breed of all dogs that are either over 60lbs or Great Danes.

````sql
SELECT id, age, weight, breed
FROM dogs
WHERE weight > 60 OR breed = 'great dane';
````

> 4. Given this [`cats`](https://www.db-fiddle.com/f/55ePhx9NLn7PzdGnebFaG7/0) table, what records are returned from these queries?

  > - `SELECT name, adoption_date FROM cats;`

All names and adoption dates in the `cats` table.

| name     | adoption_date            |
| -------- | ------------------------ |
| Mushi    | 2016-03-22T00:00:00.000Z |
| Seashell |                          |
| Azul     | 2016-04-17T00:00:00.000Z |
| Victoire | 2016-09-01T00:00:00.000Z |
| Nala     |                          |

  > - `SELECT name, age FROM cats;`

All names and ages in `cats` table

| name     | age |
| -------- | --- |
| Mushi    | 1   |
| Seashell | 7   |
| Azul     | 3   |
| Victoire | 7   |
| Nala     | 1   |

> 5. From the `cats` table, write queries to select the following pieces of data.
  > - Display all the information about all of the available cats.

````sql
SELECT *
FROM cats
WHERE adoption_date IS NULL;
````

  > - Display the name and sex of all cats who are 7 years old.

````sql
SELECT name, gender
FROM cats
WHERE age = 7;
````


  > - Find all of the names of the cats, so you don’t choose duplicate names for new cats.

````sql
SELECT name FROM cats;
````

> 6. List each comparison operator and explain when you would use it. Include a real world example for each.

- `>`, find purchases over $100
- `<`, find users under age 50
- `>=`, find students with at least a C grade
- `<=`, find music less than or equal to 5 years old
- `=`, find exactly the book with a given title
- `!=` or `<>`, find any color drapes except green
- `LIKE`, find movies with the word "Shark" in the title

_Special Constructs_
- `BETWEEN`, (inclusive of values given) find antiques between $30 and $110
- `NOT BETWEEN`, (exclusive of values given) find antiques under $30 and over $110
- `IS NULL`, find all foster cats who haven't been named
- `IS NOT NULL`, find all users with friends
- `IS UNKNOWN`, same as `IS NULL` for boolean types
- `IS NOT UNKNOWN`, same as `IS NOT NULL` for boolean types
- `IS DISTINCT FROM`, same as `!=` or `<>`
- `IS NOT DISTINCT FROM` same as `=`

> 7. From the `cats` table, what data is returned from these queries?
  > - `SELECT name FROM cats WHERE gender = 'F';`

All names of female cats

| name     |
| -------- |
| Seashell |
| Nala     |

  > - `SELECT name FROM cats WHERE age <> 3;`

All names of cats that are not 3 years old

| name     |
| -------- |
| Mushi    |
| Seashell |
| Victoire |
| Nala     |


  > - `SELECT ID FROM cats WHERE name != ‘Mushi’ AND gender = ‘M’;`

ID of all cats not named "Mushi" and who are male.

| id  |
| --- |
| 3   |
| 4   |


