- [Feathers behind Apache Reverse Proxy](##feathers-behind-apache-reverse-proxy)
- [REST Requests](#rest-requests)
- [Service Query Examples](#service-query-examples)
- [Validation](#validation)

# REST Requests

```bash
curl 'http://localhost:3030/auth/local' -H 'Content-Type: application/json' --data-binary '{ "email": "user1@example.com", "password": "foobar" }'
```

# Service Query Examples

```javascript
someService.find({
  query: {
    $sort: {
      votes: -1
    }
  }
})
```

```javascript
someService.find({
  query: {
    name: 'Alice'
  }
})
```

```javascript
foodService.find({
  query: {
    "bestandteile.analytisch.fettgehalt": {
          $gt: 6.5,
          $lt: 20
        }
    }
}).then( (res) => {
  console.log(res)
})
```

```javascript
const result = await balancesService.find({
  query: {
    $limit: 20
  }
});
```

# Validation

## AJV

- http://epoberezkin.github.io/ajv/
- https://code.tutsplus.com/tutorials/validating-data-with-json-schema-part-1--cms-25343
