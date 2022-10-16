# Intro-to-sql

## Running the postgres docker container

```bash
# Only run this if you don't have the container running. It'll error otherwise
docker run -e POSTGRES_PASSWORD=lol --name=pg --rm -d -p 5432:5432 postgres:14

docker exec -u postgres -it pg psql
```

## psql commands

psql has a bunch of build in commands to help you navigate around. `\?` to show help docs.
`\l` to list databases

## default databases

`template1` is what Postgres uses by default when you create a database. If you want your databases to have a default shape, you can modify template1 to suit that.

`template0` should never be modified. If your template1 gets out of whack, you can drop it and recreate from the fresh template0.

`postgres` exists for the purpose of a default database to connect to. We're actually connected to it by default since we didn't specify a database to connect to. You can technically could delete it but there's no reason to and a lot of tools and extensions do rely on it being there.

## Create you own

```sql
CREATE DATABASE recipeguru;
```

`\c recipeguru` to connect to the database.

## Create our table

```sql
CREATE TABLE ingredients (
  id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  title VARCHAR ( 255 ) UNIQUE NOT NULL
);
```

## Adding a record

```sql
INSERT INTO ingredients (title) VALUES ('bell pepper');
```

To view the record run:

```sql
SELECT * FROM ingredients;
```

### Dropping a table

To drop a table

```sql
DROP TABLE ingredients;
```

### Alter table

Add an column to our table

```sql
ALTER TABLE ingredients ADD COLUMN image VARCHAR ( 255 );

ALTER TABLE ingredients
ADD COLUMN image VARCHAR ( 255 ),
ADD COLUMN type VARCHAR ( 50 ) NOT NULL DEFAULT 'vegetable';
```

Drop the column

```sql
ALTER TABLE ingredients DROP COLUMN image;
```

### Data types

There are so many data types in PostgreSQL that we won't get close to covering them all. [Have a peek here from the PostgreSQL docs](https://www.postgresql.org/docs/14/datatype.html) to see the full list.

### Inserting data

```sql
INSERT INTO "ingredients" (
 "title", "image", "type" -- Notice the " here
) VALUES (
  'broccoli', 'broccoli.jpg', 'vegetable' -- and the ' here
);
```

This is the standard way of doing an insert. In the first set of parens you list out the column names then in the values column you list the actual values you want to insert.

Big key here which will throw JS developers for a loop: you must use single quotes. Single quotes in SQL means "this is a literal value". Double quotes in SQL mean "this is an identifier of some variety".

`Use -- for comments`

The above query works because the double quotes are around identifiers like the table name and the column names. The single quotes are around the literal values. The double quotes above are optional. The single quotes are not.

```sql
INSERT INTO ingredients (
  title, image, type
) VALUES
  ( 'avocado', 'avocado.jpg', 'fruit' ),
  ( 'banana', 'banana.jpg', 'fruit' ),
  ( 'beef', 'beef.jpg', 'meat' ),
  ( 'black_pepper', 'black_pepper.jpg', 'other' ),
  ( 'blueberry', 'blueberry.jpg', 'fruit' ),
  ( 'broccoli', 'broccoli.jpg', 'vegetable' ),
  ( 'carrot', 'carrot.jpg', 'vegetable' ),
  ( 'cauliflower', 'cauliflower.jpg', 'vegetable' ),
  ( 'cherry', 'cherry.jpg', 'fruit' ),
  ( 'chicken', 'chicken.jpg', 'meat' ),
  ( 'corn', 'corn.jpg', 'vegetable' ),
  ( 'cucumber', 'cucumber.jpg', 'vegetable' ),
  ( 'eggplant', 'eggplant.jpg', 'vegetable' ),
  ( 'fish', 'fish.jpg', 'meat' ),
  ( 'flour', 'flour.jpg', 'other' ),
  ( 'ginger', 'ginger.jpg', 'other' ),
  ( 'green_bean', 'green_bean.jpg', 'vegetable' ),
  ( 'onion', 'onion.jpg', 'vegetable' ),
  ( 'orange', 'orange.jpg', 'fruit' ),
  ( 'pineapple', 'pineapple.jpg', 'fruit' ),
  ( 'potato', 'potato.jpg', 'vegetable' ),
  ( 'pumpkin', 'pumpkin.jpg', 'vegetable' ),
  ( 'raspberry', 'raspberry.jpg', 'fruit' ),
  ( 'red_pepper', 'red_pepper.jpg', 'vegetable' ),
  ( 'salt', 'salt.jpg', 'other' ),
  ( 'spinach', 'spinach.jpg', 'vegetable' ),
  ( 'strawberry', 'strawberry.jpg', 'fruit' ),
  ( 'sugar', 'sugar.jpg', 'other' ),
  ( 'tomato', 'tomato.jpg', 'vegetable' ),
  ( 'watermelon', 'watermelon.jpg', 'fruit' )
ON CONFLICT DO NOTHING;
```

### Update a record

```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon';
```

The WHERE clause is where you filter down what you want to update. In this case there's only one watermelon so it'll just update one but if you had many watermelons it would match all of those and update all of them

If you want to return what was updated try:

```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING id, title, image;
```

The RETURNING clause tells Postgres you want to return those columns of the things you've updated. In our case I had it return literally everything we have in the table so you could write that as

```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING *;
```
