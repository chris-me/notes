
# Docker

https://elk-docker.readthedocs.io/

## docker-compose

```
version: '3'

services:
  elk:
    image: sebp/elk
    ports:
      - "5044:5044"
      - "5601:5601"
      - "9200:9200"
```

## create dummy log entry

```bash
docker exec -it <container-name> /bin/bash
/opt/logstash/bin/logstash --path.data /tmp/logstash/data \
    -e 'input { stdin { } } output { elasticsearch { hosts => ["localhost"] } }'
# Wait for Logstash to start
dummy entry 1
dummy entry 2
# Create as much entries as wanted
```

## Access kibana web interface

http://yourhost:5601
