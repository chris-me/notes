https://github.com/hapijs/joi

## Validate a schema

```javascript
const validationResult = Joi.validate(inputToValidate, schema);
if (validationResult.error) {
  // validation error
}
```

## Schema examples

```javascript
const schema1 = Joi.object({
  username: Joi.string().when('emitTo', {
    is: 'username',
    then: Joi.required(),
  }),
  ipaddress: Joi.string()
    .ip()
    .when('emitTo', {
      is: 'ipaddress',
      then: Joi.required(),
    }),
  emitTo: Joi.string()
    .valid(['ipaddress', 'username'])
    .required(),
});
```
