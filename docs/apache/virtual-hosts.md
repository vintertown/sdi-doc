# Virtual Hosts

## Define DNS aliases

For the sake of simplicity, we have created 2 A records on the nssdi.mi.hdm-stuttgart.de server. One for `fw061` and one for `manual` which both point to our server. Now we can access the server publicly via fw061.g8.sdi.mi.hdm-stuttgart.de and manual.g8.sdi.mi.hdm-stuttgart.de. You can find access to the process [here](/dns/transfer-dns-configurations).

## Creating config files for new subdomains

In the main config file `etc/apache2/apache2.conf`, we are adding the follwing instructions:

```ssh
<Directory /home/Sdidoc/site>
        AllowOverride None
        Require all granted
        Options Indexes FollowSymLinks
</Directory>
```

Now we have to create 2 separate virtual hosts for the two subdomains. We create 2 new .conf files under the path `/etc/apache2/sites-available`. One file `fw061.conf` and one `manual.conf`. Within the `fw061.conf` we define:

```ssh
<VirtualHost *:80>
  ServerAdmin fw061@hdm-stuttgart.de
  DocumentRoot /home/Sdidoc/site
  ServerName g8.sdi.mi.hdm-stuttgart.de
  ServerAlias fw061.g8.sdi.mi.hdm-stuttgart.de
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Within the manual.conf file we define:

```ssh
<VirtualHost *:80>
  ServerAdmin fw061@hdm-stuttgart.de
  DocumentRoot /usr/share/doc/apache2-doc/manual
  ServerName g8.sdi.mi.hdm-stuttgart.de
  ServerAlias manual.g8.sdi.mi.hdm-stuttgart.de
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

The config files must be activated with this command:

```ssh
a2ensite /etc/apache2/sites-available/fw061.conf
a2ensite/etc/apache2/sites-available/manual.conf
```

and reload the Server with `systemctl reload apache2`.

## Checking the domains

To check whether the server configurations are working properly you can check the specified domains in your web browser. In our case they should point to the documentation and the manual.
`fw061.g8.sdi.mi.hdm-stuttgart.de`
`manual.g8.sdi.mi.hdm-stuttgart.de`
