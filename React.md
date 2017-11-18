- [Code Splitting](#code-splitting)
- [Iterating Components](#iterating-components)

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

## Iterating Components

### directly inside of render function

```javascript
{this.props.camps.map( (camp, index) => {
  return <Camp camp={camp} key={index}/>
})}
```

### as class method

```javascript
  renderCamps() {
    var camps = this.props.campStore.camps;
    if (camps && camps.length > 0) {
      return (
        camps.map( (camp, index) => {
          return <Camp camp={camp} key={index}/>
        })
      );
    } else {
      return (
        <h4>Not Camps found</h4>
      )
    }
  }
```

Then call it somewhere in a render function

```javascript
{this.renderCamps()}
```
