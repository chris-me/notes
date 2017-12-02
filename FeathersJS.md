- [Service Query Examples](#service-query-examples)

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
