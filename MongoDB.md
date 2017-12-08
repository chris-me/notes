# MongoDB

## Docker

```bash
#!/bin/bash

docker run --detach \
  --restart=unless-stopped \
  --publish 127.0.0.1:27017:27017 \
  --volume mongodb-data:/data/db \
  --volume mongodb-config:/data/configdb \
  --name mongodb \
  mongo:3.4
```
