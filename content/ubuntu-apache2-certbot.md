Title: Ubuntu Apache2 Certbot
Date: 2019-03-15T11:52:59+08:00
Category: web
Tags: ubuntu, certbot, apache2
Author: Jack

## Enviroment:

* Ubuntu 18.04
* Python 3.6

## Prerequisites:

* A target web application that you want to host, mantis for example.
* A domain name point to server, mantis.mydomain.com for example

## Installations:

1. Apache2

```bash
sudo apt-get install apache2
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo systemctl restart apache2
```

2. Certbot

```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt install python-certbot-apache    
```

3. Config Apache with a new site, (mentis)

create configuration file

```bash
sudo nano /etc/apache2/sites-available/mantis.conf
```

with following content

```
<VirtualHost *:80>
DocumentRoot "/var/www/html/mantis"
ServerName mantis.wedev.tk
ErrorLog "/var/log/apache2/mantis-error_log"
CustomLog "/var/log/apache2/mantis-access_log" combined
<Directory "/var/www/html/mantis/">
DirectoryIndex index.php index.html
Options FollowSymLinks
AllowOverride All
Require all granted
</Directory>
</VirtualHost>
```
run commands

```bash
sudo a2ensite mantis.conf
sudo systemctl reload apache2
```

4. ask certbot to generate SSL for your new site

```bash
sudo certbot --apache -d mantis.xxx.xxx
```

You should be able to open browser at https://mantis.xxx.xxx now.
