> Testet on Ubuntu 16.04

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
    --hostname git.teamheartcode.com \
	--env GITLAB_OMNIBUS_CONFIG="gitlab_rails['gitlab_shell_ssh_port'] = 10022" \
    --publish 127.0.0.1:10080:80 --publish 10022:22 \
    --name gitlab \
    --restart always \
    --volume gitlab-config:/etc/gitlab \
    --volume gitlab-logs:/var/log/gitlab \
    --volume gitlab-data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```

### Upgrade

```bash
docker ps -a
docker rm -f gogs
docker pull gogs/gogs
docker run -d --restart=unless-stopped \
    --name=gogs -p 10022:22 -p 127.0.0.1:10080:3000 \
    -v /mnt/docker-gogs-data:/data gogs/gogs
docker logs -f gogs
```
