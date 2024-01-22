# Backup and recovery / restore

For setting up LDAP on your second machine you can use the instructions listed in our [LDAP Installation Manual](/ldap/set-up/).
To export the data from your `a machine` to your `b machine` you can use Apache Directory Studio. We have registered with the following DN on the a machine `dc=betrayer,dc=com`. By right-clicking on the DN root, we can generate an `ldif` file under export. We save it locally on
our machine and upload it to the `b machine` via scp.

The uploaded file maybe looks something like this.
It is important to delete the head in order to successfully reconfigure it with `slapadd -l /data.ldif`. (`data.ldif` is the exported file) When installing `ldap` beforehand we have already configured our root tree with `dc=betrayer,dc=com`. That is the reason we need to delete the head.

This should be removed

```ssh
dn: dc=betrayer,dc=com
objectClass: dcObject
objectClass: organization
objectClass: top
dc: betrayer
o: betrayer.com
```

We can restore the exported data with `slapadd -l /data.ldif`
This is the file we uploaded

```ssh
dn: dc=betrayer,dc=com
objectClass: dcObject
objectClass: organization
objectClass: top
dc: betrayer
o: betrayer.com

dn: ou=departments,dc=betrayer,dc=com
objectClass: organizationalUnit
objectClass: top
ou: departments

dn: ou=software,ou=departments,dc=betrayer,dc=com
objectClass: organizationalUnit
objectClass: top
ou: software

dn: ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
objectClass: organizationalUnit
objectClass: top
ou: devel

dn: ou=testing,ou=software,ou=departments,dc=betrayer,dc=com
objectClass: organizationalUnit
objectClass: top
ou: testing

dn: ou=financial,ou=departments,dc=betrayer,dc=com
objectClass: organizationalUnit
objectClass: top
ou: financial

dn: uid=jsmith,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
objectClass: top
cn: John Smithy
gidNumber: 100
homeDirectory: /home/jsmith
sn: Smithy
uid: jsmith
uidNumber: 1002
mail: john@smith.com

dn: uid=abean,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
objectClass: top
cn: Albert Bean
gidNumber: 100
homeDirectory: /home/abean
sn: Bean
uid: abean
uidNumber: 1000
mail: albert@bean.com

dn: uid=bein,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
objectClass: top
cn: Bernd Bein
gidNumber: 100
homeDirectory: /home/bbein
sn: Bein
uid: bbein
uid: bein
uidNumber: 1001
```

Now when logging in with Apache Directory Studio on our `b machine`, we can see the same directories we have on our `a machine`.
