---
title: How to install Magento 2.4.7 in Ubuntu 22.04
description: Learn how to install Magento 2.4.5 in Ubuntu 22.04 locally (wsl2)
summary: Detailed guide on how to install magento 2.4.7 on ubuntu 22.04 with all the required dependencies.
published: '2024-06-25T00:00:00.000Z'
updated: '2024-06-25T00:00:00.000Z'
cover: ./R.png
coverCaption: Image from https://inchoo.net/magento-2/magento-2-is-available/
tags:
  - [M2]
  - [Installation]
  - [Ubuntu]
  - [wsl2]
  - [local]
---

# How to Install Magento 2.4.7 on Ubuntu 22.04 with WSL2 on Windows

As an e-commerce platform, Magento is a popular choice for building robust and feature-rich online stores. In this guide, we'll walk you through the process of installing Magento 2.4.7 on Ubuntu 22.04 using the Windows Subsystem for Linux (WSL2) on your Windows machine.

## Prerequisites

Before we begin, make sure you have the following installed on your system:

- Windows 10 or 11 with WSL2 enabled
- Ubuntu 22.04 distribution installed on WSL2
- Apache, PHP, MySQL, and Composer installed on your Ubuntu 22.04 environment

## Step 1: Update the Ubuntu 22.04 Environment

Start by updating your Ubuntu 22.04 environment to ensure you have the latest packages and security updates:

```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2: Install Required Dependencies

Install the necessary dependencies for Magento 2.4.7 installation:

```bash
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt install -y php8.1 php8.1-common php8.1-curl php8.1-gd php8.1-intl php8.1-mbstring php8.1-soap php8.1-xml php8.1-zip php8.1-bcmath php8.1-iconv php8.1-opcache
sudo apt install -y mysql-server mysql-client
sudo apt install -y composer
```

## Step 3: Create a MySQL Database and User

Create a new MySQL database and user for your Magento 2.4.7 installation:

```bash
sudo mysql -u root -p
CREATE DATABASE magento;
CREATE USER 'magento'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON magento.* TO 'magento'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Replace `'your_password'` with a secure password of your choice.

## Step 4: Install Magento 2.4.7

Navigate to the directory where you want to install Magento and run the following command to download and install Magento 2.4.7:

```bash
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.7 magento2
```

When prompted, enter your Magento Marketplace username (public key) and password (private key).

## Step 5: Configure Magento Installation

Change to the Magento installation directory and run the setup command:

```bash
cd magento2
sudo -u www-data bin/magento setup:install \
--base-url=http://local.magento.com \
--db-host=localhost \
--db-name=magento \
--db-user=magento \
--db-password=your_password \
--admin-firstname=Admin \
--admin-lastname=User \
[email protected] \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1
```

Replace `'your_password'` with the password you set for the Magento database user.

## Step 6: Configure Apache Virtual Host

Create a new Apache virtual host configuration file for your Magento 2.4.7 installation:

```bash
sudo nano /etc/apache2/sites-available/magento2.conf
```

Add the following content to the file:

```apache
<VirtualHost *:80>
    ServerName local.magento.com
    DocumentRoot /var/www/html/magento2/pub
    <Directory /var/www/html/magento2/pub>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/magento2-error.log
    CustomLog ${APACHE_LOG_DIR}/magento2-access.log combined
</VirtualHost>
```

Save and close the file, then enable the virtual host:

```bash
sudo a2ensite magento2.conf
sudo systemctl restart apache2
```

## Step 7: Access Magento 2.4.7

You can now access your Magento 2.4.7 installation by visiting `http://local.magento.com` in your web browser. The Magento admin panel can be accessed at `http://local.magento.com/admin`.

That's it! You have successfully installed Magento 2.4.7 on Ubuntu 22.04 using WSL2 on your Windows machine. Enjoy building your e-commerce store with Magento!