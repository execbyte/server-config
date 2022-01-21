# SSH
---
Interactive terminal
```bash
$ export TERM=ansi
```

# CA - HTTPS
----

1. Apache

# Roundcube
---
1. Database `mariadb-server`
```bash
$ su -
# mysql
MariaDB [(none)]> create database roundcubedb;
MariaDB [(none)]> grant all privileges on roundcubedb.* to 'roundcube'@'localhost' identified by '1';
MariaDB [(none)]> flush privileges;
```

2. Download Roundcube
```bash
$ su -
# curl https://github.com/roundcube/roundcubemail/releases/download/1.5.2/roundcubemail-1.5.2.tar.gz -L -O
# tar -xzvf roundcubemail-1.5.2.tar.gz
# mv rouncubemail-1.5.2 roundcube /var/www/html/roundcube
# chown -R root:root /var/www/html/roundcube
```

3. PHP - extension
```bash
$ su -
# apt install php libapache2-mod-php php-mysql php-mbstring php-intl php-pear
# nano /etc/php/7.3/apache2/php.ini
extension=dom.so
extension=mbstring
extension=mysqli
extension=intl
# systemctl restart apache2.service
```

4. PHP - extension ( OPTIONAL BUT RECOMMENDED )
```bash
$ su -
# apt install php-curl php-gd php-imagick php-zip php-ldap php-auth-sasl php-net-smtp php-mail-mime
# nano /etc/php/7.3/apache2/php.ini 
extension=curl
extension=ldap
date.timezone = UTC
# apt install composer
# composer require guzzlehttp/guzzle
# systemctl restart apache2.service
```

4. Set Up Roundcube DB
```bash
$ su -
# cd /var/www/html/roundcube/SQL
# mysql -u roundcube -p roundcubedb < mysql.initial.sql
# mysql -u roundcube -p
MariaDB [(none)]> show databases;
MariaDB [(none)]> use roundcubedb;
MariaDB [roundcubedb]> show tables;
```

5. Test browser
	- `https://domain.com/installer`
	- 