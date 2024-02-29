# Set Up

## Prerequesits

We use these guides for installing [Icinga](/acronyms) on our server:

- [www.howtoforge.com/how-to-install-Icinga-2-monitoring-software-on-debian-12](https://www.howtoforge.com/how-to-install-Icinga-2-monitoring-software-on-debian-12/#prerequisites)
- [www.digitalocean.com/community/tutorials/how-to-install-Icinga-and-Icinga-web-on-ubuntu-16-04#step-2-%E2%80%93-installing-the-Icinga-web-interface](https://www.digitalocean.com/community/tutorials/how-to-install-Icinga-and-Icinga-web-on-ubuntu-16-04#step-2-%E2%80%93-installing-the-Icinga-web-interface)

Before starting we have to update apt:

```ssh
 apt update && apt upgrade
```

## Installing MariaDB Server

We want to use [Icinga](/acronyms) with MariaDB. We can install the mariadb-server with the following command:

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

All done! If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

You can enter the MariaDB shell with:

```ssh
sudo mysql || sudo mariadb
```

## Configuring MariaDB

We can create a new table for [Icinga](/acronyms) with a User:

```ssh
sudo mysql
```

Create the table

```ssh
CREATE DATABASE Icinga2;
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
curl -sSL https://packages.Icinga.com/Icinga.key | sudo apt-key add -
```

Now we need to add the repository address to an apt configuration file.

```ssh
nano /etc/apt/sources.list.d/Icinga.list
```

Paste the following code into the file `/etc/apt/sources.list.d/Icinga.list`

```ssh
deb https://packages.Icinga.com/ubuntu Icinga-xenial main
```

Save and close the file, then refresh your package cache:

```ssh
apt-get update
```

Following the addition of the repository, apt-get will proceed to retrieve data, enabling the installation of [Icinga](/acronyms) packages.

```ssh
apt-get install Icinga2 Icinga2-ido-mysql monitoring-plugins -y
```

Again you will be asked for a configuration prompts:

1. Enable Icinga 2’s ido-mysql feature? - YES
2. Configure database for Icinga2-ido-mysql with dbconfig-common? - YES

Now we need to actually enable the [Icinga](/acronyms) database backend:

```ssh
Icinga2 feature enable ido-mysql command
```

Restart Icinga2 to use the new features:

```ssh
systemctl restart Icinga2
```

You can check the status of Icinga2 and your configuration with:

```ssh
systemctl status Icinga2
```

## Install IDO MySQL driver on the Master Server

For Icinga2 to work, it needs a database. For that, we need to install the IDO MySQL driver and set up the database connection.

```ssh
apt install -y Icinga2-ido-mysql
```

You will be asked again for configuration steps:

1. You will be asked to enable the ido-mysql feature. Select Yes to continue.
2. You will be prompted to set up the driver and create a database using the dbconfig-common utility. Select Yes to continue.
3. You will be asked for the MySQL password for the Icinga2 database. Enter the password you configured previously to continue.
4. You will be asked to confirm the password again.

Check the database details in `/etc/Icinga2/features-available/ido-mysql.conf`:

```ssh
/**
 * The db_ido_mysql library implements IDO functionality
 * for MySQL.
 */

library "db_ido_mysql"

object IdoMysqlConnection "ido-mysql" {
  user = "Icinga2",
  password = "sdi",
  host = "localhost",
  database = "Icinga2"
}
```

Restart the Icinga2 Service:

```ssh
systemctl restart Icinga2
```

You can check the status again:

```ssh
systemctl status Icinga2
```

## Configure Icinga2 API

The last step before we can configure the web setup is to adjust [Icinga](/acronyms)'s API. Install [Icinga](/acronyms) Web with `apt-get`:

```ssh
Icinga2 api setup
```

Notice the output that you get. The end should say something like:
`Now restart your Icinga 2 daemon to finish the installation!`

We need a new user with minimal permissions required by [Icinga](/acronyms) Web. Open the api-users.conf file for editing.

```ssh
nano /etc/Icinga2/conf.d/api-users.conf
```

Append the provided code to the end of the file, and be sure to select a robust password for the [API](/acronyms).

```ssh
/** api for Icingaweb2 */
object ApiUser "Icingaweb2" {
  password = "PassWordApiIcingaWeb2"
  permissions = [ "status/query", "actions/*", "objects/modify/*", "objects/query/*" ]
}
```

Make a note of the credentials which will be needed later on to access the website. The Icinga2 API server listens on port 5665 by default.
Restart the service for the changes to be applied:

```ssh
systemctl restart Icinga2
```

Now we can install the Web Interface:

## Prepare Web Setup

Let’s install Icinga Web with apt-get:

```ssh
apt-get install Icingaweb2
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

We should now be able to access the web interface of [Icinga](/acronyms) via this URL:

```ssh
http://fw061.g8.sdi.mi.hdm-stuttgart.de/Icingaweb2/setup
```

In order to start the configuration process we have to create a token first that we can use to get access:

```ssh
Icingacli setup token create
```

Take the token from the output and paste it into the input field `Setup Token` on the website

![Icinga - 1](/media/Icinga/1.png)

As a default configuration we have the monitoring plugin installed. We can leave it like that and click on next.

![Icinga - 2](/media/Icinga/2.png)

We get an overview of the status of systems and configurations. For example which PHP version is installed and which databases are available.

![Icinga - 3](/media/Icinga/3.png)

We can choose the authentication type that we want to use. We could also choose [LDAP](/acronyms) for authentication. Since we have previously configured our database with users, we choose Database. You will be asked to fill in the database credentials on the next page.

![Icinga - 4](/media/Icinga/4.png)

Enter the database credentials you have previously generated. Validate the configuration by clicking the corresponding button. Once the credentials are confirmed, proceed to the next step by clicking `Next`. You will then be prompted to assign a name to the authentication backend.

![Icinga - 5](/media/Icinga/5.png)

Reenter your created credentials.

![Icinga - 6](/media/Icinga/6.png)

Keep the default value and proceed by clicking `Next`. On the following page, you will be prompted to establish an administrator account.

![Icinga - 7](/media/Icinga/7.png)

Input the credentials for your newly created administrator account that you have created in the section `Configuring MariaDB` and proceed by clicking `Next`.

![Icinga - 8](/media/Icinga/8.png)

Click `Next` to proceed. You will be asked to review the configuration on the last page.

![Icinga - 9](/media/Icinga/9.png)

Feel free to revisit any settings by navigating back. Once you are content with the configurations, click `Next` to continue.

![Icinga - 10](/media/Icinga/10.png)

Enter the database credentials that you configured earlier and click on Validate Configuration to confirm the connection. After successful verification, proceed by clicking `Next`. Subsequently, you will be prompted to provide the [API](/acronyms) details.

![Icinga - 11](/media/Icinga/11.png)

Enter the previously generated [API](/acronyms) credentials, set the Host as 127.0.0.1, and click on Validate Configuration to ensure the connection is valid. Once confirmed, proceed by clicking `Next`. In the following step, you'll be prompted to select protected custom variables for enhancing security monitoring.

![Icinga - 12](/media/Icinga/12.png)

Maintain the default values and advance by clicking `Next`. You will be prompted to review the Monitoring configuration. If necessary, you have the option to go back and make changes before finalizing.

![Icinga - 13](/media/Icinga/13.png)

If you are satisfied with the configurations, proceed to finalize the installation by clicking on the `Finish` button.

![Icinga - 14](/media/Icinga/14.png)

After successful completion, click the "Login to [Icinga](/acronyms) Web 2" button to open the login page at (http://fw061.g8.sdi.mi.hdm-stuttgart.de/Icingaweb2/authentication/login).

![Icinga - 15](/media/Icinga/15.png)

You can now log in via [Icinga](/acronyms)'s web interface.

![Icinga - 16](/media/Icinga/16.png)

You can now use [Icinga](/acronyms) on your server!

![Icinga - 17](/media/Icinga/17.png)

You can go to Overview > Services to check the status of the master server similar to the following:

![Icinga - 18](/media/Icinga/18.png)
