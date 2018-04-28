
```bash
#!/bin/bash
docker run --name postgres01 \
    -e POSTGRES_PASSWORD=foobar \
    --publish 5432:5432 \
    --detach \
    postgres
```
