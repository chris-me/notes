# PostgreSQL

- [Docker](#docker)
  - [PostgreSQL Server](#postgresql-server)
  - [pgadmin4](#pgadmin4)
- [psql](#psql)

## Docker

### PostgreSQL Server

Set up a PostgreSQL server with forced SSL connections. Remember to change the initial passwords immediately before using.

#### Links

https://hub.docker.com/_/postgres/

https://markwoodbridge.com/2017/08/16/postgres-docker-ssl.html

#### Installation steps

Run once to create initial database files / initial postgresql-data volume:

```bash
docker run --detach \
    --name postgresql \
    --hostname postgresql \
    -e POSTGRES_PASSWORD=s3cr3t \
    --volume postgresql-data:/var/lib/postgresql/data \
    postgres:10.3
docker logs -f postgresql
docker stop postgresql
docker rm postgresql
```

Create configuration files

```bash
mkdir -p ~/postgres-config && cd ~/postgres-config
# Self signed SSL certificate
openssl req -new -text -passout pass:abcd -subj /CN=localhost -out server.req
openssl rsa -in privkey.pem -passin pass:abcd -out server.key
openssl req -x509 -in server.req -text -key server.key -out server.crt
chmod og-rwx server.key
# generate and adjust configuration files
docker run -i --rm postgres:10.3 cat /usr/share/postgresql/postgresql.conf.sample > my-postgres.conf
docker run -i --volume postgresql-data:/var/lib/postgresql/data --rm postgres:10.3 cat /var/lib/postgresql/data/pg_hba.conf > pg_hba.conf
echo "ssl = on" >> my-postgres.conf
sed -i -- 's/host all all all md5/hostssl all all all md5/g' pg_hba.conf
cd ~
```

Container run script:

```bash
#!/bin/bash
docker run --detach \
    --name postgresql \
    --hostname postgresql \
    -e POSTGRES_PASSWORD=s3cr3t \
    --publish 25432:5432 \
    --restart=unless-stopped \
    --volume ~/postgres-config/my-postgres.conf:/etc/postgresql/postgresql.conf \
    --volume ~/postgres-config/pg_hba.conf:/var/lib/postgresql/data/pg_hba.conf \
    --volume ~/postgres-config/server.key:/var/lib/postgresql/data/server.key \
    --volume ~/postgres-config/server.crt:/var/lib/postgresql/data/server.crt \
    --volume postgresql-data:/var/lib/postgresql/data \
    postgres:10.3 -c 'config_file=/etc/postgresql/postgresql.conf'
```

### pgadmin4

https://hub.docker.com/r/dpage/pgadmin4/

```bash
#!/bin/bash
docker run --detach \
    --name pgadmin4 \
    --hostname pgadmin4 \
    --link postgresql:postgresql \
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
