> 1. How do you find related data held in two separate data tables?

To find related data in two separate data tables, use a `JOIN` statement.

> 2. Explain, in your own words, the difference between an `INNER JOIN`, `LEFT OUTER JOIN`, and `RIGHT OUTER JOIN`. Give a real-world example for each.

`INNER JOIN` - returns all the queried rows from the first table, plus the rows matching the condition from the second table.

  **Example**: I want a list of all parks in the city of Pawnee, Indiana that have ponds.

````sql
SELECT PawneeParks.name, PawneeParks.address
FROM PawneeParks
JOIN IndianaParksWithPonds
ON PawneeParks.id = IndianaParksWithPonds.id;
````

`LEFT OUTER JOIN` - returns rows matching the condition in both tables, plus all queried rows in the first table.

  Example: I want a list of all territories in Tiwan with Chinese-disputed territories' total land area indicated.

````sql
SELECT t.landmass_name, c.area
FROM tiwan_territory AS t
LEFT OUTER JOIN china_territory AS c
ON t.landmass_name = c.landmass_name;
````

`RIGHT OUTER JOIN` - returns rows matching the condition in both tables, plus all queried rows the second table.

  Example: I want to find all vegan-friendly recipes from my Traditional Cookbook, plus all recipes from my Vegan Cookbook.
````sql
SELECT traditional_cookbook.recipe, vegan_cookbook.recipe
FROM traditional_cookbook
RIGHT OUTER JOIN vegan_cookbook
ON traditional_cookbook.dietary_restrictions = 'vegan';
````

> 3. Define primary key and foreign key. Give a real-world example for each.

**Primary Key**: a unique identifier for each row in a table

  *Example*: A doctor's office may have a list of patients and their primary keys are their social security numbers

**Foreign Key**: an identifier matching the primary key of another table so that they can be `JOIN`ed

  *Example*: The doctor's office has a list of prescriptions administered, and each row has the patient's social security number associated with it.

> 4. Define aliasing.

Aliasing is the practice of making short variables, usually one letter, to reference tables in queries.

> 5. Change this query so that you are using aliasing:
> ```sql
>  SELECT professor.name, compensation.salary,
>  compensation.vacation_days FROM professor JOIN
>  compensation ON professor.id =
>  compensation.professor_id;
>  ```

 ```sql
  SELECT p.name, c.salary, c.vacation_days
    FROM professor AS p
    JOIN compensation AS c
      ON p.id = c.professor_id;
  ```

> 6. Why would you use a `NATURAL JOIN`? Give a real-world example.

`NATURAL JOIN` is used when you want data from two tables that only have one column name in common.

  *Example*: I have a table listing flowers I stock at my flower shop containing an ID for their supplier, and I have another table of suppliers containing the same IDs, I can `NATURAL JOIN` the tables to get a comprehensive list of my flowers and information for suppliers.

> 7. Using [this Employee schema and data](https://www.db-fiddle.com/f/sG1TKgR15GhH8cjbAwzjAm/0), write queries to find the following information:
>  - List all employees and all shifts.

````sql
SELECT e.name, s.date, s.start_time, s.end_time
  FROM employees AS e
  JOIN scheduled_shifts AS sch
    ON sch.employee_id = e.id
  JOIN shifts AS s
    ON sch.shift_id = s.id
  ORDER BY s.date, s.start_time;
````

> 8. Using [this Adoption schema and data](https://www.db-fiddle.com/f/tpodLv3A43VL4gHqohqx2o/0
), please write queries to retrieve the following information and include the results:
>  - Create a list of all volunteers. If the volunteer is fostering a dog, include each dog as well.

````sql
SELECT v.first_name, v.last_name, d.name AS foster_dog
  FROM volunteers AS v
  LEFT OUTER JOIN dogs AS d
    ON v.foster_dog_id = d.id;
````

>  - The cat's name, adopter's name, and adopted date for each cat adopted within the past month to be displayed as part of the &quot;Happy Tail&quot; social media promotion which posts recent successful adoptions.

````sql
SELECT c.name, a.first_name, a.last_name, ca.date
  FROM adopters AS a
  JOIN cat_adoptions AS ca
    ON a.id = ca.adopter_id
  JOIN cats AS c
    ON ca.cat_id = c.id
  WHERE ca.date > NOW() - INTERVAL '1 month';
````

>  - Create a list of adopters who have not yet chosen a dog to adopt.

````sql
SELECT a.*
  FROM adopters AS a
  JOIN dog_adoptions AS d
    ON d.adopter_id != a.id;
````

>  - Lists of all cats and all dogs who have not been adopted.

````sql
SELECT c.id, c.name, c.gender, c.age
  FROM cats AS c
  LEFT JOIN cat_adoptions AS ca
    ON ca.cat_id = c.id
  WHERE ca.cat_id IS NULL
UNION
SELECT d.id, d.name, d.gender, d.age
  FROM dogs AS d
  JOIN dog_adoptions AS da
    ON d.id != da.dog_id;
-- I expected both queries to look exactly the same
-- except "cat" and "dog" being swapped, but when I
-- ran the cat query it returned several repeating
-- rows of all the cats (see results from DB Fiddle
-- below). I tried different solutions but the only
-- way it would work for cats was with a left excluding
-- join. I honestly don't understand why '!=' worked
-- for dogs and not for cats.

-- Here's another implementation that might be better
SELECT c.id, c.name, c.gender, c.age
  FROM cats AS c
  WHERE NOT EXISTS (
    SELECT 'x' FROM cat_adoptions AS ca
      WHERE ca.cat_id = c.id
  )
UNION
SELECT d.id, d.name, d.gender, d.age
  FROM dogs AS d
  WHERE NOT EXISTS (
    SELECT 'x' FROM dog_adoptions AS da
      WHERE da.dog_id = d.id
  );
````
---
### Example of cats query output from DB Fiddle:
**Query #1**
````sql
  SELECT c.id, c.name, c.gender, c.age
  FROM cats AS c
  JOIN cat_adoptions AS ca
  ON c.id != ca.cat_id;
````

| id  | name     | gender | age |
| --- | -------- | ------ | --- |
| 1   | Mushi    | M      | 1   |
| 2   | Seashell | F      | 7   |
| 4   | Victoire | M      | 7   |
| 5   | Nala     | F      | 1   |
| 2   | Seashell | F      | 7   |
| 3   | Azul     | M      | 3   |
| 4   | Victoire | M      | 7   |
| 5   | Nala     | F      | 1   |
| 1   | Mushi    | M      | 1   |
| 2   | Seashell | F      | 7   |
| 3   | Azul     | M      | 3   |
| 5   | Nala     | F      | 1   |

---

>  - The name of the person who adopted Rosco.

````sql
SELECT a.first_name, a.last_name
  FROM adopters AS a
  JOIN dog_adoptions AS da
    ON a.id = da.adopter_id
  JOIN dogs AS d
    ON d.name = 'Rosco';
````

> 9. Using [this Library schema and data](https://www.db-fiddle.com/f/j4EGoWzHWDBVtiYzB9ygC4/0), write queries applying the following scenarios and include the results:
>  - To determine if the library should buy more copies of a given book, please provide the names and position, in order, of all of the patrons with a hold (request for a book with all copies checked out) on "Advanced Potion-Making".

````sql
SELECT p.name, h.rank
  FROM patrons AS p
  JOIN holds AS h
    ON p.id = h.patron_id
  JOIN books AS b
    ON h.isbn = b.isbn
  WHERE b.title = 'Advanced Potion-Making'
  ORDER BY h.rank;
````

>  - List all of the library patrons. If they have one or more books checked out, list the books with the patrons.

````sql
SELECT p.name, b.title
  FROM patrons AS p
  LEFT OUTER JOIN transactions AS t
    ON p.id = t.patron_id
  JOIN books AS b
    ON t.isbn = b.isbn
  ORDER BY p.name;
````