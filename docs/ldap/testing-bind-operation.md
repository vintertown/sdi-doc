# Testing a bind operation as non-admin user

First step is to set a password. We can achieve this by right clicking on the side uid e.g. `uid=abean`.
You can select `New Attribute` on the top bar and choose as a attribute type `userPassword` from the selectable.
Select a new password and a suitable hash method for encrypting the password. We used for example `SMD5`.
Important: Some password hash types may not be supported. We used SHA beforehand and the configuration did not work.
To configure a second connection click on `LDAP ` / `New Connection`.
We name it for example `LDAP Server abean`. Use the correct password and the correct binding parameters. For our user `uid=abean` we had to use: `uid=abean,ou=devel,ou=software,ou=departments,dc=betrayer,dc=com`
