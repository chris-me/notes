- [GitLab CI](#gitLab-ci)
- [GitLab Docker](#gitlab-docker)

# GitLab CI

## Setup runner

```bash
docker run --name gitlab-runner \
    --rm \
    --volume /docker-volumes/gitlab-runner-config:/etc/gitlab-runner \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    -it \
    gitlab/gitlab-runner:latest \
    register
```

* enter info from glitlab -> admin -> runners
* executor = docker

* start runner script:

```bash
#!/bin/bash
docker run --detach \
    --name gitlab-runner \
    --restart=unless-stopped \
    --volume /docker-volumes/gitlab-runner-config:/etc/gitlab-runner \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    -it \
    gitlab/gitlab-runner:latest
```

* the runner is now registered in gitlab

## Workflow for deploying to remote server with SSH

https://docs.gitlab.com/ee/ci/ssh_keys/README.html

- On the server / user that gitlab will login to, run:

```bash
ssh-keygen
```

- Add the own public key to the authorized_keys file and adjust permissions

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 700 ~/.ssh/*
```

- Add the private key as a variable named SSH_PRIVATE_KEY to your project

- Adjust / use basic .gitlab-ci.yml file:

```yaml
image: node:10.6

variables:
  DEPLOY_IP: "1.2.3.4"
  DEPLOY_SSH_PORT: "22"
  DEPLOY_USER: "my-deploy-user"

before_script:
  - 'which ssh-agent || ( apt-get update -y && apt-get install openssh-client -y )'
  - eval $(ssh-agent -s)
  - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add - > /dev/null
  - mkdir -p ~/.ssh
  - chmod 700 ~/.ssh
  - '[[ -f /.dockerenv ]] && echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config'

stages:
  - deploy

deploy_backend:
  stage: deploy
  only:
    - master
  script:
    - ssh -p $DEPLOY_SSH_PORT -l $DEPLOY_USER $DEPLOY_IP uname -a
```


## Delete an environment

https://docs.gitlab.com/ce/api/environments.html#delete-an-environment

- Create an personal access token (User Settings / Access Tokens) with 'api' scope
- Get the roject ID (project settings --> general project settings)
- Get the environment ID (project --> CI --> Hover over the environment)
- Execute CURL request:

```bash
curl --request DELETE --header "PRIVATE-TOKEN: mytoken" "https://gitlab.example.com/api/v4/projects/1/environments/1"
```

- Deleting can take up to several minutes

# GitLab Docker

## Related Links

https://docs.gitlab.com/omnibus/docker/README.html

https://docs.gitlab.com/omnibus/settings/nginx.html#supporting-proxied-ssl

## Apache vHost


```bash
a2enmod proxy_http
```

Apache Configuration example:

```apache
<VirtualHost *:80>
    ServerName gitlab.mydomain.com
    AllowEncodedSlashes NoDecode # added after WebIDE problems
    ProxyPass / http://localhost:10080/ nocanon # nocanon added after WebIDE problems
    ProxyPassReverse / http://localhost:10080/
</VirtualHost>
```

## UFW

```bash
sudo ufw allow 10022/tcp
sudo ufw allow 10080/tcp
```

## Docker

```bash
docker run --detach \
    --hostname gitlab.mydomain.com \
    --publish 127.0.0.1:10080:80 \
    --publish 22:22 \
    --name gitlab \
    --restart=unless-stopped \
    --volume /docker-volumes/gitlab-config:/etc/gitlab \
    --volume /docker-volumes/gitlab-logs:/var/log/gitlab \
    --volume /docker-volumes/gitlab-data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```

### Adjustments in /etc/gitlab/gitlab.rb:

```ruby
external_url 'https://git.mydomain.com'
gitlab_rails['gravatar_ssl_url'] = 'https://secure.gravatar.com/avatar/%{hash}?s=%{size}&d=identicon'
nginx['listen_port'] = 80
nginx['listen_https'] = false
nginx['proxy_set_headers'] = {
  "X-Forwarded-Proto" => "https",
  "X-Forwarded-Ssl" => "on"
}
# only if ssl is on the host is not on port 22:
#gitlab_rails['gitlab_shell_ssh_port'] = 10022
```

### Upgrade

```bash
docker stop gitlab
docker rm gitlab
docker pull gitlab/gitlab-ce:latest
```

run as usual
