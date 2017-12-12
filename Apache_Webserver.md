- [Single Page Application Virtual Hosts](#single-page-application-virtual-hosts)
- [Websockets (Socket.io) Virtual Hosts](#websockets-socket-io-virtual-hosts)

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
