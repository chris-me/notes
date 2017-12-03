## Docker

https://jenkins.io/doc/book/installing/#docker
https://github.com/jenkinsci/docker/blob/master/README.md

```bash
docker run --detach \
  --rm --user root \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --name jenkins jenkins/jenkins:lts
```
