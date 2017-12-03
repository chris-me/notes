## Docker

```bash
docker run --detach \
  --rm --user root \
  --publish 8080:8080 \
  --volume jenkins-data:/var/jenkins_home \
  --name jenkins jenkins/jenkins:lts
```
