# Code samples

## Queries

```graphql
query {
  users(where: {
    name_contains:"chris"
  }) {
    id
    name
    email
  }
}
```

## Mutations

```graphql
mutation {
  createUser(data: {
    name:"chris",
    email:"hey@cool.com"
  }) {
    # the fields that will be returned:
    name
    email
  }
}
```
