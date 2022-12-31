# Install Apache2, MySQL/MariaDB, PHP, PHPMyAdmin, Webite hosting With SSL

> Note: This document is for beginners. I've also added seprate links for details. Always welcome for contributions.

Before any installation. Just run ```sudo apt-get update``` or ```sudo apt update``` to update the Ubuntu/Debian package list.

## Install Apache, MySQL/MariaDB, PHP (LAMP) stack on Ubuntu

First of all, if you doesn't have ```nano``` or ```vim``` editor installed in Lunix. You can install my with commands:

### Install VIM

```install vim command
sudo apt update
sudo apt install vim
```

### Install Nano

```install nano command
sudo apt update
sudo apt install nano
```

I'll be using nano editor.

## Installation of Firewall

If you doesn't have firewall installed:
For more: [How To Set Up a Firewall with UFW on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-22-04)

### Install Firewall

```install firewall command
sudo apt update
sudo apt install ufw
```

---

1. Apache Server
2. MySQL/MariabDB Server

## Install Apache Server

For more: [How To Install the Apache Web Server on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-22-04)

```install apache server
sudo apt update
sudo apt install apache2 apache2-utils
```

Now add apache in firewall list. Use following command to show the list of allowed protocols.

```text
sudo ufw app list
```

Add following protocols in firewall list

```add apache in firewall list
sudo ufw allow in "Apache"
sudo ufw allow in "Apache Full"
sudo ufw allow in "Apache Secure"
sudo ufw allow in "OpenSSH"
```

Enable the firewall

```text
sudo ufw status
sudo ufw enable
```

For apache test:

```command
sudo apachectl configtest
```

Output:

```output
AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
Syntax OK
```

For this: Open **apache2.conf** in **nano** editor by ```sudo nano /etc/apache2/apache2.conf``` and add the ```ServerName 127.0.0.1``` line to the end of the file and then reload apache2 server.

```command
sudo systemctl reload apache2.service
```

## Install MySQL/MariaDB

>Note: MySQL and MariaDB has same installation process.

For more: [How To Install MySQL on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-22-04)

```command
sudo apt install mysql-server
sudo mysql_secure_installation
```

For more: [How To Install MariaDB on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-22-04)

```command
sudo apt install mariadb-server
sudo mysql_secure_installation
```

If it throw fatal change password error, Open the terminal application. Terminate the mysql_secure_installation from another terminal using the killall command:

```command
sudo killall -9 mysql_secure_installation
```

Start the mysql client:

```command
sudo mysql
```

Run the following SQL query:

```command
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
exit
```

Then run the following command to secure it:

```command
sudo mysql_secure_installation
```

When promoted for the password enter the SetRootPasswordHere (or whatever you set when you ran the above SQL query) That is all. Then Allow mysql remote access.

>Add Mysql Rule to firewall

```command
sudo ufw allow 3306
```

> To enable the remote access of MySQL/MariaDB on the internet.

```command
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Search for this:

```output
. . .
lc-messages-dir = /usr/share/mysql
skip-external-locking
#
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address = 127.0.0.1
. . .
```

Change bind-address from ```127.0.0.1 -> 0.0.0.0``` and restart server:

```command
sudo systemctl restart mysql
```

For Remote Access User

```command
sudo mysql

UserforAll(localhost)
CREATE USER 'username'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost';
FLUSH PRIVILEGES;

exit
```

```command
sudo mysql

(Remote Access of the Server)
CREATE USER 'username'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'username'@'%';
FLUSH PRIVILEGES;
    
exit
```

## Install PHPMYADMIN

For more: [How To Install and Secure phpMyAdmin on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-22-04)

```command
sudo apt update
sudo apt install phpmyadmin
```

Assuming that you've already installed MySQL/MariaDB, you may have decided to enable the Validate Password plugin. As of this writing, enabling this component will trigger an error when you attempt to set a password for the phpmyadmin user: ```select abort```

```command
sudo mysql
mysql -u root -p
UNINSTALL COMPONENT "file://component_validate_password";
exit
```

Now again install phpmyadmin

```command
sudo apt install phpmyadmin
sudo mysql
mysql -u root -p
INSTALL COMPONENT "file://component_validate_password";
exit
```

After that run following commands:

```command
sudo phpenmod mbstring
sudo systemctl restart apache2
```

Now we have to set password, you can use a new one or set previous password as well

```command
sudo mysql
sudo mysql -u root -p
```

```command
SELECT user,authentication_string,plugin,host FROM mysql.user;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
```

```command
SELECT user,authentication_string,plugin,host FROM mysql.user;
```

> Do this otherwise you phpmyadmin link will not work

```command
sudo nano /etc/apache2/apache2.conf
```

end of the file Include ```/etc/phpmyadmin/apache.conf``` and then ```/etc/init.d/apache2 restart```

## Install PHP (Latest Version)

```command
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
```

```command
sudo apt update
```

For PHP 8.2

```command
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-opcache libapache2-mod-php8.2 php8.2-mysql php8.2-zip php8.2-gd php8.2-mbstring php8.2-xml php8.2-bcmath php8.2-bz2 php8.2-intl php8.2-gmp php-json php-pear php8.2-curl php8.2-dev php8.2-xdebug php8.2-pgsql php8.2-fpm php8.2-redis php8.2-intl
```

For PHP 8.1

```command
sudo apt install php8.1 php8.1-cli php8.1-common php8.1-opcache libapache2-mod-php8.1 php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-xml php8.1-bcmath php8.1-bz2 php8.1-intl php8.1-gmp php-json php-pear php8.1-curl php8.1-dev php8.1-xdebug php8.1-pgsql php8.1-fpm php8.1-redis php8.1-intl
```

For PHP 7.4

```command
sudo apt install php7.4 php7.4-cli php7.4-common php7.4-opcache libapache2-mod-php7.4 php7.4-mysql php7.4-zip php7.4-gd php7.4-mbstring php7.4-xml php7.4-bcmath php7.4-bz2 php7.4-intl php7.4-gmp php-json php-pear php7.4-curl php7.4-dev php7.4-xdebug php7.4-pgsql php7.4-fpm php7.4-redis php7.4-intl
```

> To change the php versions of the server

```command
sudo update-alternatives --config php
```

and for Apache2

```command
sudo a2dismod php7.1
sudo a2enmod php5.6
sudo service apache2 restart
```

and then restart apache2 server: ```sudo systemctl restart apache2```

## Setup site

I'm a Laravel Developer that's why I've installed Apache server instead of Ngix. Be my guest to write this document for Ngix as well.

## Laravel Webiste

### For Apache2 Server

> It is recommended use ```your_domain.com``` with ```.com``` or any extension you have.

```command
mkdir /var/www/your_domain.com
```

```command
sudo chown -R $USER:$USER /var/www/your_domain.com
```

```command
sudo nano /etc/apache2/sites-available/your_domain.com.conf

<VirtualHost *:80>
    ServerName your_domain.com
    ServerAlias www.your_domain.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain.com/public
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

To save the document: ```Ctrl+S``` and for Exit: ```Ctrl+X```

```command
sudo a2ensite your_domain.com.conf
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

Clone/Copy site to folder

```command
git clone git@github.com:haroon-mahmood-4276/your_domain.git your_domain.com
```

Now install Composer & setup .env

#### Install Composer

```command
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```

```command
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

```command
php composer-setup.php
```

```command
php -r "unlink('composer-setup.php');"
```

Most likely, you want to put the ```composer.phar``` into a directory on your PATH, so you can simply call ```composer``` from any directory (Global install), using for example:

```command
sudo mv composer.phar /usr/local/bin/composer
```

For laravel folder permissions

```command
sudo chown -R $USER:www-data storage
sudo chown -R $USER:www-data bootstrap/cache
chmod -R 775 storage
chmod -R 775 bootstrap/cache
```

Some Apache2 Configuration

```command
sudo nano /etc/apache2/apache2.conf
```

```command
<Directory /var/www/html/>
    Options Indexes FollowSymLinks
    AllowOverride all
    Require all granted
</Directory>
```

```command
sudo service apache2 restart
```

## Install SSL

```command
sudo apt update
sudo apt install snapd
sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

```command
sudo certbot --apache -d your_domain.com -d www.your_domain.com
sudo certbot renew --dry-run
```
