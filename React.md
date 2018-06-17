- [Image Processing](#image-processing)
- [Iterating Components](#iterating-components)
- [Prop Types](#prop-types)


## Image Processing

Encode an image / a file into a base64 string

```javascript
// Somewhere in a form
<Input
type="file"
name="picture"
id="picture"
onChange={this.handleImageChange} />
              
// event handler      
  handleImageChange = async (e) => {
    // TODO check it's really an image
    e.preventDefault();
    let reader = new FileReader();
    let file = e.target.files[0];
    // console.log(file);
    reader.onloadend = () => {
      this.setState({
        file: file,
        imageAsBase64: reader.result
      });
    }
    reader.readAsDataURL(file);
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
        <h4>No Camps found</h4>
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
