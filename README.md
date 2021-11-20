# How to install, setup and configure LEMP on VPS

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
 * see attached image: https://github.com/squteronsquter/wordpress-lemp-vps-setup/blob/main/images/digitalocean-dms-monster-domain-ns-setup.jpg
2. Configure NS with the domain registrar