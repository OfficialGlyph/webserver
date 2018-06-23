# webserver
centos 7 / php 7.2 / mariadb / nginx / laravel 5.5

## Initial Setup

Install as minimal install.

sudo dhclient

sudo yum update -y

sudo yum install epel-release -y

sudo -i

mkdir /media/VirtualBoxGuestAdditions

mount -r /dev/cdrom /media/VirtualBoxGuestAdditions

yum install gcc kernel-devel kernel-headers dkms make bzip2 perl wget deltarpm -y


KERN_DIR=/usr/src/kernels/\`uname -r\`/build

export KERN_DIR

cd /media/VirtualBoxGuestAdditions

./VBoxLinuxAdditions.run

reboot

sudo dhclient


wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm

sudo rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm


sudo yum install yum-utils -y

sudo yum-config-manager --enable remi-php72

sudo yum update -y

### nginx

sudo yum install nginx -y


sudo systemctl start nginx

sudo systemctl enable nginx

sudo systemctl status nginx


sudo firewall-cmd --zone=public --permanent --add-service=http

sudo firewall-cmd --zone=public --permanent --add-service=https

sudo firewall-cmd --reload


curl localhost

### mariadb

sudo yum install mariadb-server mariadb -y

sudo systemctl start mariadb

sudo mysql_secure_installation

sudo systemctl enable mariadb


### php

(or use vim)

sudo yum install php nano -y



sudo nano /etc/php.ini 

cgi.fix_pathinfo=0


sudo nano /etc/php-fpm.d/www.conf

listen = /var/run/php-fpm/php-fpm.sock

user = nginx

group = nginx

listen.owner = nginx

listen.group = nginx

listen.mode = 0660


sudo systemctl start php-fpm

sudo systemctl enable php-fpm


sudo nano /etc/nginx/nginx.conf

#### Change server block

```bash

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
```

sudo systemctl restart nginx


sudo nano /usr/share/nginx/html/info.php



## to be continued:
https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-centos-7

https://laravel.com/docs/5.5/installation

https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx


sudo yum search php | egrep 'openssl|mbstring|tokenzier|xml|ctype'

#### We need these php extensions

OpenSSL PHP Extension
Mbstring PHP Extension
Tokenizer PHP Extension
XML PHP Extension
Ctype PHP Extension

## file permissions

## selinux
