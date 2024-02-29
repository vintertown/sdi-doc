# Set up an OpenLdap server

## How to setup and configure an OpenLdap server

First step in installing and setting up an OpenLdap server is to install [slapd](/acronyms) with the command:

```ssh
apt install slapd ldap-utils
```

In our installation we need to reconfigure `slapd` with `dpkg-reconfigure`.

```ssh
dpkg-reconfigure slapd
```

1. First option: We need a start configuration for the database. Therefore we click on `No`.
2. Second option: The DNS domain name is used to generate the base DN of your [LDAP](/acronyms) directory. We use `betrayer.com`, so our base DN will be `dc=betrayer, dc=com` We set the name of our organization to `betrayer.com`.
3. Third option: Now we can choose a password for the administrator LDAP-directory.
4. Fourth option: We want to store the database when it's deleted so we choose `yes`.
5. Fifth option: We also move the old database with `yes`.

As a result we get:

```ssh
Backing up /etc/ldap/slapd.d in /var/backups/slapd-2.5.13+dfsg-5... done.
  Moving old database directory to /var/backups:
  - directory unknown... done.
  Creating initial configuration... done.
  Creating LDAP directory... done.
root@sdi08a:~# dpkg-reconfigure slapd
  Omitting slapd configuration as requested.
root@sdi08a:~# dpkg-reconfigure slapd
  Omitting slapd configuration as requested.
root@sdi08a:~# dpkg-reconfigure slapd
  Backing up /etc/ldap/slapd.d in /var/backups/slapd-2.5.13+dfsg-5... done.
  Moving old database directory to /var/backups:
  - directory unknown... done.
  Creating initial configuration... done.
  Creating LDAP directory... done.
```

The [LDAP](/acronyms) server is configured. We tested the connection using Apache Directory Studio. As a host name we used vm1.g8.sdi.mi.hdm-stuttgart.de. We do not use an encryption method because we do not have a valid certificate.

For authentication we use simple authentication. The string `dc=betrayer,dc=com` is the "domain" for the [LDAP](/acronyms) tree. Since we have registered the admin with the password in the `slapd config` we can use `cn=admin` to authenticate.

## References

1. [www.ubuntu.com/server/docs/service-ldap](https://ubuntu.com/server/docs/service-ldap)