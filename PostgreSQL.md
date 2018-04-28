
```bash
#!/bin/bash
docker run --name postgres01 \
    -e POSTGRES_PASSWORD=foobar \
    --detach \
    postgres
```
