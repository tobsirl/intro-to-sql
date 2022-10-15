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
