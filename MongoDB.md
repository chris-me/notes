# MongoDB

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
