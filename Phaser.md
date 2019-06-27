## Sprites

```javascript
// preload
this.load.image('heart', 'images/heart.png')
// add to scene
const heart = this.add.sprite(xpos, ypos, 'heart')
// set alpha
heart.alpha = 0.6 // 0 to 1
// rotate
heart.angle = 45
// scale
heart.setScale(0.5)
// change width
heart.displayWidth = 50
// scale proportionaly / equally (based on previously changed displayWidth
heart.scaleY = heart.scaleX
```
