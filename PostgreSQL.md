# PostgreSQL

## Docker

### PostgreSQL Server

https://hub.docker.com/_/postgres/

```bash
#!/bin/bash
docker run --detach \
    --name postgresql \
    --hostname postgresql \
    -e POSTGRES_PASSWORD=foobar123 \
    --publish 5432:5432 \
    --restart=unless-stopped \
    --volume postgresql-data:/var/lib/postgresql/data \
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
