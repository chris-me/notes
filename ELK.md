- [Docker](#docker)
- [Logstash](#logstash)
- [Links](#links)

# Docker

https://elk-docker.readthedocs.io/

## set required kernel options

```bash
sudo sysctl -w vm.max_map_count=262144
sudo /bin/su -c "echo 'vm.max_map_count=262144' >> /etc/sysctl.conf"
```

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

# Logstash

## Grok pattern examples

```
input {
  beats {
    host => "0.0.0.0"
    port => 31312
  }
}
filter {
  grok {
    match => { "message" => "%{TIMESTAMP_ISO8601};%{WORD:loglevel};%{WORD:request_type};%{UUID:request_id};%{IP:ipaddress};%{GREEDYDATA}" }
  }
  if ([message] =~ "calculateExtendedRouteResponse") {
    drop {}
  }
}
```

```
if "_grokparsefailure" in [tags] {
  drop { }
}
```

# Links


## Data filtering and classification

- https://discuss.elastic.co/t/how-to-tag-log-files-in-filebeat-for-logstash-ingestion/44713/7
- https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html
- https://github.com/logstash-plugins/logstash-patterns-core/blob/master/patterns/grok-patterns

## Data curation

- http://www.madhur.co.in/blog/2017/04/09/usingcuratordeleteelasticindex.html
