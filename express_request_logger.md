Simple express server log log all incoming requests

```javascript
const express = require('express');
const http = require('http');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser());

app.use((req, res, next) => {
  console.log(`originalUrl: ${req.originalUrl}`);
  console.log(req.body);
  res.json({ some: 'test' });
});

http.createServer(app).listen(7000);
```
