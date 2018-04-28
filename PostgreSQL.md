# PostgreSQL

## Docker

### PostgreSQL Server

https://hub.docker.com/_/postgres/

```bash
#!/bin/bash
docker run --name postgres01 \
    -e POSTGRES_PASSWORD=foobar \
    --publish 5432:5432 \
    --detach \
    postgres
```

### pgadmin4

https://hub.docker.com/r/dpage/pgadmin4/

```bash
#!/bin/bash
docker run --name pgadmin4 \
    -p 8080:80 \
    -e "PGADMIN_DEFAULT_EMAIL=foo@bar.com" \
    -e "PGADMIN_DEFAULT_PASSWORD=foobar" \
    --detach \
    dpage/pgadmin4
```
