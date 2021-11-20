# How to install, setup and configure LEMP on VPS (Ubuntu 20.04)

This installation will include the following steps

* Associating your domain (eg. __dms.monster__) with hosting provider (VPS provider)
* Installing __LEMP__ (Linux Nginx MySQL PHP)
* Securing MariaDB (mysql) database server
* Adding database with user and creatinpermissions
* Getting WordPress files and configuring its public folder
* Configuring virtual host for our domain with nginx & php
* Installing free SSL with Certbot
* Cleaning up WordPress installation after first login to admin panel

## DNS setup of your domain

1. Add domain with the hosting provider (eg. Digital Ocean)
 *	my domain is dms.monster & I just parked it with Digital Ocean in the administration panel
 * see attached image: 
 <https://github.com/squteronsquter/wordpress-lemp-vps-setup/blob/main/images/digitalocean-dms-monster-domain-ns-setup.jpg>
 
## Configure NS with the domain registrar. 

See the picture: <https://github.com/squteronsquter/wordpress-lemp-vps-setup/blob/main/images/namecheap-dms-monster-ns-config.jpg>

## Install LEMP:

* update and upgrade VPS after you sss into it
    
`
apt-get update && apt-get upgrade
`

* Install Nginx, MariaDB, PHP (*precede commands with __sudo__ if you are not root*):

`
apt install nginx mariadb-server php-mysql php-fpm
`
* Lock the database server after installation:

`
mysql_secure_installation
`

##  Get WordPress

```
cd /var/www/
mkdir dms.monster
cd dms.monster
wget https://wordpress.org/latest.tar.gz
ls
tar -xzvf latest.tar.gz
rm latest.tar.gz
chown -R www-data:www-data wordpress
```
## Create database, database user and grant permissions;

```
mysql
#>
CREATE DATABASE example_db default character set utf8 collate utf8_unicode_ci;
CREATE USER 'example_user'@'localhost' IDENTIFIED BY 'example_passwd';
GRANT ALL PRIVILEGES ON example_db.* TO 'example_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Setup Nginx virtualhost for dms.monster domain on that server

```
cd /etc/nginx
rm sites-enabled/default
php --version
vim sites-available/dms.monster
```

You could also name the vhost configuration file "wordpress.conf" but "dms.monster" would help with Certbot, as it would discover probably the domain by the name of this file rather than by the folder in /var/www/ folder

## The contents of the vhost conf file (named above)

```
# vim /etc/nginx/sites-aivailable/wordpress.conf or dms.monster


upstream php-handler {
		server unix:/var/run/php/php7.4-fpm.sock;
}
server {
		listen 80 ;
		server_name dms.monster www.dms.monster;
		root /var/www/dms.monster/wordpress;
		index index. php;
		location / {
			try_files $uri $uri/ /index .php?$args;
		}
		location ~ \. php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass php-handler;
		}
}
```
* check the Nginx configuration and reload nginx server

```
nginx -t
systemctl reload nginx

```
## Install SSL with Certbot

<https://certbot.eff.org/>

```
snap install core; sudo snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
certbot --nginx
certbot renew --dry-run

```
## Check the website and proceed to WordPress Installation:

<https://dms.monster/>

This is all.😀😀😀

***Happy coding***




	
