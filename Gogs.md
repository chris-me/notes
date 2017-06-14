> Ubuntu specific for the moment

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
docker run --restart=unless-stopped --name=gogs -p 10022:22 -p 10080:3000 -v /mnt/docker-gogs:/data gogs/gogs
```
