# LDAP configuration

Some parameters have to be added manually to the [LDAP](/acronyms) configuration file. OpenLdap supports parameter configuration within its own database backend.

## Configuration with Apache Directory Studio

First we have to search for the two different [DIT](/acronyms)s. We do this by using the following ldapsearch command:

```ssh
ldapsearch -Y EXTERNAL -H ldapi://` -b cn=config
```

Now we see following answer in the console:

```ssh
# {0}config, config
dn: olcDatabase={0}config,cn=config
objectClass: olcDatabaseConfig
olcDatabase: {0}config
olcAccess: {0}to * by dn.exact=gidNumber=0+uidNumber=0,cn=peercred,cn=external
 ,cn=auth manage by * break
olcRootDN: cn=admin,cn=config

```

At the end of the of the answer we see our required cn=config database! To access the config database via ApacheDirectoryStudio, we need to set an `olcRootPW` attribute, the hashed password for the complete credentials. We do this by hashing our `SSHA` password. We can do this for example like this:

Generate Password Hash with salt.

```ssh
echo -n passwordsalt | shasum -a 1 | awk '{print $1}'
```

Base64 encode it with the salt again appended to the string.

```ssh
echo -n 'c88e9c67041a74e0357befdff93f87dde0904214salt' | base64
```

Now we just have to add {SSHA} before the string. You can find the documentation for this method here: <https://community.canvaslms.com/t5/SIS-User-Articles/SSHA-Password-Generation/ta-p/243730>

Creating a [LDIF](/acronyms) file with the corresponding parameters: (name it like this for Example add_olcRootPW.ldif )

```ssh
dn: olcDatabase={0}config,cn=config
add: olcRootPW
olcRootPW: {ssha}dn: olcDatabase={0}config,cn=config
add: olcRootPW
olcRootPW: {ssha}4yjYu6pKazMWjyCBk7unVBAa3RGlg9oW
```

Now we can add the generated olcRootPW attribute with the following command:

```ssh
ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f ~/add_olcRootPW.ldif
```

To test it we can check the database config again with:

```ssh
ldapsearch -Y EXTERNAL -H ldapi:/// -b cn=config
```

Now we can see the added hashed password.

To connect with ApacheDirectoryStudio we need to uncheck the Box `Get base DNs from Root DSEGet base DNs from Root DSE`. Above we now can add the required Base DN manually. (cn=config). As authentication we use the bind dn `cn=admin,cn=config` and the password which we hashed.
