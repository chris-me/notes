# MongoDB

## Docker

### generate self signed certificate

https://docs.mongodb.com/manual/tutorial/configure-ssl/

```bash
openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes -out mongodb-cert.crt -keyout mongodb-cert.key
cat mongodb-cert.key mongodb-cert.crt > mongodb.pem
rm mongodb-cert.key mongodb-cert.crt
sudo mv mongodb.pem /docker-volumes/mongo-config/
```

### Running

Important: user is only created if no database exists at start time, otherwise the parameters for username and password are ignored. The database is then automatically running with "auth" parameter enabled, e. g. only authenticated / authorized users can interact with the database.

#### Bash Script

```bash
#!/bin/bash
docker run --detach \
  --restart=unless-stopped \
  --publish 27017:27017 \
  --volume /docker-volumes/mongo-data:/data/db \
  --volume /docker-volumes/mongo-config:/data/configdb \
  -e MONGO_INITDB_ROOT_USERNAME=mongoadmin
  -e MONGO_INITDB_ROOT_PASSWORD=mypassword \
  --name mongodb \
  mongo:4 \
  --sslMode requireSSL --sslPEMKeyFile /data/configdb/mongodb.pem
```

#### Compose file

```yml
version: '3.3'

services:
  mongo:
    container_name: mongo
    image: mongo:4
    volumes:
      - /docker-volumes/mongo-data:/data/db
      - /docker-volumes/mongo-config:/data/configdb
    restart: unless-stopped
    environment:
      MONGO_INITDB_ROOT_USERNAME: mongoadmin
      MONGO_INITDB_ROOT_PASSWORD: password
    ports:
      - "27017:27017"
    command: --sslMode requireSSL --sslPEMKeyFile /data/configdb/mongodb.pem


```


## Authentication / Authorization

### Create a user and assign collection

```javascript
use myapp
db.createUser(
  {
    user: "myappuser",
    pwd: "myappuserpassword",
    roles: [ { role: "readWrite", db: "myapp" }]
  }
)
```

### Connection string for nodejs driver

Also works for mongoose

```javascript
"mongodb://myappuser:myappuserpassword@localhost:27017/myapp?ssl=true"
```
