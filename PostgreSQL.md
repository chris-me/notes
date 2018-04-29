# PostgreSQL

- [Docker](#docker)
  - [PostgreSQL Server](#postgresql-server)
  - [pgadmin4](#pgadmin4)
- [psql](#psql)

## Docker

### PostgreSQL Server w/ SSL (forced)

#### Links

https://hub.docker.com/_/postgres/
https://markwoodbridge.com/2017/08/16/postgres-docker-ssl.html

#### Installation steps

```bash
mkdir -p ~/postgres-config && cd ~/postgres-config
# Create self signed SSL certificate
openssl req -new -text -passout pass:abcd -subj /CN=localhost -out server.req
openssl rsa -in privkey.pem -passin pass:abcd -out server.key
openssl req -x509 -in server.req -text -key server.key -out server.crt
chmod og-rwx server.key
```


Container run script:

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
docker run --detach \
    --name pgadmin4 \
    --hostname pgadmin4 \
    --volume pgadmin-data:/var/lib/pgadmin \
    -p 8080:80 \
    -e "PGADMIN_DEFAULT_EMAIL=foo@bar.com" \
    -e "PGADMIN_DEFAULT_PASSWORD=foobar123" \
    dpage/pgadmin4
```

## psql


List databases

```bash
\l
```

Connect to a database

```bash
\c mydatabase
```
