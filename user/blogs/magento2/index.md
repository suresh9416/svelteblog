---
title: How to install Magento 2.4.5 in Ubuntu 22.04
description: Learn how to install Magento 2.4.5 in Ubuntu 22.04 locally (wsl2)
summary: Detailed guide on how to install magento 2.4.5 on ubuntu 22.04 with all the required dependencies.
published: '2024-05-27T00:00:00.000Z'
updated: '2024-05-27T00:00:00.000Z'
cover: ./R.png
coverCaption: Image from https://inchoo.net/magento-2/magento-2-is-available/
tags:
  - [M2]
  - [Installation]
  - [Ubuntu]
  - [wsl2]
  - [local]
---

## Installing Magento 2.4.5 on Ubuntu 22.04 (WSL2) - Step-by-Step Guide with Example Values

This guide builds upon the previous one, including example values to illustrate the process. Remember, replace these examples with your actual information. Refer to the official Magento documentation for the latest details: [https://devdocs.magento.com/guides/v2.3/install-gde/bk-install-guide.html](https://devdocs.magento.com/guides/v2.3/install-gde/bk-install-guide.html)

**1. Update and Upgrade:**

```
sudo apt update
sudo apt upgrade
```

**2. Install Apache2:**

```
sudo apt install apache2
```

**3. Install MySQL and Create Database:**

```
sudo apt install mariadb-server
sudo mysql_secure_installation
```

**Set a Strong Password During `mysql_secure_installation`**

**4. Install PHP and Extensions (Example: PHP 8.1):**

```
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.1 php8.1-common php8.1-cli php8.1-curl php8.1-gd php8.1-imagick php8.1-mbstring php8.1-mysql php8.1-xml php8.1-zip
```

**5. Install Composer:**

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php --install-dir=~/.config/composer
mv ~/.config/composer/vendor/bin/composer /usr/local/bin/composer
```

**6. Download and Install Magento 2.4.5:**

```
sudo mkdir /var/www/html/magento2
cd /var/www/html/magento2
composer create-project magento/project-community-edition 2.4.5 . --stability=beta --prefer-dist
```

**7. Set Permissions:**

```
sudo chown -R www-data:www-data .
sudo chmod -R 755 .
```

**8. Configure Apache (Virtualhost):**

  - Create a virtual host file (e.g., `/etc/apache2/sites-available/magento2.conf`):

```
sudo nano /etc/apache2/sites-available/magento2.conf
```

  - Paste the following configuration, replacing `your_domain` with your actual domain name or IP address:

```apache
<VirtualHost *:80>

  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/html/magento2

  <Directory /var/www/html/magento2>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/magento2-error.log
  CustomLog ${APACHE_LOG_DIR}/magento2-access.log combined

</VirtualHost>
```

  - Enable the virtual host configuration:

```
sudo a2ensite magento2.conf
```

  - Restart Apache:

```
sudo systemctl restart apache2
```

**9. Configure Database Connection (Example):**

  - Edit the `app/etc/env.php` file:

```
sudo nano app/etc/env.php
```

  - Update the database connection details (replace with your actual information):

    ```php
    'host' => 'localhost',
    'dbname' => 'my_magento_database',  // Replace with your database name
    'username' => 'magento_user',        // Replace with your database username
    'password' => 'your_strong_password', // Replace with your strong password
    ```

**10. Install Magento:**

  - Navigate to the Magento root directory:

```
cd bin/
```

  - Run the installation script, replacing placeholders with your details:

```
cd /var/www/html/magento
bin/magento setup:install --base-url=http://example.com \
--db-host=localhost --db-name=magento --db-user=magentouser --db-password=yourpassword \
--admin-firstname=Admin --admin-lastname=User --admin-email=admin@example.com \
--admin-user=admin --admin-password=admin123 --language=en_US --currency=USD \
--timezone=America/Chicago --use-rewrites=1

```
## Conclusion

Congratulations! You have successfully installed Magento 2.4.5 on Ubuntu 22.04. You can now access your Magento store through your browser and start customizing it to fit your e-commerce needs. For further customization and optimization, refer to Magento's official documentation and community resources.