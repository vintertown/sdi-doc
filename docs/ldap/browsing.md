# Browse an existing LDAP Server

To browse directories with LDAP we use [Apache Directory Studio](https://directory.apache.org/studio/).

## How to browse an existing LDAP Server?

To configure a new connection we can open `LDAP` / `New LDAP Connection`. We use the LDAP hdm server ldap1.hdm-stuttgart.de and connect to it via TLS. We use the `No Authentication` Method to get a general view of the ldap server. Make sure, that we are using the MI VPN.

Now we are seeing the LDAP DIT and can navigate through it to see the userlist. If we right click on userlist we can filter the children and look after our user (for Example (uid=nv023)). Now we are seeing the following information for the user. (see picture)

To get an extended view of our user (for example to see the hash) We can right click on our connection and click on ``Properties`. Now we can use the `Simple Authentification` Method. We can get our DN credentials from or user on the top (see picture) and authenticate with the corresponding password. Now we can see further materials like the Matrikelnummer or the hashed password.

To generate the same with ldapsearch you can use the following promt `ldapsearch -x -H ldap://ldap1.hdm-stuttgart.de -b "ou=userlist,dc=hdm-stuttgart,dc=de" -s sub "(uid=nv023)"`

Here is the output with the same information as with the gui:

```ssh
# extended LDIF
#
# LDAPv3
# base <ou=userlist,dc=hdm-stuttgart,dc=de> with scope subtree
# filter: (uid=nv023)
# requesting: ALL
#

# nv023, userlist, hdm-stuttgart.de
dn: uid=nv023,ou=userlist,dc=hdm-stuttgart,dc=de
displayName: Vinterstad Niklas
employeeType: student
objectClass: hdmAccount
objectClass: hdmStudent
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
objectClass: eduPerson
eduPersonAffiliation: member
eduPersonAffiliation: student
eduPersonAffiliation: library-walk-in
uid: nv023
mail: nv023@hdm-stuttgart.de
uidNumber: 73699
cn: Vinterstad Niklas
loginShell: /bin/sh
hdmCategory: 1
gidNumber: 100
givenName: Niklas
homeDirectory: /home/stud/n/nv023
sn: Vinterstad

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

If you want to authenticate, here is the following promt:
`ldapsearch -x -H ldap://ldap1.hdm-stuttgart.de -b "ou=userlist,dc=hdm-stuttgart,dc=de" -D "uid=nv023,ou=userlist,dc=hdm-stuttgart,dc=de" -W "(uid=nv023)"`

and the follwing output with more information is showing:

```ssh
# extended LDIF
#
# LDAPv3
# base <ou=userlist,dc=hdm-stuttgart,dc=de> with scope subtree
# filter: (uid=nv023)
# requesting: ALL
#

# nv023, userlist, hdm-stuttgart.de
dn: uid=nv023,ou=userlist,dc=hdm-stuttgart,dc=de
businessCategory: 1
employeeType: student
postOfficeBox: 2G
objectClass: hdmAccount
objectClass: hdmStudent
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
objectClass: eduPerson
eduPersonAffiliation: member
eduPersonAffiliation: student
eduPersonAffiliation: library-walk-in
uid: nv023
mail: nv023@hdm-stuttgart.de
uidNumber: 73699
cn: Vinterstad Niklas
loginShell: /bin/sh
hdmCategory: 1
gidNumber: 100
employeeNumber: 45516
givenName: Niklas
homeDirectory: /home/stud/n/nv023
sn: Vinterstad
matrikelNr: 45516
userPassword:: e1NTSEF9TVBSZzZQQTJGMUxUKzVrOFM5Y2RnRlVjbmtaUm1iVlh5UERoTkhYdEx
 3UHZ0dDEw
shadowLastChange: 19272
sambaNTPassword: EEB75997E36EFFC420B8BFC8EB8E6CF6

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

```
