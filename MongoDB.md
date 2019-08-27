# MongoDB

## MongoDB shell

###  Connect to ssl w/ self-signed cert

```bash
mongo -ssl --sslAllowInvalidCertificates "mongodb://user:password@localhost/kursportal_dev?ssl=true"
```

## Connect with docker

```bash
docker run -it --rm mongo:4 mongo --sslAllowInvalidCertificates --ssl --host mongodb.myserver.com --authenticationDatabase admin --username user1 --password foobar
```

## Authentication / Authorization

### Create a user and assign collection

```javascript
use mydatabase
db.createUser(
  {
    user: "myappuser",
    pwd: "myappuserpassword",
    roles: [ { role: "readWrite", db: "mydatabase" }]
  }
)
```

### Connection string for nodejs driver

Also works for mongoose

```javascript
"mongodb://myappuser:myappuserpassword@localhost:27017/mydatabase?ssl=true"
```
