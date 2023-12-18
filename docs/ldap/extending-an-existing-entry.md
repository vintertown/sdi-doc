# Extending an existing entry

There are two ways to extend an existing entry. We'll extend our user jsmith with an posixAccount objectClass.
One way would be to right click within the jsmith object and select `Add new value`. Then you can select the value you want to extend your entry with. In our case it would be `posixAccount`. After selecting the value we can click next and choose a proper value. We choose 1000.
The other way would be to select `LDAP` in the toolbar and choose `New LDIF File`. You can paste this code to the `LDIF` file.

```ssh
dn: uid=jsmith,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
changetype: modify
add: objectClass
objectClass: posixAccount
-
add: uidNumber
uidNumber: 1001
-
add: gidNumber
gidNumber: 1001
-
add: homeDirectory
homeDirectory: /home/jsmith
-
```

It is important to set the `dn: ` to the specific entry you want to extend. Furthermore the `changetype: ` should be `modify`.

```ssh
dn: uid=jsmith,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com
changetype: modify
add: objectClass
objectClass: posixAccount
...
```
