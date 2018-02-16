- [Single Page Application Virtual Hosts](#single-page-application-virtual-hosts)
- [Websockets (Socket.io) Virtual Hosts](#websockets-socket-io-virtual-hosts)

## Centos 7 Installation with Virtual Host

https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-centos-7


```bash
sudo yum -y install httpd
sudo systemctl enable httpd.service
sudo mkdir /var/www/mysite
sudo chown -R mysiteuser:mysiteuser /var/www/mysite
sudo vi /var/www/mysite/index.html # for testing
sudo mkdir /etc/httpd/sites-available
sudo mkdir /etc/httpd/sites-enabled
sudo vi /etc/httpd/conf/httpd.conf
# Add this line at the end:
IncludeOptional sites-enabled/*.conf
# Create a default virtual hosts file:
sudo vi /etc/httpd/sites-available/00-default
# Content:
<VirtualHost *:80>
    ServerName www.example2.com
    ServerAlias example2.com
    DocumentRoot /var/www/html
    ErrorLog /var/log/httpd/error.log
    CustomLog /var/log/httpd/access.log combined
</VirtualHost>
sudo ln -s /etc/httpd/sites-available/00-default.conf /etc/httpd/sites-enabled/
# Create an application specific virtual hosts config file:
sudo vi /etc/httpd/sites-available/z_mysite.conf
sudo ln -s /etc/httpd/sites-available/z_mysite.conf /etc/httpd/sites-enabled/
sudo systemctl restart httpd.service
```

## Single Page Application Virtual Hosts

https://stackoverflow.com/questions/31744657/vhosts-conf-for-single-page-app

```apache
<VirtualHost *:80>
  ServerName my.app.com
  DirectoryIndex index.html
  DocumentRoot /export/www/app
  <Directory "/export/www/app">
    order allow,deny
    allow from all

    RewriteEngine on

    RewriteCond %{REQUEST_FILENAME} -s [OR]
    RewriteCond %{REQUEST_FILENAME} -l [OR]
    RewriteCond %{REQUEST_FILENAME} -d
    RewriteRule ^.*$ - [NC,L]
    RewriteRule ^(.*) /index.html [NC,L]
  </Directory>
</VirtualHost>
```


## Websockets (Socket.io) Virtual Hosts

[stackoverflow](https://stackoverflow.com/questions/27526281/websockets-and-apache-proxy-how-to-configure-mod-proxy-wstunnel/27534443#27534443
)

Assuming some socket.io backend is running on port 3030.

```bash
a2enmod proxy
a2enmod proxy_wstunnel
```


```apache
<VirtualHost *:80>
    ServerName api.myserver.com

    RewriteEngine On
    RewriteCond %{REQUEST_URI}  ^/socket.io            [NC]
    RewriteCond %{QUERY_STRING} transport=websocket    [NC]
    RewriteRule /(.*)           ws://localhost:3030/$1 [P,L]

    ProxyPass / http://localhost:3030/
    ProxyPassReverse / http://localhost:3030/
RewriteEngine on
RewriteCond %{SERVER_NAME} =api.myserver.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,QSA,R=permanent]
</VirtualHost>
```
