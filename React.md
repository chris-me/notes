- [Code Splitting](#code-splitting)
- ["Dumb" Components](#dumb-components)
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

## "Dumb" Components

```javascript
const Camp = ({camp}) => (
  <div className="card">
    <img className="card-img-top img-fluid" src="http://placehold.it/1200x150" alt="Card image cap" />
    <div className="card-block">
      <h4 className="card-title">{camp.name}</h4>
      <p className="card-text">Some quick example text to build on the card title and make up the bulk of the cards content.</p>
      <a href="#" className="btn btn-primary">Go somewhere</a>
    </div>
  </div>
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
