# PostgreSQL

- [Docker](#docker)
  - [PostgreSQL Server](#postgresql-server)
    - [Backup / Restore](#backup--restore)
  - [pgadmin4](#pgadmin4)
- [psql](#psql)
- [Snippets](#snippets)
  - [Users / Permissions](#users--permissions)

## Docker

### PostgreSQL Server

Set up a PostgreSQL server with forced SSL connections. Remember to change the initial passwords immediately after setup.

#### Links

https://hub.docker.com/_/postgres/

https://markwoodbridge.com/2017/08/16/postgres-docker-ssl.html

#### Installation steps

Create configuration files

```bash
PGVERSION=10.5
docker run --detach \
    --name postgresql \
    --hostname postgresql \
    -e POSTGRES_PASSWORD=s3cr3t \
    --volume postgresql-data:/var/lib/postgresql/data \
    postgres:$PGVERSION
sleep 5
docker stop postgresql
docker rm postgresql

mkdir -p ~/docker-postgres/config && cd ~/docker-postgres/config
# Create self signed SSL certificate
openssl req -new -text -passout pass:abcd -subj /CN=localhost -out server.req
openssl rsa -in privkey.pem -passin pass:abcd -out server.key
openssl req -x509 -in server.req -text -key server.key -out server.crt
chmod og-rwx server.key
# generate and adjust configuration files
docker run -i --rm postgres:$PGVERSION cat /usr/share/postgresql/postgresql.conf.sample > my-postgres.conf
docker run -i --volume postgresql-data:/var/lib/postgresql/data --rm postgres:$PGVERSION cat /var/lib/postgresql/data/pg_hba.conf > pg_hba.conf
echo "ssl = on" >> my-postgres.conf
echo "hba_file = '/config/pg_hba.conf'" >> my-postgres.conf
echo "ssl_cert_file = '/config/server.crt'" >> my-postgres.conf
echo "ssl_key_file = '/config/server.key'" >> my-postgres.conf
sed -i -- 's/host all all all md5/hostssl all all all md5/g' pg_hba.conf
cd ..
```

save docker-compose.yml:

```yml
version: '3'

services:
  postgres:
    image: postgres:10.5
    restart: unless-stopped
    ports:
      - "5432:5432"
    volumes:
      - postgresql-data:/var/lib/postgresql/data
      - ./config/my-postgres.conf:/config/postgresql.conf
      - ./config/pg_hba.conf:/config/pg_hba.conf
      - ./config/server.key:/config/server.key
      - ./config/server.crt:/config/server.crt
    environment:
      POSTGRES_PASSWORD: s3cr3t
    command: -c 'config_file=/config/postgresql.conf'
  pgadmin:
    image: dpage/pgadmin4
    restart: unless-stopped
    ports:
      - "8888:80"
    environment:
      - PGADMIN_DEFAULT_EMAIL=foo@bar.com
      - PGADMIN_DEFAULT_PASSWORD=foobar
    volumes:
      - pgadmin-data:/var/lib/pgadmin

volumes:
  postgresql-data:
  pgadmin-data:
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

#### Backup / Restore

##### All Databases

https://stackoverflow.com/questions/24718706/backup-restore-a-dockerized-postgresql-database

Backup:

```bash
docker exec -t your-db-container pg_dumpall -c -U postgres > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql
```

Restore:

```bash
cat your_dump.sql | docker exec -i your-db-container psql -U postgres
```

##### Single Database

https://www.digitalocean.com/community/tutorials/how-to-backup-postgresql-databases-on-an-ubuntu-vps

Backup:

```bash
docker exec -t your-db-container pg_dump -c -U dbname > dump_dbname_`date +%d-%m-%Y"_"%H_%M_%S`.sql
```

Restore:

```bash
cat your_dump.sql | docker exec -i your-db-container psql -U postgres dbname
```


### pgadmin4

Setup an a pgadmin4 container listening only on localhost for using with a reverse proxy serving over https.

https://hub.docker.com/r/dpage/pgadmin4/

bash script:

```bash
#!/bin/bash
docker run --detach \
    --name pgadmin4 \
    --hostname pgadmin4 \
    --link postgresql:postgresql \
    --volume pgadmin-data:/var/lib/pgadmin \
    --publish 127.0.0.1:10080:80 \
    -e "PGADMIN_DEFAULT_EMAIL=foo@bar.com" \
    -e "PGADMIN_DEFAULT_PASSWORD=s3cr3t" \
    dpage/pgadmin4
```

docker-compose.yml:

```ruby
pgadmin:
  image: dpage/pgadmin4
  restart: unless-stopped
  ports:
    - "10080:80"
  environment:
    - PGADMIN_DEFAULT_EMAIL=foo@bar.com
    - PGADMIN_DEFAULT_PASSWORD=foobar
  volumes:
    - pgadmin-data:/var/lib/pgadmin

volumes:
  pgadmin-data:
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

## Snippets

### Users / Permissions

```sql
CREATE DATABASE database_name;
CREATE USER my_username WITH PASSWORD 'my_password';
GRANT ALL PRIVILEGES ON DATABASE "database_name" to my_username;
```
