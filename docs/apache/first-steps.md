# First Steps

To install our server we need the latest version of Apache. We can install the version with the following command:

`aptitude install apache2`

Apache will automatically show a default page if the package has been installed correctly. Since we have already entered our VM in the previous DNS task, we can call it globally and see the default page.

We have moved the index.html file which was located in the path /var/www/html to the root folder. When reloading the web page again, a new default page appears. It shows the Apache and Linux version (fig.1).

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

To examine the output that you get with the `dpkg` command we can do the following:
`dpkg -L apache2-doc > files.txt`

This will save the output to a `files.txt`-file. This way we can examine the whole output. Without doing so, it would be not possible to see everything within your console.

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
If the configuration is not already enabled, use the a2enconf command to enable it, and then restart Apache:

```ssh
a2enconf apache2-doc
systemctl restart apache2
```

Now you should be able to access the manuals on `your-url.de/manual`
