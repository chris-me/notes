## Docker

https://jenkins.io/doc/book/installing/#downloading-and-running-jenkins-in-docker

```bash
docker run --detach \
  --user root \
  --rm \
  --publish 8080:8080 \
  --volume jenkins-data:/var/jenkins_home \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --name jenkins
  jenkinsci/blueocean
```
