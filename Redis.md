# Redis

## Docker

```bash
#!/bin/bash

docker run --detach \
  --publish 6379:6379 \
  --volume redis-data:/data \
  --name redis \
  --appendonly yes \
  redis:alpine redis-server

```
