# Portainer

https://portainer.io/install.html

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
