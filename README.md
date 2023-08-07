
## Assignment Submission

This is a detailed document containing all the necessary server configuration instructions, details of security practices followed, nginx server optimization applied.

All the details regarding the project can be found in this document.


Server Provisiong:-
 
Using AWS EC2 instances with basic configuration (t2.mico, Ubuntu 20.04, 20GB storage)
From AWS securtiy groups one kept post 22, 443 open rest closed.


Linux Security Service Best Pactices Followed:-

Ensured strong password of the machine by using SSH key pairs
Keeping the system Up-to-date
Removing all the Unnecessary softwares
Keeping only required ports open for access with restricted IP access if possible
Create and maintain Backups (AWS automatic backups can be used here)


Installation Steps:-

#Keeping system up-to-date
sudo apt update
sudo apt upgrade -y

#Installing Nginx
sudo apt install nginx -y

#Installing SSL using Credbot
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot

#Cofiguring Nginx with Domain
cd /etc/nginx/sites-enabled/
sudo nano default
sudo nginx -t
sudo systemctl reload nginx

#Obtaining SSL Certifcate
sudo certbot --nginx -d example.com -d www.example.com

#Configuring Nginx to use PHP preprocessor
cd /etc/nginx/sites-enabled/
sudo nano default
sudo nginx -t
sudo systemctl reload nginx

#Installing MySQL
sudo apt install mysql-server -y

#Running security script of MySQL to remove insecure settings and lock down access of DB to my system
sudo mysql_secure_installation

#Setting us DB and DBuser for wordpress installation
sudo mysql
CREATE DATABASE wordpressDB DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'wordpressUSER'@'localhost' IDENTIFIED BY 'admin1234';
GRANT ALL ON wordpressDB.* TO 'wordpressUSER'@'localhost';
EXIT;

#Installing PHP and required dependencies for workpress
sudo apt install php8.1-fpm php-mysql php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip
sudo systemctl restart php8.1-fpm

#Configure Nginx again for wordpress installation
cd /etc/nginx/sites-enabled/
sudo nano default
sudo nginx -t
sudo systemctl reload nginx

#Downloading and Installing WordPress
cd /home/ubuntu
cd /wordpress
curl -LO https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
cp /wordpress/wp-config-sample.php ./wordpress/wp-config.php
sudo cp -a /wordpress/.  /var/www/html/wordpress
sudo chown -R www-data:www-data /var/www/html/wordpress

#Generating secret keys for wordpress config
curl -s https://api.wordpress.org/secret-key/1.1/salt/

#Pasting the above generated keys and DB credentials in the config file
sudo nano /var/www/html/wordpress/wp-config.php

#Complete the istallation by visting the hosted website link and then doing the installation steps from the UI.


Nginx server Configration to enhance performance:-

Adjusting NGINX worker processes
Modify the number of worker connections
Enable static content caching
Changing limits on timeout values
