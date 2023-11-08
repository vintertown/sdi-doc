# Installing and configuring Bind

## 1. Step

Update apt and install the bind service:

```ssh
apt update
apt install bind9 bind9utils bind9-doc
```

To check whether the installation was successful run the following command:

```ssh
named -v
```

Check whether the system is active and running:

```ssh
systemctl status bind9
```

Edit the file to disable recursion.

```ssh
nano /etc/bind/named.conf.options
```

Add the following lines to the named.conf.options file:

```ssh
 // hide version number from clients for security reasons.
 version "not currently available";

 // disable recursion on authoritative DNS server.
 recursion no;

 // enable the query log
 querylog yes;

 // disallow zone transfer
 allow-transfer { none; };
```

Restart bind to confirm the set options:

```ssh
systemctl restart bind9
```

To configure zones edit the following file:

```ssh
nano /etc/bind/named.conf.local
```

Add the following parameters:

```ssh
zone "g8.sdia.sdi.mi.hdm-stuttgart.de" {
      type master;
      file "/etc/bind/db.example.com";
      allow-query { any; };
};
```
