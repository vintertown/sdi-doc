# Filter based search

We use `ldapsearch` to perform search operations on [LDAP](/acronyms).

## All users with a uid attribute value starting with the letter “b”

```ssh
ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=example,dc=com" "(uid=b*)"
```

returns this answer:

```ssh
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=example,dc=com> with scope subtree
# filter: (uid=b*)
# requesting: ALL
#

# search result
search: 2
result: 32 No such object

# numResponses: 1
root@sdi08a:~# ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(uid=b*)"
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (uid=b*)
# requesting: ALL
#

# bein, devel, software, departments, betrayer.com
dn: uid=bein,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: bbein
uid: bein
cn: Bernd Bein
sn: Bein
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

## All entries with either a defined uid attribute or a ou attribute starting with letter “d”

```ssh
ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(|(uid=*)(ou=d*))"
```
Answer:
```ssh
# extended LDIF
#
# LDAPv3
# base <dc=example,dc=com> with scope subtree
# filter: (uid=b*)
# requesting: ALL
#

# search result
search: 2
result: 32 No such object

# numResponses: 1
root@sdi08a:~# ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(uid=b*)"
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (uid=b*)
# requesting: ALL
#

# bein, devel, software, departments, betrayer.com
dn: uid=bein,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: bbein
uid: bein
cn: Bernd Bein
sn: Bein
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
root@sdi08a:~# ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(|(uid=*)(ou=d*))"
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (|(uid=*)(ou=d*))
# requesting: ALL
#

# departments, betrayer.com
dn: ou=departments,dc=betrayer,dc=com
ou: departments
objectClass: organizationalUnit
objectClass: top

# devel, software, departments, betrayer.com
dn: ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
ou: devel
objectClass: organizationalUnit
objectClass: top

# jsmith, devel, software, departments, betrayer.com
dn: uid=jsmith,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
objectClass: posixAccount
mail: john@smith.com
sn: Smithy
cn: John Smithy
uid: jsmith
uidNumber: 1001
gidNumber: 1001
homeDirectory: /home/jsmith

# abean, devel, software, departments, betrayer.com
dn: uid=abean,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: abean
cn: Albert Bean
sn: Bean
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
objectClass: posixAccount
mail: albert@bean.com
userPassword:: e1NNRDV9aHVZTzQwUSthNDFuY2JlbS90NWlxNUFMYU1Iaml3aTU=
uidNumber: 1
gidNumber: 1
homeDirectory: /home/abean

# bein, devel, software, departments, betrayer.com
dn: uid=bein,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: bbein
uid: bein
cn: Bernd Bein
sn: Bein
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top

# search result
search: 2
result: 0 Success

# numResponses: 6
# numEntries: 5
```

## All users entries within the whole DIT having a gidNumber value of 100

```ssh
ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(gidNumber=100)"
```

Our gid numbers start at 1000 so this should not return anything.

```ssh
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (gidNumber=100)
# requesting: ALL
#

# search result
search: 2
result: 0 Success

# numResponses: 1
```

## All user entries belonging to the billing department having a uidNumber value greater than 1023

```ssh
ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "ou=billing,dc=betrayer,dc=com" "(uidNumber>=1023)"
```

We do not have a department `billing` so this search query cannot be resolved.
Answer:

```ssh
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <ou=billing,dc=betrayer,dc=com> with scope subtree
# filter: (uidNumber>=1023)
# requesting: ALL
#

# search result
search: 2
result: 32 No such object
matchedDN: dc=betrayer,dc=com

# numResponses: 1
```

## All user entries within the whole DIT having a commonName containing the substring “ei”

```ssh
ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(cn=*ei*)"
```

Answer:

```ssh
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (cn=*ei*)
# requesting: ALL
#

# bein, devel, software, departments, betrayer.com
dn: uid=bein,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
uid: bbein
uid: bein
cn: Bernd Bein
sn: Bein
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: top
objectClass: posixAccount
gidNumber: 1024
homeDirectory: /home/bbein
uidNumber: 1024

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

## All user entries within the whole DIT belonging to gidNumber == 100 or having a uid value starting with letter “t”

```ssh
ldapsearch -x -D "cn=admin,dc=betrayer,dc=com" -W -b "dc=betrayer,dc=com" "(|(gidNumber=100)(uid=t*))"
```

Answer:

```ssh
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <dc=betrayer,dc=com> with scope subtree
# filter: (|(gidNumber=100)(uid=t*))
# requesting: ALL
#

# search result
search: 2
result: 0 Success

# numResponses: 1
```

## References
1. [www.docs.oracle.com/cd/E19693-01/819-0997/auto45/index.html](https://docs.oracle.com/cd/E19693-01/819-0997/auto45/index.html)