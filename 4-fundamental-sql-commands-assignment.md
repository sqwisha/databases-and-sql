> 1. List the commands for adding, updating, and deleting data.

- INSERT INTO

- UPDATE

- DELETE

> 2. Explain the structure for each type of command.
````sql
CREATE TABLE table_name (
  col_name data_type,
  col_name data_type
);

INSERT INTO table_name (col_name, col_name)
VALUES (val, val);

UPDATE table_name
SET col_name = val
WHERE condition;

DELETE FROM table_name
WHERE condition (optional AND condition);

ALTER TABLE table_name ADD COLUMN col_name data_type;

DROP TABLE table_name;
````

> 3. What are some of the data types that can be used in tables? Give a real-world example of each type.

- **integer** - such as age, height in inches, street number
- **text** - such as street name, title, description, message
- **timestamp** - such as when a purchase was made, when a comment was posted
- **boolean** - such as whether a member has access to parts of a site, or whether they opt-out of promotional emails
- **enum** - such as days of the week
- **json** - such as a response from an API

> 4. Decide how to create a new table to hold a list of people invited to a wedding dinner. The table needs to have first and last names, whether they sent in their RSVP, the number of guests they are bringing, and the number of meals (1 for adults and 1/2 for children).
  > * Which data type would you use to store each of the following pieces of information?
  > - First and last name.

text

  > - Whether they sent in their RSVP.

boolean

  > - Number of guests.

integer

  > - Number of meals.

decimal or numeric

  > * Write a command that creates the table to track the wedding dinner.

````sql
CREATE TABLE wedding_dinner (
  first_name text,
  last_name text,
  sent_rsvp boolean,
  num_guests integer,
  num_meals decimal(3, 1)
);
````

  > * Write a command that adds a column to track whether the guest sent a thank you card.

````sql
ALTER TABLE wedding_dinner ADD COLUMN sent_thank_you boolean;
````

  > * You have decided to move the data about the meals to another table, so write a command to remove the column storing the number meals from the wedding table.

````sql
ALTER TABLE wedding_dinner DROP COLUMN num_meals;
````

  > * The guests will need a place to sit at the reception, so write a command that adds a column for table number.

````sql
ALTER TABLE wedding_dinner ADD COLUMN table_number integer;
````

  > * The wedding is over and we do not need to keep this information, so write a command that deletes the table numbers from the database.
````sql
ALTER TABLE wedding_dinner DROP COLUMN table_number;
````

> 5. Write a command to create a new table to hold the books in a library with the columns ISBN, title, author, genre, publishing date, number of copies, and available copies.

````sql
CREATE TABLE books (
  ISBN integer,
  title text,
  author text,
  genre text,
  published date,
  num_copies integer,
  available_copies integer
);
````

  > * Find three books and add their information to the table.

````sql
INSERT INTO books
VALUES
  (0399555773, 'Skyward', 'Brandon Sanderson', 'Science Fiction', 'November 6, 2018', 10, 4),
  (9781599909394, 'Throne of Glass', 'Sarah J. Maas', 'Fantasy', 'August 7, 2012', 20, 9),
  (9781429959810, 'The Eye of the World', 'Robert Jordan', 'Epic Fantasy', 'September 15, 2000', 17, 3);
````

  > * Someone has just checked out one of the books. Change the number of available copies to 1 fewer.

````sql
UPDATE TABLE books
SET available_copies = available_copies - 1
WHERE title = 'Throne of Glass';
````

  > * Now one of the books has been added to the banned books list. Remove it from the table.
````sql
DELETE FROM books
WHERE title = 'Skyward' AND author = 'Brandon Sanderson';
````

> 6. Write a command to make a new table to hold spacecrafts. Information should include id, name, year launched, country of origin, a brief description of the mission, orbiting body, if it is currently operating, and its approximate miles from Earth. In addition to the table creation, provide commands that perform the following operations:

````sql
CREATE TABLE spacecrafts (
  id integer,
  name text,
  year_launched integer,
  country_of_origin text,
  mission text,
  orbiting_body text,
  operating boolean,
  miles_from_earth integer
);
````

  > * Add three non-Earth-orbiting satellites to the table.

````sql
INSERT INTO spacecrafts (id, name, year_launched, country_of_origin, mission, orbiting_body, operating, miles_from_earth)
VALUES
(30580, 'THEMIS', 2007, 'USA', 'Magnetospheric research', 'Moon', TRUE, 238900),
(36576, 'Akatsuki', 2010, 'Japan', 'Study atmosphere of Venus', 'Venus', TRUE, 162177882),
(39370, 'Mangalyaan', 2013, 'India', 'Interplanetary operations preparation', 'Mars', TRUE, 33926867);
````

  > * Remove one of the satellites from the table since it has just crashed into the planet.

````sql
DELETE FROM spacecrafts
WHERE id = 36576;
````

  > * Edit another satellite because it is no longer operating and change the value to reflect that.

````sql
UPDATE TABLE spacecrafts
SET operating = FALSE
WHERE id = 30580;
````
> 7. Write a command to create a new table to hold the emails in your inbox. This table should include an id, the subject line, the sender, any additional recipients, the body of the email, the timestamp, whether or not you have read the email, and the id of the email chain it's in. Also provide commands that perform the following operations:

````sql
CREATE TABLE inbox (
  id integer,
  subject text,
  sender text,
  other_recipients text,
  body text,
  time TIMESTAMP,
  read boolean,
  chain_id integer
);
````

  > * Add three new emails to the inbox.

````sql
INSERT INTO inbox
VALUES
(198, 'Nullam convallis accumsan faucibus ridiculus.', 'Deac.HAMILTO4018@mailinator.com', 'Mck.MERC5939@mailinator.com', 'Lorem ipsum laoreet feugiat odio cubilia etiam ligula pharetra cras.', TIMESTAMP '10/06/2008 18:09:04', FALSE, 23),
(184, 'Orci condimentum vulputate pretium lorem.', 'Irv.KEN8735@mailinator.com', NULL, 'Lorem ipsum sit sollicitudin interdum taciti non purus tincidunt vel tortor?', TIMESTAMP '12/26/2008 09:23;05', TRUE, 42),
(209,'Sollicitudin, conubia odio suscipit. Litora.','Lucia.HORT9761@yopmail.com', 'Jeremia.COLL9649@dispostable.com','Lorem ipsum primis, ornare condimentum viverra placerat fames fames mauris lacus lectus morbi.', TIMESTAMP '12/08/2008 12:52:01', TRUE, NULL);

````

  > * You deleted one of the emails, so write a command to remove the row from the inbox table.

````sql
DELETE FROM inbox
WHERE id = 198;
````

  > * You started reading an email but just heard a crash in another room. Mark the email as unread before investigating the crash, so you can come back and read it later.

````sql
UPDATE inbox
SET read = FALSE
WHERE id = 184;
````