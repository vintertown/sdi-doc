# Set up an OpenLdap server

## How to setup and configure an OpenLdap server

First step in installing and setting up an OpenLdap server is to install [slapd](/acronyms) with the command:

```ssh
apt install slapd ldap-utils
```

We need to reconfigure [DIT](/acronyms)

```ssh
dpkg-reconfigure slapd
```

After going through the installation steps we have created this file:

```ssh
/etc/ldap/ldap.conf
```

After the creation you can set the following instructions within file:

```ssh
BASE dc=betrayer,dc=com
URI ldap://ldap01.mi.hdm-stuttgart.de
```
