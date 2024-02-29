# Functional Checks

## Setup

Install the Nagios plugins with the following commands:

```ssh
sudo apt-get update
sudo apt-get install nagios-plugins
```

Configure [HTTP(s)](/acronyms) for your web server:

```ssh
sudo nano /etc/nagios-plugins/config/http.cfg
```

Add this to the configuration:

```ssh
define command {
    command_name    check_http
    command_line    $USER1$/check_http -H $HOSTADDRESS$ -I $HOSTADDRESS$ -t 30
}
```

## Ping HTTP(s)

You can use the plugin directly to ping the website:

```ssh
/usr/lib/nagios/plugins/check_http -H fw061.g8.sdi.mi.hdm-stuttgart.de -t 30
```

When everything works you should get the following output:

```ssh
HTTP OK: HTTP/1.1 200 OK - 11999 bytes in 0,001 second response time |time=0,001000s;;;0,000000;30,000000 size=11999B;;;0;
```

When shutting down apache with ...

```ssh
systemctl stop apache2
```

... and trying the ping again, we get the following:

```ssh
HTTP CRITICAL - Konnte TCP socket nicht öffnen
```

## Ping LDAP

We can use this command to ping our [LDAP](/acronyms) server:

```ssh
/usr/lib/nagios/plugins/check_ldap -H ldap://vm1.g8.sdi.mi.hdm-stuttgart.de -b "dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -P 'new_password'
```

We get the following output:

```ssh
Could not connect to the server at port 389
```
