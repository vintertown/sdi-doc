# Populating your DIT

We can use Apache Directory Studio to populate our [DIT](/acronyms). We can create a new entry by right clicking on `dc=betrayer,dc=com`. Then choose `New Entry`. In the first step you can choose `Create entry from scratch`. For the available object classes please choose `organizationalUnit`. The object class `top` should have been generated automatically. The third step is to configure a distinguished name. As a parent choose your upper layer (for example for ou=departments, the upper layer is dc=betrayer,dc=com). Then you can complete these steps by clicking `finish`.

After successfully setting up all organizational units we can implement `inetOrgPerson`.

## Exporting the ldap tree

We can create a dumb of the ldap tree by using the following command:

```ssh
/etc/ldap# ldapsearch -x -H ldap://vm1.g8.sdi.mi.hdm-stuttgart.de -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" -s sub > ldap_export.ldif
```

The dumb should look like:

```ssh
LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# betrayer.com
dn: dc=betrayer,dc=com
objectClass: top
objectClass: dcObject
objectClass: organization
o: betrayer.com
dc: betrayer

# departments, betrayer.com
dn: ou=departments,dc=betrayer,dc=com
ou: departments
objectClass: organizationalUnit
objectClass: top

# software, departments, betrayer.com
dn: ou=software,ou=departments,dc=betrayer,dc=com
ou: software
objectClass: organizationalUnit
objectClass: top

# devel, software, departments, betrayer.com
dn: ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
ou: devel
objectClass: organizationalUnit
objectClass: top

# testing, software, departments, betrayer.com
dn: ou=testing,ou=software,ou=departments,dc=betrayer,dc=com
ou: testing
objectClass: organizationalUnit
objectClass: top

# financial, departments, betrayer.com
dn: ou=financial,ou=departments,dc=betrayer,dc=com
ou: financial
objectClass: organizationalUnit
objectClass: top

# jsmith, devel, software, departments, betrayer.com
dn: uid=jsmith,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: jsmith
sn: Smith
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
cn: John Smith
mail: john@smith.com

# abean, devel, software, departments, betrayer.com
dn: uid=abean,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: abean
cn: Albert Bean
sn: Bean
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
mail: albert@bean.com

# search result
search: 2
result: 0 Success

# numResponses: 9
# numEntries: 8
```
