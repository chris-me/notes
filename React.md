## Code Splitting

### General / inside of a Component

https://facebook.github.io/react/blog/2017/05/18/whats-new-in-create-react-app.html#code-splitting-with-dynamic-import

### Route based (react-router v3)

https://www.youtube.com/watch?v=bb6RCrDaxhw

Example:

```javascript
const routes = (
  <Route>
    <Route
      path="/somepage"
      getComponent={ (_, cb) => {
        import('./SomePage')
        .then(somePageModule => somePageModule.default)
        .then(somePage => cb(null, somePage))
        .catch( e => cb(e, null))
      }}
      />
  </Route>
)
```
