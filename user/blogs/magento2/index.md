---
title: How to install Magento 2.4.5 on ubuntu 22.04
description: 
summary: 
published: '2024-05-24T18:03:48.000+08:00'
updated: '2024-05-24T18:03:48.000+08:00'
cover: ./cover.jpg
coverCaption: Photo by
coverStyle: 'IN'
series_tag:
series_title:
tag:
  - [svelte-QWER]
---
Step-by-Step Guide to Installing Magento 2.4.5 on Ubuntu 22.04
Introduction

Magento is a leading e-commerce platform trusted by millions of businesses worldwide for its flexibility and scalability. This guide will walk you through the process of installing Magento 2.4.5 on Ubuntu 22.04, ensuring your system is ready for high-performance e-commerce operations.

Prerequisites

Before we begin, ensure your system meets the following requirements:

Ubuntu 22.04 installed.
Root or sudo user access.
Minimum 2GB RAM (4GB or more recommended).
At least 5GB of free disk space.
You will also need to install the following software and tools:

Apache2 web server
MySQL database server
PHP and required extensions
Composer
Elasticsearch
Step 1: Update the System

First, update your system packages to the latest versions:

bash
Copy code
sudo apt update
sudo apt upgrade -y
Step 2: Install Apache2 Web Server

Install Apache2 and enable it to start on boot:

bash
Copy code
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
Step 3: Install MySQL Database Server

Install MySQL and secure the installation:

bash
Copy code
sudo apt install mysql-server -y
sudo mysql_secure_installation
Create a database and user for Magento:

bash
Copy code
sudo mysql -u root -p
CREATE DATABASE magento;
CREATE USER 'magentouser'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON magento.* TO 'magentouser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
Step 4: Install PHP and Required Extensions

Install PHP 7.4 and necessary extensions:

bash
Copy code
sudo apt install -y php7.4 php7.4-cli php7.4-fpm php7.4-mysql php7.4-xml php7.4-curl php7.4-gd php7.4-mbstring php7.4-intl php7.4-bcmath php7.4-soap php7.4-zip php7.4-xsl
Step 5: Install Composer

Install Composer globally:

bash
Copy code
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
Step 6: Download Magento 2.4.5

Use Composer to download Magento:

bash
Copy code
cd /var/www/html
sudo composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.5
Step 7: Configure Apache for Magento

Create a virtual host configuration for Magento:

bash
Copy code
sudo nano /etc/apache2/sites-available/magento.conf
Add the following configuration:

apache
Copy code
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/magento
    ServerName example.com

    <Directory /var/www/html/magento>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/magento_error.log
    CustomLog ${APACHE_LOG_DIR}/magento_access.log combined
</VirtualHost>
Enable the Magento site and rewrite module:

bash
Copy code
sudo a2ensite magento.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
Set appropriate file permissions:

bash
Copy code
sudo chown -R www-data:www-data /var/www/html/magento
sudo find /var/www/html/magento -type d -exec chmod 755 {} \;
sudo find /var/www/html/magento -type f -exec chmod 644 {} \;
Step 8: Install Elasticsearch

Magento requires Elasticsearch. Install it with:

bash
Copy code
sudo apt install elasticsearch -y
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
Step 9: Finalize the Magento Installation

Navigate to your Magento directory and start the setup wizard:

bash
Copy code
cd /var/www/html/magento
bin/magento setup:install --base-url=http://example.com \
--db-host=localhost --db-name=magento --db-user=magentouser --db-password=yourpassword \
--admin-firstname=Admin --admin-lastname=User --admin-email=admin@example.com \
--admin-user=admin --admin-password=admin123 --language=en_US --currency=USD \
--timezone=America/Chicago --use-rewrites=1
Conclusion

Congratulations! You have successfully installed Magento 2.4.5 on Ubuntu 22.04. You can now access your Magento store through your browser and start customizing it to fit your e-commerce needs. For further customization and optimization, refer to Magento's official documentation and community resources.