# Set Up

## Installing Nextcloud on Apache

We use this guide as a reference to carry out all the necessary installations and preparations. <https://help.nextcloud.com/t/complete-nc-installation-on-debian-9-stretch-and-manual-update/21881>

First we need to install all the packages for apache and php. We exclucde the MariaDB package, because we are already using MySQL from [this chapter](/apache/mysql-database).

```ssh
apt install php-gd php-json php-mysql php-curl
apt install php-intl php-mcrypt php-imagick
apt install php-zip php-xmlwriter php-xmlreader php-xml php-mbstring php-simplexml
```

Now we need to download the latest nextcloud version.

```ssh
wget https://download.nextcloud.com/server/releases/latest.zip
unzip latest.zip
mv nextcloud/ /var/www
```

To check if the installation was succesful, we can go to the nexcloud folder. We are getting this output.

```ssh
root@sdi08a:~# cd /var/www/nextcloud/
root@sdi08a:/var/www/nextcloud# ls
3rdparty       composer.lock  core        index.php  ocs-provider       remote.php  themes
apps           config         cron.php    lib        package.json       resources   updater
AUTHORS        console.php    dist        occ        package-lock.json  robots.txt  version.php
composer.json  COPYING        index.html  ocs        public.php         status.php
```

In the next step, we create an apache configuration file for nextcloud. We can update the existing `fw061.conf` file (which we specified [here](/apache/virtual-hosts)) and create a new alias for the path /nextcloud, so that Nextcloud can be reached via <http://fw061.g8.sdi.mi.hdm-stuttgart.de/nextcloud>. We add this to the `fw061.conf` file.

```ssh
 Alias /nextcloud /var/www/nextcloud

  <Directory /var/www/nextcloud>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
  </Directory>
```

Then enable the following `apache2`modules, setup folder permissions and restart `apache2` and `mysql`.

```ssh
a2enmod rewrite
a2enmod headers
a2enmod env
a2enmod dir
a2enmod mime

chown -R www-data /var/www/nextcloud/

systemctl restart apache2
systemctl restart mysql
```

Next we create an empty database for nextcloud and communicate with the MariaDB monitor. This is the output we get.

```ssh
root@sdi08a:~# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 33
Server version: 10.11.6-MariaDB-0+deb12u1 Debian 12

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CREATE DATABASE nextcloud;
Query OK, 1 row affected (0,001 sec)

MariaDB [(none)]> GRANT ALL ON nextcloud.* to 'nextcloud'@'localhost' IDENTIFIED BY 'sdi';
Query OK, 0 rows affected (0,002 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0,001 sec)

MariaDB [(none)]> exit
Bye
```

And then install nextcloud and connect it to the database. Ensure that you are in the correct folder. (`/var/www/nextcloud`)

```ssh
sudo -u www-data php occ maintenance:install --database "mysql" --database-name "nextcloud" --database-user "nextcloud" --database-pass "sdi" --admin-user "ncadmin" --admin-pass "sdi"
```

This should come as an output when it is installed correctly.

```ssh
Nextcloud was successfully installed
```

The last step is to add the `fw061.g8.sdi.mi.hdm-stuttgart.de` to the trusted domain section. We do this by updating the `/var/www/nextcloud/config/config.php` file with our domain. The first section should now loo like this.

```ssh
 array (
    0 => 'localhost',
    1 => 'fw061.g8.sdi.mi.hdm-stuttgart.de',
  ),
```

We restart Apache one more time with `systemctl restart apache2` and are now able to reach the Log-In Screen. We log in with the admin credentials and can now see the Nextcloud Hub.

![Nextcloud Log-In Screen](/media/nextcloud-login.png)

And the Nexcloud Dashboard.

![Nextcloud Log-In Screen](/media/nextcloud-dashboard.png)

## Access with LDAP HdM credentials
