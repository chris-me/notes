- [Classes](#classes)
- [Iterating Components](#iterating-components)
- [Prop Types](#prop-types)


## Classes

## class properties + arrow function syntax

```javascript
class InputExample extends React.Component {
  state = { text: '' }
  change = ev => this.setState({text: ev.target.value})

  render() {
    let {text} = this.state
    return (<input type="text" value={text} onChange={this.change} />);
  }
}
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

## Prop Types

### Require a prop type based on presence of another

```javascript
Card.propTypes = {
  imageUrl: PropTypes.string,
  imagePlaceholder: function(props, propName, componentName) {
    if (
      props.hasOwnProperty('imageUrl') &&
      !props.hasOwnProperty('imagePlaceholder')
    ) {
      return new Error(
        'If the prop `imageUrl` is given, the prop `imagePlaceholder` is required in `Card`.'
      );
    }
  }
};
```
