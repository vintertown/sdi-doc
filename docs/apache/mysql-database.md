# MySQL™ database administration

## Motivation

We want to install a MySQL Server on our Apache Webserver and access it with PhpMyAdmin.

## Install MySQL

First we can install the default-mysql-server.

```ssh
apt install mysql-server
```

and start the server

```ssh
systemctl start mysql
```

To see whether everything works correctly we can use.

```ssh
systemctl status mysql
```

It provides us the following feedback.

```ssh
● mariadb.service - MariaDB 10.11.6 database server
     Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-02-19 21:07:45 CET; 1min 9s ago
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
   Main PID: 52008 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 9 (limit: 618984)
     Memory: 81.1M
        CPU: 826ms
     CGroup: /system.slice/mariadb.service
             └─52008 /usr/sbin/mariadbd
```

When the installation successfully finishes it provides the following output:

```ssh
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] n
 ... skipping.

By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] n
 ... skipping.

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Install phpmyadmin

Install phpmyadmin with the following command:

```ssh
apt install phpmyadmin
```

Furthermore we have to use the following package in order to install php. We need php for phpmyadmin.

```ssh
apt install libapache2-mod-php
```

We encountered the issue that no config files were created under /etc/apache2/conf-enabled. We fixed the problem with manually creating the config files in /etc/apache2/conf-enabled/phpmyadmin.conf and /etc/apache2/conf-available/phpmyadmin.conf. Write the following code into your config files.

```ssh
Alias /phpmyadmin /usr/share/phpmyadmin
<Directory /usr/share/phpmyadmin>
    Options FollowSymLinks
    DirectoryIndex index.php
    Require all granted
</directory>
```

We also use a link to the /etc/apache2/conf-available with `ln -s /etc/apache2/conf-available/phpmyadmin.conf /etc/apache2/conf-enabled/phpmyadmin.conf`.

Using the `dpkg-reconfigure phpmyadmin` command, we can select the web server we are using (such as Apache) and enter the MySQL credentials that were specified earlier. This step configures phpMyAdmin to connect to the MySQL server. After completing this configuration, phpMyAdmin will be able to successfully establish a connection to the MySQL server, allowing for database management through its web interface.

Important: Restart apache before trying to reach your server in the browser.

```ssh
systemctl restart apache2
```

<!-- ```ssh
Login:
phpmyadmin@localhost

Password:
sdi
``` -->

When everything is correctly set up you can go to the phpmyadmin website and log in with your credentials.
![Check Authentication](/media/phpmyadmin.jpeg)
When successfully logged in you get to this screen.
![Check Authentication](/media/phpmyadmin_logged_in.jpeg)
