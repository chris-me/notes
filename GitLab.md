- [GitLab CI](#gitLab-ci)
- [GitLab Docker Integration](#gitlab-docker-integration)

# GitLab CI

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

# GitLab Docker Integration

## Related Links

https://docs.gitlab.com/omnibus/docker/README.html

https://docs.gitlab.com/omnibus/settings/nginx.html#supporting-proxied-ssl

## Apache vHost


```bash
a2enmod proxy_http
```

Apache Configuration example:

```
<VirtualHost *:80>
    ServerName git.mydomain.com
    #ServerAlias www.domain.tld 
    ProxyPass / http://localhost:10080/
    ProxyPassReverse / http://localhost:10080/
    RewriteEngine on
    RewriteCond %{SERVER_NAME} =git.mydomain.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,QSA,R=permanent]
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
    --hostname git.mydomain.com \
    --publish 127.0.0.1:10080:80 --publish 10022:22 \
    --name gitlab \
    --restart=unless-stopped \
    --volume gitlab-config:/etc/gitlab \
    --volume gitlab-logs:/var/log/gitlab \
    --volume gitlab-data:/var/opt/gitlab \
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
