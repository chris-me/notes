# Netstat for docker containers

https://stackoverflow.com/questions/40350456/docker-any-way-to-list-open-sockets-inside-a-running-docker-container

```bash
# get PID of container
CONTAINER_PID=`docker inspect -f '{{.State.Pid}}' container_name_or_id`
# execute netstat in namespace of container
sudo nsenter -t $CONTAINER_PID -n netstat -at
# or in one line
sudo nsenter -t `docker inspect -f '{{.State.Pid}}' container_name_or_id` -n netstat -at
```
