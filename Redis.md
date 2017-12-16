# Redis

## Docker

### Create container

```bash
#!/bin/bash

docker run --detach \
  --publish 6379:6379 \
  --volume redis-data:/data \
  --restart=unless-stopped \
  --name redis \
  redis:alpine redis-server --appendonly yes
```

### Flush database

```bash
#!/bin/bash
docker run -it --link redis:redis --rm redis:alpine redis-cli -h redis FLUSHALL
```
