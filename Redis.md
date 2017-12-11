# Redis

## Docker

```bash
#!/bin/bash

docker run --detach \
  --publish 6379:6379 \
  --volume redis-data:/data \
  --restart=unless-stopped \
  --name redis \
  redis:alpine redis-server --appendonly yes

```
