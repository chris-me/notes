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

### start script

Important: user is only created if no database exists at start time, otherwise the parameters for username and password are ignored

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
