
# Postgres

- [Backup / Restore (docker)](#backup-restore-docker)
- [Links](#links)
- [pqsql](#pqsql)
- [SQL Snippets](#sql-snippets)

## Backup / Restore (docker)

### Textfile (.sql)

#### Backup

```bash
docker exec -u postgres -i CONTAINER_NAME pg_dump DBNAME > db.sql
```

#### Restore

```bash
docker exec -u postgres -i CONTAINER_NAME psql -U postgres DBNAME < db.sql
```

### Binary dump

> Caution: Schema + GRANTs have to exist before restore

#### Backup

```bash
docker exec -u postgres CONTAINER_NAME pg_dump -Fc DBNAME > db.dump
```

#### Restore

```bash
# -c drops data before restore / assumes DB exists
docker exec -u postgres -i CONTAINER_NAME pg_restore -c -d restore < db.dump
```


## Links

- http://www.jancarloviray.com/blog/postgres-quick-start-and-best-practices/
- https://kukuruku.co/post/postgresql-set-of-practices/
- https://www.pgbarman.org/index.html

## pqsql

### Start psql in docker

```bash
# connect into running container:
docker-compose exec -u postgres postgres psql
# connect to remote database:
docker run -it -e PGPASSWORD=mypassword postgres:alpine psql -h myserverhostname -U postgres
```

### List databases

```bash
\l
```

### Connect to database

```bash
\c mydatabase
```

### Execute query every X seconds

```sql
SELECT pid, datname, query FROM pg_stat_activity; \watch 2
```

## SQL Snippets

## Create and grant user

```sql
CREATE DATABASE database_name;
CREATE USER my_username WITH PASSWORD 'my_password';
GRANT ALL PRIVILEGES ON DATABASE "database_name" to my_username;
```

## List active connections

```sql
SELECT * FROM pg_stat_activity;
```

## List table sizes

```sql
SELECT
   relname AS table_name,
   pg_size_pretty(pg_total_relation_size(relid)) AS total,
   pg_size_pretty(pg_relation_size(relid)) AS internal,
   pg_size_pretty(pg_table_size(relid) - pg_relation_size(relid)) AS external,
   pg_size_pretty(pg_indexes_size(relid)) AS indexes
FROM pg_catalog.pg_statio_user_tables ORDER BY pg_total_relation_size(relid) DESC
```
