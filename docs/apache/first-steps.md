# First Steps

## Installing Apache

To install our server we need the latest version of Apache. We can install the version with the following command:

`aptitude install apache2`

Apache will automatically show a default page if the package has been installed correctly. Since we have already entered our virtual machine in the previous [DNS task](/dns/transfer-dns-configurations), we can call it globally and see the default page.

## Using the default Apache frontend

We have moved the index.html file which was located in the path `/var/www/html to the root folder`. When reloading the web page again, a new default page appears. It shows the Apache and Linux version.

## Hosting own content

We can now host our own index.html page with the following content:

```ssh
<!DOCTYPE html>
<html>
    <head>
        <title>Hello World.</title>
    </head>
    <body>
        <p>Hello World.</p>
    </body>
</html>
```

When moving it to the folder `/var/www/html` and reloading the browser, we can finally see our own website.

## Installing the Apache2 documentation

First we install the apache2 documentation with the following command

```ssh
apt install apache2-doc
```

After installation, you might wonder how to access this documentation via your website. 
To locate the documentation files, you can use the `dpkg` command for package management. By using the `-L` flag, you can list all the files contained within the package. This will help us identify the path to access the documentation. To examine the output that you get with the `dpkg` command we can use the following:
`dpkg -L apache2-doc > files.txt`

This will save the output to a `files.txt`-file. This way we can examine the whole output.

We get the following output:

```ssh
/.
/etc
/etc/apache2
/etc/apache2/conf-available
/etc/apache2/conf-available/apache2-doc.conf
/usr
/usr/share
/usr/share/doc
/usr/share/doc/apache2
/usr/share/doc/apache2/examples
...
```

When taking a close look on the output you can find a conf-available file.
Within this file you will find an Alias that looks like this: `Alias /manual /usr/share/doc/apache2-doc/manual`
This directive tells Apache where to serve the documentation files from in the URL path.
If the configuration is not already enabled, use the `a2enconf` command to enable it, and then restart Apache:

```ssh
a2enconf apache2-doc
systemctl restart apache2
```

Now you should be able to access the manual on `vm1.g8.mi.hdm-stuttgart.de/manual`

## Uploading our own documentation

In order to upload and access our sdi-docs with a browser client, we can use scp with the following command `scp -r site/ root@sdi08a.mi.hdm-stuttgart.de:/home/Sdidoc`.
This uploads the docs to our machine. Now we can adjust the apache configuration file `000-default.conf` (`/etc/apache2/sites-available/`) with:

- Add the following lines to set permissions for apache to enter `/home/...`:

```ssh
<Directory /home/Sdidoc/site>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

- Furthermore we have to set the permissions on our server with the following commands:
  `chown -R www-data:www-data /home/Sdidoc/site`
  `chmod -R 755 /home/Sdidoc/site`

- Restart the apache server with: `systemctl restart apache2`
- Configure an Alias to be able to access the docs via `.../xy001`. In our case we added this line: `Alias /fw061 /home/Sdidoc/site`

Our apache config file looks like this in the end:

```ssh
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        Alias /fw061 /home/Sdidoc/site
        <Directory /home/Sdidoc/site>
            Options Indexes FollowSymLinks
            AllowOverride None
            Require all granted
        </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
