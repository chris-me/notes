# MongoDB

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
