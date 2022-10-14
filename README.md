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
