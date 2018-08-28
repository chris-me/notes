# Portainer

https://portainer.io/install.html


## docker-compose

```
version: '3'

services:
  portainer:
    image: portainer/portainer
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer-data:/data
    ports:
      - "9000:9000"

volumes:
  portainer-data:
```

## Bash script
```bash
#!/bin/bash

docker run --detach \
  --restart=unless-stopped \
  --publish 9000:9000 \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --volume portainer-data:/data \
  --name portainer \
  portainer/portainer
```
