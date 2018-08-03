- [Installation](#installation)
  - [Centos 7 Installation with Virtual Host](#centos-7-installation-with-virtual-host)
  - [Docker](#docker)
- [Virtual Hosts Configuration](#virtual-hosts-configuration)
  - [Single Page Application Virtual Hosts](#single-page-applications)
  - [Websockets (Socket.io) Virtual Hosts](#websockets-socketio)

## Installation

### Centos 7 Installation with Virtual Host

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

### Docker

Apache with PHP (and ionCube Extension) in Ubuntu Docker Container

#### Dockerfile

```dockerfile
FROM ubuntu:16.04

ENV IONCUBE_DOWNLOAD_URL https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    software-properties-common \
    python-software-properties \
    bzip2 unzip vim wget curl libxml2-dev git mysql-client \
    apache2 apache2-utils libapache2-mod-php php-mbstring \
    php-cli php-gd php-mysql php-mcrypt php-ldap \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/*

RUN wget ${IONCUBE_DOWNLOAD_URL} -O /tmp/ioncube_loaders.tar.gz

RUN tar xvzfC /tmp/ioncube_loaders.tar.gz /tmp/ \
    && rm /tmp/ioncube_loaders.tar.gz \
    && mkdir -p /usr/local/ioncube \
    && cp /tmp/ioncube/ioncube_loader_* /usr/local/ioncube \
    && rm -rf /tmp/ioncube

COPY 00-ioncube.ini /etc/php/7.0/apache2/conf.d/00-ioncube.ini
COPY 00-ioncube.ini /etc/php/7.0/cli/conf.d/00-ioncube.ini

VOLUME /var/www/html
WORKDIR /var/www/html

EXPOSE 80
CMD ["apache2ctl", "-DFOREGROUND"]
```

#### ioncube.ini

```ini
zend_extension = /usr/local/ioncube/ioncube_loader_lin_7.0.so
```

## Virtual Hosts Configuration

### Single Page Applications

https://stackoverflow.com/questions/31744657/vhosts-conf-for-single-page-app

```apache
<VirtualHost *:80>
  ServerName my.app.com
  DirectoryIndex index.html
  DocumentRoot /var/www/app
  <Directory "/var/www/app">
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


### Websockets (Socket.io)

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
</VirtualHost>
```
