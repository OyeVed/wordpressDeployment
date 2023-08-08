
## Assignment Submission

This is a detailed document containing all the necessary server configuration instructions, details of security practices followed, and nginx server optimization applied.

All the details regarding the project can be found in this document.

I have built the complete CICD pipeline for a WordPress website deployment by using RDS for the database so as to keep the database centralized so that it does not act as a limitation for multiple developers to work upon to keep all DBs in sync and also smooth deployment process as we do not manually have to update the DB.
## Resources provisioned

* Used AWS EC2 instance with basic configuration (t2.micro, 20GB storage)
* As per security practices followed kept only two ports open namely 22 for SSH and 443 for HTTPS 
* For database used AWS RDS with limited access given only to the EC2 instance IP 
## Server Setup

Installation Steps:-

#Keeping system up-to-date\
sudo apt update\
sudo apt upgrade -y

#Installing PHP and required dependencies for workpress\
sudo apt install php8.1-fpm php-mysql php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip\
sudo systemctl restart php8.1-fpm

#Installing Nginx\
sudo apt install nginx -y

#Configuring Nginx to use PHP preprocessor\
cd /etc/nginx/sites-enabled/\
sudo nano default\
sudo nginx -t\
sudo systemctl reload nginx

#Cofiguring Nginx with Domain\
sudo mkdir -p /var/www/webb-3.com/html\
sudo chown -R $USER:$USER /var/www/webb-3.com/html\
sudo chmod -R 755 /var/www\
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/webb-3.com\
sudo nano /etc/nginx/sites-available/webb-3.com\
sudo ln -s /etc/nginx/sites-available/webb-3.com /etc/nginx/sites-enabled/\
sudo nano /etc/nginx/nginx.conf\
sudo nginx -t\
sudo systemctl reload nginx

#Installing SSL using Credbot\
sudo snap install core; sudo snap refresh core\
sudo snap install --classic certbot\
sudo ln -s /snap/bin/certbot /usr/bin/certbot

#Obtaining SSL Certifcate\
sudo certbot --nginx -d webb-3.com -d www.webb-3.com

#Configure Nginx again for wordpress installation\
cd /etc/nginx/sites-enabled/\
sudo nano webb-3.com\
sudo nginx -t\
sudo systemctl reload nginx

#Downloading and Installing WordPress\
cd /home/ubuntu\
curl -LO https://wordpress.org/latest.tar.gz\
tar xzvf latest.tar.gz\
cp /wordpress/wp-config-sample.php ./wordpress/wp-config.php\
sudo cp -a ./wordpress/.  /var/www/webb-3.com/wordpress\
sudo chown -R www-data:www-data /var/www/webb-3.com/wordpress

#Generating secret keys for wordpress config\
curl -s https://api.wordpress.org/secret-key/1.1/salt/

#Pasting the above generated keys and DB credentials in the config file\
sudo nano /var/www/webb-3.com/wordpress/wp-config.php

#Complete the installation by visiting the hosted website link and then doing the installation steps from the UI.
## Linux Security Best Practices Followed

* Ensured strong password of the machine by using SSH key pairs
* Keeping the system Up-to-date
* Removing all the Unnecessary software
* Keeping only required ports open for access with restricted IP access if possible
* Create and maintain Backups (AWS automatic backups can be used here)

## Nginx server Configuration to enhance performance

* Adjusting NGINX worker processes
* Modify the number of worker connections
* Enable static content caching
* Changing limits on timeout values

## Final hosted website link


https://webb-3.com/wordpress/

This is a default wordpress website which I have hosted with a configured CICD pipeline.