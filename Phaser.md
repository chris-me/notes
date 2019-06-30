## Fonts

Include fonts in html head and add a div in body that's not displayed to load the font(s)

```html
<link
  href="https://fonts.googleapis.com/css?family=Indie+Flower&display=swap"
  rel="stylesheet"
/>

<div
  style="font-family:'Indie Flower'; position:absolute; left:-1000px; visibility:hidden;"
>
  .
</div>
```

Font can now be used in game
```javascript
const test1 = this.add.text(400, 50, 'Phaser 3', {
  fontSize: 32,
  fontFamily: 'Indie Flower',
});
```

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
