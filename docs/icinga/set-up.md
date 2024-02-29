# Set Up

## Prerequesits

We use these guides for installing Icinga on our server:

- [www.howtoforge.com/how-to-install-icinga-2-monitoring-software-on-debian-12](https://www.howtoforge.com/how-to-install-icinga-2-monitoring-software-on-debian-12/#prerequisites)
- [www.digitalocean.com/community/tutorials/how-to-install-icinga-and-icinga-web-on-ubuntu-16-04#step-2-%E2%80%93-installing-the-icinga-web-interface](https://www.digitalocean.com/community/tutorials/how-to-install-icinga-and-icinga-web-on-ubuntu-16-04#step-2-%E2%80%93-installing-the-icinga-web-interface)

Before starting we have to update apt:

```ssh
 apt update && apt upgrade
```

## Installing MariaDB Server

We want to use Icinga with MariaDB. We can install the mariadb-server with the following command:

```ssh
apt install mariadb-server
```

Afterwards we can install the secure script:

```ssh
mariadb-secure-installation
```

We are being asked for a password. Since we do not have one configured yet we can press `enter`.
The secure installation guide will ask you multiple questions to your configuration:

```ssh
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

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

You can enter the MariaDB shell with:

```ssh
sudo mysql || sudo mariadb
```

## Configuring MariaDB

We can create a new table for Icinga with a User:

```ssh
sudo mysql
```

Create the table

```ssh
CREATE DATABASE icinga2;
```

Create a User Account for Icinga2

```ssh
CREATE USER 'fw061'@'localhost' IDENTIFIED BY 'sdi';
```

We can grant full access for the user:

```ssh
GRANT ALL PRIVILEGES ON fw061.* TO 'fw061'@'localhost';
```

We can create another SQL user for performing administrative tasks that employ password authentication.

```ssh
GRANT ALL ON *.* TO 'fw061_2'@'localhost' IDENTIFIED BY 'sdi' WITH GRANT OPTION;
```

Flush the user privileges:

```ssh
FLUSH PRIVILEGES;
```

Exit the shell:

```ssh
exit
```

## Install Icinga2 and Monitoring plugins on the Master Server

First, download the Icinga developers’ package signing key and add it to the apt system:

```ssh
curl -sSL https://packages.icinga.com/icinga.key | sudo apt-key add -
```

Now we need to add the repository address to an apt configuration file.

```ssh
nano /etc/apt/sources.list.d/icinga.list
```

Paste the following code into the file `/etc/apt/sources.list.d/icinga.list`

```ssh
deb https://packages.icinga.com/ubuntu icinga-xenial main
```

Save and close the file, then refresh your package cache:

```ssh
apt-get update
```

Following the addition of the repository, apt-get will proceed to retrieve data, enabling the installation of Icinga packages.

```ssh
apt-get install icinga2 icinga2-ido-mysql monitoring-plugins -y
```

Again you will be asked for a configuration prompts:

1. Enable Icinga 2’s ido-mysql feature? - YES
2. Configure database for icinga2-ido-mysql with dbconfig-common? - YES

Now we need to actually enable the Icinga database backend:

```ssh
icinga2 feature enable ido-mysql command
```

Restart icinga2 to use the new features:

```ssh
systemctl restart icinga2
```

You can check the status of icinga2 and your configuration with:

```ssh
systemctl status icinga2
```

## Install IDO MySQL driver on the Master Server

For Icinga2 to work, it needs a database. For that, we need to install the IDO MySQL driver and set up the database connection.

```ssh
apt install -y icinga2-ido-mysql
```

You will be asked again for configuration steps:

1. You will be asked to enable the ido-mysql feature. Select Yes to continue.
2. You will be prompted to set up the driver and create a database using the dbconfig-common utility. Select Yes to continue.
3. You will be asked for the MySQL password for the icinga2 database. Enter the password you configured previously to continue.
4. You will be asked to confirm the password again.

Check the database details in `/etc/icinga2/features-available/ido-mysql.conf`:

```ssh
/**
 * The db_ido_mysql library implements IDO functionality
 * for MySQL.
 */

library "db_ido_mysql"

object IdoMysqlConnection "ido-mysql" {
  user = "icinga2",
  password = "sdi",
  host = "localhost",
  database = "icinga2"
}
```

Restart the Icinga2 Service:

```ssh
systemctl restart icinga2
```

You can check the status again:

```ssh
systemctl status icinga2
```

## Configure Icinga2 API

The last step before we can configure the web setup is to adjust Icinga's API. Install Icinga Web with `apt-get`:

```ssh
icinga2 api setup
```

Notice the output that you get. The end should say something like:
`Now restart your Icinga 2 daemon to finish the installation!`

We need a new user with minimal permissions required by Icinga Web. Open the api-users.conf file for editing.

```ssh
nano /etc/icinga2/conf.d/api-users.conf
```

Append the provided code to the end of the file, and be sure to select a robust password for the API.

```ssh
/** api for icingaweb2 */
object ApiUser "icingaweb2" {
  password = "PassWordApiIcingaWeb2"
  permissions = [ "status/query", "actions/*", "objects/modify/*", "objects/query/*" ]
}
```

Make a note of the credentials which will be needed later on to access the website. The Icinga2 API server listens on port 5665 by default.
Restart the service for the changes to be applied:

```ssh
systemctl restart icinga2
```

Now we can install the Web Interface:

## Prepare Web Setup

Let’s install Icinga Web with apt-get:

```ssh
apt-get install icingaweb2
```

We have to adjust the config file:

```ssh
nano /etc/php/7.0/apache2/php.ini
```

In the file `/etc/php/7.0/apache2/php.ini` we can add the following lines:

```ssh
date.timezone = Europe/Berlin
```

Restart Apache for the changes to take effect:

```ssh
systemctl restart apache2
```

## Setting up IcingaWeb

We should now be able to access the web interface of Icinga via this URL:

```ssh
http://fw061.g8.sdi.mi.hdm-stuttgart.de/icingaweb2/setup
```

In order to start the configuration process we have to create a token first that we can use to get access:

```ssh
icingacli setup token create
```

Take the token from the output and paste it into the input field `Setup Token` on the website

![icinga - 1](/media/icinga/1.png)

As a default configuration we have the monitoring plugin installed. We can leave it like that and click on next.

![icinga - 2](/media/icinga/2.png)

We get an overview of the status of systems and configurations. For example which PHP version is installed and which databases are available.

![icinga - 3](/media/icinga/3.png)

We can choose the authentication type that we want to use. We could also choose LDAP for authentication. Since we have previously configured our database with users, we choose that.

![icinga - 4](/media/icinga/4.png)
