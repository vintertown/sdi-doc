# LDAP authentication

## Motivation

We want to limit the manual directory by providing a [LDAP](/acronyms) based authentication.

## 1. Step: Create a test user

We create a test user `tuser` with Apache Directory Studio.

![New Entry](/media/new_entry.jpeg)

For security set a SMD5 hashed password.

![SMD5](/media/smd5.jpeg)

We can now check the bind configuration

![Check Authentication](/media/check_authentication.jpeg)

In the /etc/apache2/sites-available/manual.conf we can add the LDAP Directory Tree for authentication with the `tuser` credentials.

```ssh
<VirtualHost *:80>
  ServerAdmin fw061@hdm-stuttgart.de
  DocumentRoot /usr/share/doc/apache2-doc/manual
  ServerName g8.sdi.mi.hdm-stuttgart.de
  ServerAlias manual.g8.sdi.mi.hdm-stuttgart.de
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
 <Directory "/usr/share/doc/apache2-doc/manual">
      Options Indexes FollowSymlinks
      AuthType Basic
      AuthName "LDAP Authentication"
      AuthBasicAuthoritative Off
      AuthBasicProvider ldap
      AuthLDAPURL "ldap://vm1.g8.sdi.mi.hdm-stuttgart.de/uid=tuser,ou=devel,ou=software,ou=departments,dc=bet>
      AuthLDAPBindDN "uid=tuser,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com"
      AuthLDAPBindPassword sdi
      Require valid-user
  </Directory>
</VirtualHost>
```

We should restart the apache server again:

```ssh
systemctl restart apache2
```

![LDAP Authentication](/media/ldap_authentication.jpeg)
