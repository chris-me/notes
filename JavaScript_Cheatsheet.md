## Async / Await

### setTimeout / sleep

```javascript
const sleep = function (ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve()
    }, ms)
  })
}
```


```javascript
await sleep(1000)
```
